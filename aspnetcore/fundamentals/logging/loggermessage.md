---
title: ASP.NET core'da LoggerMessage ile yüksek performans günlüğü
author: guardrex
description: LoggerMessage yüksek performanslı günlük kaydı senaryoları için daha az nesne ayırma işlemleri gerektiren önbelleğe temsilci oluşturmak için kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: 7a030b4bb754f65f8d93e51f203344c2dc02a634
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "58809269"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="fb427-103">ASP.NET core'da LoggerMessage ile yüksek performans günlüğü</span><span class="sxs-lookup"><span data-stu-id="fb427-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="fb427-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fb427-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fb427-105"><xref:Microsoft.Extensions.Logging.LoggerMessage> özellikleri daha az nesne ayırma işlemleri gerektiren önbelleğe temsilcileri oluşturup azaltılmış hesaplama ek yüküne karşılaştırdığınızda [Günlükçü genişletme yöntemleri](xref:Microsoft.Extensions.Logging.LoggerExtensions), gibi <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> ve <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>.</span><span class="sxs-lookup"><span data-stu-id="fb427-105"><xref:Microsoft.Extensions.Logging.LoggerMessage> features create cacheable delegates that require fewer object allocations and reduced computational overhead compared to [logger extension methods](xref:Microsoft.Extensions.Logging.LoggerExtensions), such as <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> and <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>.</span></span> <span data-ttu-id="fb427-106">Yüksek performanslı günlük kaydı senaryoları için kullanmak <xref:Microsoft.Extensions.Logging.LoggerMessage> deseni.</span><span class="sxs-lookup"><span data-stu-id="fb427-106">For high-performance logging scenarios, use the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

<span data-ttu-id="fb427-107"><xref:Microsoft.Extensions.Logging.LoggerMessage> Günlükçü genişletme yöntemleri performans aşağıdaki avantajları sağlar:</span><span class="sxs-lookup"><span data-stu-id="fb427-107"><xref:Microsoft.Extensions.Logging.LoggerMessage> provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="fb427-108">Günlükçü genişletme yöntemleri gerektirir "kutulama (dönüştürme)" değer türleri gibi `int`, içine `object`.</span><span class="sxs-lookup"><span data-stu-id="fb427-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="fb427-109"><xref:Microsoft.Extensions.Logging.LoggerMessage> Deseni statik kullanarak kutulama önler <xref:System.Action> alanları ve kesin tür belirtilmiş parametrelere sahip genişletme yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="fb427-109">The <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern avoids boxing by using static <xref:System.Action> fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="fb427-110">Günlükçü genişletme yöntemleri, her bir günlük iletisine yazılır ileti şablonunu (adlandırılmış bir biçim dizesi) ayrıştırma gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb427-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="fb427-111"><xref:Microsoft.Extensions.Logging.LoggerMessage> yalnızca bir şablon ileti tanımlandığında kez ayrıştırma gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fb427-111"><xref:Microsoft.Extensions.Logging.LoggerMessage> only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="fb427-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/2.x/LoggerMessageSamples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fb427-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/2.x/LoggerMessageSamples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="fb427-113">Örnek uygulamayı gösterir <xref:Microsoft.Extensions.Logging.LoggerMessage> özelliklerle izleme sistemi temel bir teklif.</span><span class="sxs-lookup"><span data-stu-id="fb427-113">The sample app demonstrates <xref:Microsoft.Extensions.Logging.LoggerMessage> features with a basic quote tracking system.</span></span> <span data-ttu-id="fb427-114">Uygulama ekler ve bir bellek içi veritabanı kullanarak siler.</span><span class="sxs-lookup"><span data-stu-id="fb427-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="fb427-115">Bu işlemler gerçekleşirken, günlük iletilerini kullanılarak oluşturulmuş <xref:Microsoft.Extensions.Logging.LoggerMessage> deseni.</span><span class="sxs-lookup"><span data-stu-id="fb427-115">As these operations occur, log messages are generated using the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="fb427-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="fb427-116">LoggerMessage.Define</span></span>

<span data-ttu-id="fb427-117">[(LogLevel, EventID, String) tanımlamak](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) oluşturur bir <xref:System.Action> temsilci bir ileti günlüğe kaydetme için.</span><span class="sxs-lookup"><span data-stu-id="fb427-117">[Define(LogLevel, EventId, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) creates an <xref:System.Action> delegate for logging a message.</span></span> <span data-ttu-id="fb427-118"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> aşırı yüklemeleri adlandırılmış biçim dizesine (şablon) en fazla altı türü parametreleri geçirme izin verir.</span><span class="sxs-lookup"><span data-stu-id="fb427-118"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="fb427-119">Sağlanan dize <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> yöntemi olan bir şablonu ve bir aradeğerlendirme dizesinde.</span><span class="sxs-lookup"><span data-stu-id="fb427-119">The string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> method is a template and not an interpolated string.</span></span> <span data-ttu-id="fb427-120">Yer tutucuları türleri belirttiğiniz sırada doldurulur.</span><span class="sxs-lookup"><span data-stu-id="fb427-120">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="fb427-121">Yer tutucu adlarını şablonundaki şablonlar arasında açıklayıcı ve tutarlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb427-121">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="fb427-122">Bunlar, özellik adları içinde yapılandırılmış günlük verileri görür.</span><span class="sxs-lookup"><span data-stu-id="fb427-122">They serve as property names within structured log data.</span></span> <span data-ttu-id="fb427-123">Öneririz [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) yer tutucu adları için.</span><span class="sxs-lookup"><span data-stu-id="fb427-123">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="fb427-124">Örneğin, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="fb427-124">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="fb427-125">Her günlük iletisi bir <xref:System.Action> tarafından oluşturulan bir statik alanda tutulan [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*).</span><span class="sxs-lookup"><span data-stu-id="fb427-125">Each log message is an <xref:System.Action> held in a static field created by [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*).</span></span> <span data-ttu-id="fb427-126">Örneğin, örnek uygulamayı dizin sayfası için bir GET isteği için bir günlük iletisine açıklamak için bir alan oluşturur (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="fb427-126">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="fb427-127">İçin <xref:System.Action>, belirtin:</span><span class="sxs-lookup"><span data-stu-id="fb427-127">For the <xref:System.Action>, specify:</span></span>

* <span data-ttu-id="fb427-128">Günlük düzeyi.</span><span class="sxs-lookup"><span data-stu-id="fb427-128">The log level.</span></span>
* <span data-ttu-id="fb427-129">Bir olay benzersiz tanımlayıcı (<xref:Microsoft.Extensions.Logging.EventId>) statik bir genişletme yöntemi adı.</span><span class="sxs-lookup"><span data-stu-id="fb427-129">A unique event identifier (<xref:Microsoft.Extensions.Logging.EventId>) with the name of the static extension method.</span></span>
* <span data-ttu-id="fb427-130">İleti şablonunu (biçim dizesi olarak adlandırılır).</span><span class="sxs-lookup"><span data-stu-id="fb427-130">The message template (named format string).</span></span> 

<span data-ttu-id="fb427-131">Örnek uygulama kümelerini dizin sayfası için bir istek:</span><span class="sxs-lookup"><span data-stu-id="fb427-131">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="fb427-132">Günlük düzeyini `Information`.</span><span class="sxs-lookup"><span data-stu-id="fb427-132">Log level to `Information`.</span></span>
* <span data-ttu-id="fb427-133">Olay Kimliği `1` adıyla `IndexPageRequested` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fb427-133">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="fb427-134">İleti şablonu (biçim dizesi olarak adlandırılır) bir dize.</span><span class="sxs-lookup"><span data-stu-id="fb427-134">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="fb427-135">Günlüğe kaydetme zenginleştirmek için olay kimliği ile sağlandığında yapılandırılmış günlük kaydı depoları olay adı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb427-135">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="fb427-136">Örneğin, [Serilog](https://github.com/serilog/serilog-extensions-logging) olay adını kullanır.</span><span class="sxs-lookup"><span data-stu-id="fb427-136">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="fb427-137"><xref:System.Action> Kesin türü belirtilmiş uzantısı yöntemiyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fb427-137">The <xref:System.Action> is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="fb427-138">`IndexPageRequested` Yöntemi örnek uygulamada bir dizin sayfası GET isteği için bir ileti kaydeder:</span><span class="sxs-lookup"><span data-stu-id="fb427-138">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="fb427-139">`IndexPageRequested` içinde günlükçü adı verilen `OnGetAsync` yönteminde *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="fb427-139">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="fb427-140">Uygulamanın konsol çıkışını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="fb427-140">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="fb427-141">Günlük ileti parametreleri geçirmek için en fazla altı tür statik alanı oluştururken tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="fb427-141">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="fb427-142">Örnek uygulama bir teklif tanımlayarak eklerken bir dize günlükleri bir `string` yazın <xref:System.Action> alan:</span><span class="sxs-lookup"><span data-stu-id="fb427-142">The sample app logs a string when adding a quote by defining a `string` type for the <xref:System.Action> field:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="fb427-143">Temsilcinin günlük iletisi şablonu, sağlanan türlerinden yer tutucu değerlerini alır.</span><span class="sxs-lookup"><span data-stu-id="fb427-143">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="fb427-144">Örnek uygulamayı teklif parametrenin bulunduğu bir tırnak işareti eklemek için bir temsilci tanımlar. bir `string`:</span><span class="sxs-lookup"><span data-stu-id="fb427-144">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="fb427-145">Bir teklif ekleme statik genişletme yöntemi `QuoteAdded`, teklif bağımsız değişkenin değerini alır ve buna ileten <xref:System.Action> temsilci:</span><span class="sxs-lookup"><span data-stu-id="fb427-145">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the <xref:System.Action> delegate:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="fb427-146">Dizin sayfa sayfa modelinde (*Pages/Index.cshtml.cs*), `QuoteAdded` ileti günlüğe kaydetmek üzere çağrılır:</span><span class="sxs-lookup"><span data-stu-id="fb427-146">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="fb427-147">Uygulamanın konsol çıkışını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="fb427-147">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="fb427-148">Örnek uygulama uygulayan bir [deneyin&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) teklif silinmek deseni.</span><span class="sxs-lookup"><span data-stu-id="fb427-148">The sample app implements a [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) pattern for quote deletion.</span></span> <span data-ttu-id="fb427-149">Bir bilgi iletisidir başarılı silme işlemi için günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fb427-149">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="fb427-150">Bir özel durum oluştuğunda bir hata iletisi bir silme işlemi için günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fb427-150">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="fb427-151">Başarısız silme işlemi için özel durum yığın izleme günlüğü iletisi içerir (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="fb427-151">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="fb427-152">Özel durum temsilciye nasıl geçirildiğini unutmayın `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="fb427-152">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="fb427-153">Dizin sayfası için sayfa modelinde başarılı teklif silme işlemini çağırır `QuoteDeleted` Günlükçü yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fb427-153">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="fb427-154">Bir teklif silinmek üzere bulunamadığında, bir <xref:System.ArgumentNullException> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fb427-154">When a quote isn't found for deletion, an <xref:System.ArgumentNullException> is thrown.</span></span> <span data-ttu-id="fb427-155">Özel durum tarafından yakalanan [deneyin&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) deyimi ve çağırarak oturum `QuoteDeleteFailed` içinde Günlükçü metodunda [catch](/dotnet/csharp/language-reference/keywords/try-catch) blok (*Pages/Index.cshtml.cs* ):</span><span class="sxs-lookup"><span data-stu-id="fb427-155">The exception is trapped by the [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) statement and logged by calling the `QuoteDeleteFailed` method on the logger in the [catch](/dotnet/csharp/language-reference/keywords/try-catch) block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="fb427-156">Teklif başarıyla silindiğinde, uygulamanın konsol çıkışını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="fb427-156">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="fb427-157">Teklif silme işlemi başarısız olduğunda uygulamanın konsol çıkışını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="fb427-157">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="fb427-158">Özel bir günlük iletisinde eklendiğini unutmayın:</span><span class="sxs-lookup"><span data-stu-id="fb427-158">Note that the exception is included in the log message:</span></span>

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T]
       (T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() 
      in <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="loggermessagedefinescope"></a><span data-ttu-id="fb427-159">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="fb427-159">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="fb427-160">[DefineScope(String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) oluşturur bir <xref:System.Func%601> temsilci tanımlamak için bir [oturum kapsamı](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="fb427-160">[DefineScope(String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) creates a <xref:System.Func%601> delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="fb427-161"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> aşırı yüklemeleri adlandırılmış biçim dizesine (şablon) en fazla üç türü parametreleri geçirme izin verir.</span><span class="sxs-lookup"><span data-stu-id="fb427-161"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="fb427-162">Olduğu gibi <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> yöntem, sağlanan dize <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> yöntemi olan bir şablonu ve bir aradeğerlendirme dizesinde.</span><span class="sxs-lookup"><span data-stu-id="fb427-162">As is the case with the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> method, the string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> method is a template and not an interpolated string.</span></span> <span data-ttu-id="fb427-163">Yer tutucuları türleri belirttiğiniz sırada doldurulur.</span><span class="sxs-lookup"><span data-stu-id="fb427-163">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="fb427-164">Yer tutucu adlarını şablonundaki şablonlar arasında açıklayıcı ve tutarlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb427-164">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="fb427-165">Bunlar, özellik adları içinde yapılandırılmış günlük verileri görür.</span><span class="sxs-lookup"><span data-stu-id="fb427-165">They serve as property names within structured log data.</span></span> <span data-ttu-id="fb427-166">Öneririz [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) yer tutucu adları için.</span><span class="sxs-lookup"><span data-stu-id="fb427-166">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="fb427-167">Örneğin, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="fb427-167">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="fb427-168">Tanımlayan bir [oturum kapsamı](xref:fundamentals/logging/index#log-scopes) günlük iletileri kullanarak bir dizi için uygulanacak <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fb427-168">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> method.</span></span>

<span data-ttu-id="fb427-169">Örnek uygulamanın bir **Tümünü Temizle** tüm veritabanı tırnak silme düğmesi.</span><span class="sxs-lookup"><span data-stu-id="fb427-169">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="fb427-170">Tırnak işaretleri bunları kaldırmayı tarafından silinir birer güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="fb427-170">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="fb427-171">Bir teklif silindiğinden, her zaman `QuoteDeleted` yöntemi Günlükçü üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fb427-171">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="fb427-172">Bir günlük kapsamı bu günlük iletilerini eklenir.</span><span class="sxs-lookup"><span data-stu-id="fb427-172">A log scope is added to these log messages.</span></span>

<span data-ttu-id="fb427-173">Etkinleştirme `IncludeScopes` konsol Günlükçü bölümünde *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="fb427-173">Enable `IncludeScopes` in the console logger section of *appsettings.json*:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/appsettings.json?highlight=3-5)]

<span data-ttu-id="fb427-174">Günlük kapsamı oluşturmak için tutacak bir alan ekleyin. bir <xref:System.Func%601> temsilci kapsamın.</span><span class="sxs-lookup"><span data-stu-id="fb427-174">To create a log scope, add a field to hold a <xref:System.Func%601> delegate for the scope.</span></span> <span data-ttu-id="fb427-175">Örnek uygulama adında bir alan oluşturur `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="fb427-175">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="fb427-176">Kullanım <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> temsilci oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="fb427-176">Use <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> to create the delegate.</span></span> <span data-ttu-id="fb427-177">Temsilci çağrıldığında, en fazla üç tür şablon bağımsız değişken olarak kullanım için belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="fb427-177">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="fb427-178">Silinen tekliflerinin sayısını içeren bir ileti şablonu örnek uygulamayı kullanır (bir `int` türü):</span><span class="sxs-lookup"><span data-stu-id="fb427-178">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="fb427-179">Günlük iletisi için bir statik genişletme yöntemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="fb427-179">Provide a static extension method for the log message.</span></span> <span data-ttu-id="fb427-180">İleti şablonunda görünen adlandırılmış özellikleri için herhangi bir tür parametreleri içerir.</span><span class="sxs-lookup"><span data-stu-id="fb427-180">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="fb427-181">Örnek uygulamayı alır bir `count` silmek için tırnak işaretleri döndürür ve `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="fb427-181">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="fb427-182">Günlüğe kaydetme genişletme çağrıları kapsam sarmalayan bir [kullanarak](/dotnet/csharp/language-reference/keywords/using-statement) engelle:</span><span class="sxs-lookup"><span data-stu-id="fb427-182">The scope wraps the logging extension calls in a [using](/dotnet/csharp/language-reference/keywords/using-statement) block:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="fb427-183">Uygulamanın konsol çıkışında günlük iletilerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="fb427-183">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="fb427-184">Aşağıdaki sonucu üç tırnak işaretleri dahil günlük kapsam iletinin silinmiş gösterir:</span><span class="sxs-lookup"><span data-stu-id="fb427-184">The following result shows three quotes deleted with the log scope message included:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => 
          All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="additional-resources"></a><span data-ttu-id="fb427-185">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fb427-185">Additional resources</span></span>

* [<span data-ttu-id="fb427-186">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="fb427-186">Logging</span></span>](xref:fundamentals/logging/index)
