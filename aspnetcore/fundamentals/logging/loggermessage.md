---
title: ASP.NET Core LoggerMessage ile yüksek performanslı günlüğe kaydetme
author: guardrex
description: Yüksek performanslı günlük kaydı senaryoları için daha az nesne ayırmaları gerektiren alınabilir temsilciler oluşturmak için LoggerMessage kullanmayı öğrenin.
manager: wpickett
ms.author: riande
ms.date: 11/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: 5b5bd03b6cb5da693f046653a09ba400ee6ff585
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729200"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="19bfc-103">ASP.NET Core LoggerMessage ile yüksek performanslı günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="19bfc-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="19bfc-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="19bfc-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="19bfc-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) özellikler oluşturmak daha az nesne ayırmaları gerektiren alınabilir Temsilciler ve azaltılmış hesaplama yükünü karşılaştırdığınızda [Günlükçü genişletme yöntemleri](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), gibi `LogInformation`, `LogDebug`, ve `LogError`.</span><span class="sxs-lookup"><span data-stu-id="19bfc-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead compared to [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="19bfc-106">Yüksek performanslı günlük kaydı senaryoları için kullanmak `LoggerMessage` düzeni.</span><span class="sxs-lookup"><span data-stu-id="19bfc-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="19bfc-107">`LoggerMessage` Günlükçü genişletme yöntemleri aşağıdaki performans avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="19bfc-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="19bfc-108">Günlükçü genişletme yöntemleri gerektirir "kutulama (dönüştürme)" değer türleri gibi `int`, içine `object`.</span><span class="sxs-lookup"><span data-stu-id="19bfc-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="19bfc-109">`LoggerMessage` Düzeni statik kullanarak kutulama önler `Action` alanları ve kesin türü belirtilmiş parametrelerle genişletme yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="19bfc-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="19bfc-110">Günlükçü genişletme yöntemleri, her saat bir günlük iletisi yazılır iletisi şablonunu (adlandırılmış biçim dizesi) ayrıştırma gerekir.</span><span class="sxs-lookup"><span data-stu-id="19bfc-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="19bfc-111">`LoggerMessage` yalnızca ileti tanımlandığında şablon kez ayrıştırma gerektirir.</span><span class="sxs-lookup"><span data-stu-id="19bfc-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="19bfc-112">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="19bfc-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="19bfc-113">Örnek uygulamayı gösteren `LoggerMessage` izleme sistemi temel bir teklif özelliklerle.</span><span class="sxs-lookup"><span data-stu-id="19bfc-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="19bfc-114">Uygulama ekler ve bir bellek içi veritabanı kullanarak teklifleri siler.</span><span class="sxs-lookup"><span data-stu-id="19bfc-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="19bfc-115">Bu işlemler ortaya çıktığında günlük iletilerini kullanılarak oluşturulan `LoggerMessage` düzeni.</span><span class="sxs-lookup"><span data-stu-id="19bfc-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="19bfc-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="19bfc-116">LoggerMessage.Define</span></span>

<span data-ttu-id="19bfc-117">[(LogLevel, olay kimliği, dize) tanımlamak](/dotnet/api/microsoft.extensions.logging.loggermessage.define) oluşturur bir `Action` temsilci bir ileti günlüğe kaydetme için.</span><span class="sxs-lookup"><span data-stu-id="19bfc-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="19bfc-118">`Define` aşırı yüklemeleri adlandırılmış biçim dizesine (şablonu) en fazla altı türü parametreleri geçirme izin verir.</span><span class="sxs-lookup"><span data-stu-id="19bfc-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="19bfc-119">Sağlanan dize `Define` yöntemdir bir şablon ve Ara değerli bir dize değil.</span><span class="sxs-lookup"><span data-stu-id="19bfc-119">The string provided to the `Define` method is a template and not an interpolated string.</span></span> <span data-ttu-id="19bfc-120">Yer tutucu türleri belirttiğiniz sırayla doldurulur.</span><span class="sxs-lookup"><span data-stu-id="19bfc-120">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="19bfc-121">Yer tutucu adları şablonundaki şablonlar arasında açıklayıcı ve tutarlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="19bfc-121">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="19bfc-122">Bunlar özellik adları yapılandırılmış günlük verileri içinde işlevini görür.</span><span class="sxs-lookup"><span data-stu-id="19bfc-122">They serve as property names within structured log data.</span></span> <span data-ttu-id="19bfc-123">Öneririz [Pascal büyük/küçük harf](/dotnet/standard/design-guidelines/capitalization-conventions) yer tutucu adları için.</span><span class="sxs-lookup"><span data-stu-id="19bfc-123">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="19bfc-124">Örneğin, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="19bfc-124">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="19bfc-125">Her günlük iletisi bir `Action` tarafından oluşturulan statik bir alana tutulan `LoggerMessage.Define`.</span><span class="sxs-lookup"><span data-stu-id="19bfc-125">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="19bfc-126">Örneğin, örnek uygulamayı dizin sayfası için bir GET isteği için bir günlük iletisi tanımlamak için bir alan oluşturur (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="19bfc-126">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="19bfc-127">İçin `Action`, belirtin:</span><span class="sxs-lookup"><span data-stu-id="19bfc-127">For the `Action`, specify:</span></span>

* <span data-ttu-id="19bfc-128">Günlük düzeyi.</span><span class="sxs-lookup"><span data-stu-id="19bfc-128">The log level.</span></span>
* <span data-ttu-id="19bfc-129">Bir benzersiz olay tanımlayıcısı ([EventID](/dotnet/api/microsoft.extensions.logging.eventid)) statik genişletme yöntemi adı.</span><span class="sxs-lookup"><span data-stu-id="19bfc-129">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="19bfc-130">İleti şablonu (biçim dizesi olarak adlandırılır).</span><span class="sxs-lookup"><span data-stu-id="19bfc-130">The message template (named format string).</span></span> 

<span data-ttu-id="19bfc-131">Örnek uygulama kümelerinin dizin sayfası için bir istek:</span><span class="sxs-lookup"><span data-stu-id="19bfc-131">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="19bfc-132">Günlük düzeyini `Information`.</span><span class="sxs-lookup"><span data-stu-id="19bfc-132">Log level to `Information`.</span></span>
* <span data-ttu-id="19bfc-133">Olay Kimliği `1` adıyla `IndexPageRequested` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="19bfc-133">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="19bfc-134">(Biçim dizesi olarak adlandırılır) ileti şablona bir dize.</span><span class="sxs-lookup"><span data-stu-id="19bfc-134">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="19bfc-135">Günlüğe kaydetme zenginleştirmek için olay kimliği ile sağlandığında yapılandırılmış günlük depoları olay adı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19bfc-135">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="19bfc-136">Örneğin, [Serilog](https://github.com/serilog/serilog-extensions-logging) olay adını kullanır.</span><span class="sxs-lookup"><span data-stu-id="19bfc-136">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="19bfc-137">`Action` Kesin türü belirtilmiş genişletme yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="19bfc-137">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="19bfc-138">`IndexPageRequested` Yöntemi örnek uygulamasında bir dizin sayfası GET isteği için bir ileti kaydeder:</span><span class="sxs-lookup"><span data-stu-id="19bfc-138">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="19bfc-139">`IndexPageRequested` Günlükçü olarak adlandırılan `OnGetAsync` yönteminde *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="19bfc-139">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="19bfc-140">Uygulamanın konsol çıkışı inceleyin:</span><span class="sxs-lookup"><span data-stu-id="19bfc-140">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="19bfc-141">Bir günlük iletisine parametreleri geçirmek için statik alan oluştururken en fazla altı türünü tanımlar.</span><span class="sxs-lookup"><span data-stu-id="19bfc-141">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="19bfc-142">Bir teklif tanımlayarak eklerken örnek uygulaması bir dize günlüklerinin bir `string` yazın `Action` alan:</span><span class="sxs-lookup"><span data-stu-id="19bfc-142">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="19bfc-143">Temsilcinin günlük iletisi şablonunu sağlanan türlerinden yer tutucu değerlerini alır.</span><span class="sxs-lookup"><span data-stu-id="19bfc-143">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="19bfc-144">Teklif parametresi olduğu bir teklif eklemek için bir temsilci örnek uygulaması tanımlar bir `string`:</span><span class="sxs-lookup"><span data-stu-id="19bfc-144">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="19bfc-145">Bir teklif eklemek için statik genişletme yöntemi `QuoteAdded`, teklif bağımsız değişken değeri alır ve buna ileten `Action` temsilci:</span><span class="sxs-lookup"><span data-stu-id="19bfc-145">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="19bfc-146">Dizin sayfasının sayfa modelinde (*Pages/Index.cshtml.cs*), `QuoteAdded` ileti günlüğe kaydetmek üzere çağrılır:</span><span class="sxs-lookup"><span data-stu-id="19bfc-146">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="19bfc-147">Uygulamanın konsol çıkışı inceleyin:</span><span class="sxs-lookup"><span data-stu-id="19bfc-147">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="19bfc-148">Örnek uygulama uygulayan bir `try` &ndash; `catch` teklif silme işlemi için desen.</span><span class="sxs-lookup"><span data-stu-id="19bfc-148">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="19bfc-149">Bir bilgi iletisidir başarılı silme işlemi için günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="19bfc-149">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="19bfc-150">Bir özel durum bir hata iletisi silme işlemi için günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="19bfc-150">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="19bfc-151">Başarısız silme işlemi için özel durum yığın izleme günlüğü iletisi içerir (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="19bfc-151">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="19bfc-152">Özel durum temsilciye nasıl geçirildiğini unutmayın `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="19bfc-152">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="19bfc-153">Dizin Sayfası sayfa modelinde başarılı teklif silme işlemini çağırır `QuoteDeleted` Günlükçü yöntemi.</span><span class="sxs-lookup"><span data-stu-id="19bfc-153">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="19bfc-154">Bir teklif silinmek üzere bulunamadığında olduğunda bir `ArgumentNullException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="19bfc-154">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="19bfc-155">Özel durum tarafından yakalanan `try` &ndash; `catch` deyimi ve çağırarak günlüğe `QuoteDeleteFailed` Günlükçü yöntemi `catch` blok (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="19bfc-155">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="19bfc-156">Bir teklif başarıyla silindiğinde, uygulamanın konsol çıkışı inceleyin:</span><span class="sxs-lookup"><span data-stu-id="19bfc-156">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="19bfc-157">Teklif silme işlemi başarısız olduğunda, uygulamanın konsol çıkışı inceleyin.</span><span class="sxs-lookup"><span data-stu-id="19bfc-157">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="19bfc-158">Not: özel günlük iletisinde yer alır</span><span class="sxs-lookup"><span data-stu-id="19bfc-158">Note that the exception is included in the log message:</span></span>

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T](T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() in 
      <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="loggermessagedefinescope"></a><span data-ttu-id="19bfc-159">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="19bfc-159">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="19bfc-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) oluşturur bir `Func` temsilci seçme tanımlamak için bir [oturum kapsamı](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="19bfc-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="19bfc-161">`DefineScope` aşırı yüklemeleri adlandırılmış biçim dizesine (şablonu) en fazla üç türü parametreleri geçirme izin verir.</span><span class="sxs-lookup"><span data-stu-id="19bfc-161">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="19bfc-162">İle olduğu gibi `Define` yöntemi, için sağlanan dize `DefineScope` yöntemdir bir şablon ve Ara değerli bir dize değil.</span><span class="sxs-lookup"><span data-stu-id="19bfc-162">As is the case with the `Define` method, the string provided to the `DefineScope` method is a template and not an interpolated string.</span></span> <span data-ttu-id="19bfc-163">Yer tutucu türleri belirttiğiniz sırayla doldurulur.</span><span class="sxs-lookup"><span data-stu-id="19bfc-163">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="19bfc-164">Yer tutucu adları şablonundaki şablonlar arasında açıklayıcı ve tutarlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="19bfc-164">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="19bfc-165">Bunlar özellik adları yapılandırılmış günlük verileri içinde işlevini görür.</span><span class="sxs-lookup"><span data-stu-id="19bfc-165">They serve as property names within structured log data.</span></span> <span data-ttu-id="19bfc-166">Öneririz [Pascal büyük/küçük harf](/dotnet/standard/design-guidelines/capitalization-conventions) yer tutucu adları için.</span><span class="sxs-lookup"><span data-stu-id="19bfc-166">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="19bfc-167">Örneğin, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="19bfc-167">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="19bfc-168">Tanımlayan bir [oturum kapsamı](xref:fundamentals/logging/index#log-scopes) günlük iletileri kullanarak bir dizi uygulamak için [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="19bfc-168">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="19bfc-169">Örnek uygulaması olan bir **Tümünü Temizle** tüm veritabanı tırnaklar silme düğmesi.</span><span class="sxs-lookup"><span data-stu-id="19bfc-169">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="19bfc-170">Tırnak işaretleri bunları kaldırarak silinir birer birer.</span><span class="sxs-lookup"><span data-stu-id="19bfc-170">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="19bfc-171">Bir teklif silinir, her zaman `QuoteDeleted` yöntemi, üzerinde Günlükçü çağrılır.</span><span class="sxs-lookup"><span data-stu-id="19bfc-171">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="19bfc-172">Bir günlük kapsamı bu günlük iletilerini eklenir.</span><span class="sxs-lookup"><span data-stu-id="19bfc-172">A log scope is added to these log messages.</span></span>

<span data-ttu-id="19bfc-173">Etkinleştirme `IncludeScopes` konsol Günlükçü bölümündeki *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="19bfc-173">Enable `IncludeScopes` in the console logger section of *appsettings.json*:</span></span>

[!code-csharp[](loggermessage/sample/appsettings.json?highlight=3-5)]

<span data-ttu-id="19bfc-174">Bir günlük kapsamı oluşturmak için tutmak için bir alan ekleyebilmek bir `Func` kapsam için temsilci.</span><span class="sxs-lookup"><span data-stu-id="19bfc-174">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="19bfc-175">Adlı bir alan örnek uygulaması oluşturur `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="19bfc-175">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="19bfc-176">Kullanım `DefineScope` temsilcisi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="19bfc-176">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="19bfc-177">Temsilci çağrıldığında en fazla üç tür şablon bağımsız değişken olarak kullanım için belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="19bfc-177">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="19bfc-178">Örnek uygulaması silinen tekliflerinin sayısını içeren bir ileti şablonunu kullanır (bir `int` türü):</span><span class="sxs-lookup"><span data-stu-id="19bfc-178">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="19bfc-179">Günlük iletisi için bir statik genişletme yöntemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="19bfc-179">Provide a static extension method for the log message.</span></span> <span data-ttu-id="19bfc-180">Herhangi bir tür parametre görünür adlandırılmış özellikleri için ileti şablona dahil.</span><span class="sxs-lookup"><span data-stu-id="19bfc-180">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="19bfc-181">Örnek uygulaması gereken bir `count` silmek için tırnak döndürür ve `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="19bfc-181">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="19bfc-182">Günlük uzantısı çağrıları kapsam sarmalayan bir `using` engelle:</span><span class="sxs-lookup"><span data-stu-id="19bfc-182">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="19bfc-183">Uygulamanın konsol çıkışı günlük iletilerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="19bfc-183">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="19bfc-184">Aşağıdaki sonucu dahil günlük kapsam iletisiyle silinmiş üç teklifleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="19bfc-184">The following result shows three quotes deleted with the log scope message included:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="additional-resources"></a><span data-ttu-id="19bfc-185">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="19bfc-185">Additional resources</span></span>

* [<span data-ttu-id="19bfc-186">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="19bfc-186">Logging</span></span>](xref:fundamentals/logging/index)
