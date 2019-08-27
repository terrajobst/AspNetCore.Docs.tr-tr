---
title: ASP.NET Core 'de LoggerMessage ile yüksek performanslı günlüğe kaydetme
author: guardrex
description: Yüksek performanslı günlük senaryolarında daha az sayıda nesne ayırması gerektiren önbelleğe alınabilir temsilciler oluşturmak için LoggerMessage kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/26/2019
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: 56c60fe405660ff39e2696de591449c25f669de2
ms.sourcegitcommit: 0774a61a3a6c1412a7da0e7d932dc60c506441fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70059040"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="483d0-103">ASP.NET Core 'de LoggerMessage ile yüksek performanslı günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="483d0-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="483d0-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="483d0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="483d0-105"><xref:Microsoft.Extensions.Logging.LoggerMessage>özellikler, <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> ve <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>gibi [günlükçü uzantısı yöntemlerine](xref:Microsoft.Extensions.Logging.LoggerExtensions)kıyasla daha az nesne ayırma ve daha düşük hesaplama yükü gerektiren önbelleğe alınabilir temsilciler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="483d0-105"><xref:Microsoft.Extensions.Logging.LoggerMessage> features create cacheable delegates that require fewer object allocations and reduced computational overhead compared to [logger extension methods](xref:Microsoft.Extensions.Logging.LoggerExtensions), such as <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> and <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>.</span></span> <span data-ttu-id="483d0-106">Yüksek performanslı günlük senaryoları için, bu <xref:Microsoft.Extensions.Logging.LoggerMessage> kalıbı kullanın.</span><span class="sxs-lookup"><span data-stu-id="483d0-106">For high-performance logging scenarios, use the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

<span data-ttu-id="483d0-107"><xref:Microsoft.Extensions.Logging.LoggerMessage>Günlükçü uzantı yöntemlerine göre aşağıdaki performans avantajlarını sağlar:</span><span class="sxs-lookup"><span data-stu-id="483d0-107"><xref:Microsoft.Extensions.Logging.LoggerMessage> provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="483d0-108">Günlükçü uzantı yöntemleri `int` `object`, gibi "kutulama" (dönüştürme) değer türlerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="483d0-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="483d0-109">Bu <xref:Microsoft.Extensions.Logging.LoggerMessage> model, kesin türü belirtilmiş parametrelerle <xref:System.Action> statik alanlar ve genişletme yöntemleri kullanarak kutulamayı önler.</span><span class="sxs-lookup"><span data-stu-id="483d0-109">The <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern avoids boxing by using static <xref:System.Action> fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="483d0-110">Günlükçü uzantısı yöntemlerinin her bir günlük iletisi yazıldığında ileti şablonunu (biçim dizesi olarak adlandırılır) ayrıştırması gerekir.</span><span class="sxs-lookup"><span data-stu-id="483d0-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="483d0-111"><xref:Microsoft.Extensions.Logging.LoggerMessage>yalnızca ileti tanımlandığında bir şablonu ayrıştırmayı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="483d0-111"><xref:Microsoft.Extensions.Logging.LoggerMessage> only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="483d0-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="483d0-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="483d0-113">Örnek uygulama, temel <xref:Microsoft.Extensions.Logging.LoggerMessage> bir Quote izleme sistemine sahip özellikleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="483d0-113">The sample app demonstrates <xref:Microsoft.Extensions.Logging.LoggerMessage> features with a basic quote tracking system.</span></span> <span data-ttu-id="483d0-114">Uygulama, bellek içi veritabanı kullanarak tırnak ekler ve siler.</span><span class="sxs-lookup"><span data-stu-id="483d0-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="483d0-115">Bu işlemler gerçekleştiğinde, günlük iletileri <xref:Microsoft.Extensions.Logging.LoggerMessage> model kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="483d0-115">As these operations occur, log messages are generated using the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="483d0-116">LoggerMessage. define</span><span class="sxs-lookup"><span data-stu-id="483d0-116">LoggerMessage.Define</span></span>

<span data-ttu-id="483d0-117">[Define (LogLevel, EventID, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) bir <xref:System.Action> iletiyi günlüğe kaydetmek için bir temsilci oluşturur.</span><span class="sxs-lookup"><span data-stu-id="483d0-117">[Define(LogLevel, EventId, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) creates an <xref:System.Action> delegate for logging a message.</span></span> <span data-ttu-id="483d0-118"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*>Aşırı Yüklemeler, adlandırılmış biçim dizesine (şablon) altı tür parametre geçişine izin verir.</span><span class="sxs-lookup"><span data-stu-id="483d0-118"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="483d0-119"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> Yöntemine girilen dize, enterpolasyonlu bir dize değil, bir şablondur.</span><span class="sxs-lookup"><span data-stu-id="483d0-119">The string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> method is a template and not an interpolated string.</span></span> <span data-ttu-id="483d0-120">Yer tutucular, türlerin belirtilme sırasına göre doldurulur.</span><span class="sxs-lookup"><span data-stu-id="483d0-120">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="483d0-121">Şablondaki yer tutucu adları, şablonlar genelinde açıklayıcı ve tutarlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="483d0-121">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="483d0-122">Bunlar, yapılandırılmış günlük verileri içinde özellik adı olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="483d0-122">They serve as property names within structured log data.</span></span> <span data-ttu-id="483d0-123">Yer tutucu adları için [Pascal büyük harfleri](/dotnet/standard/design-guidelines/capitalization-conventions) öneririz.</span><span class="sxs-lookup"><span data-stu-id="483d0-123">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="483d0-124">Örneğin, `{Count}` `{FirstName}`,.</span><span class="sxs-lookup"><span data-stu-id="483d0-124">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="483d0-125">Her günlük iletisi, <xref:System.Action> [loggermessage. define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*)tarafından oluşturulan statik bir alanda tutulur.</span><span class="sxs-lookup"><span data-stu-id="483d0-125">Each log message is an <xref:System.Action> held in a static field created by [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*).</span></span> <span data-ttu-id="483d0-126">Örneğin, örnek uygulama, Dizin sayfası (*iç/LoggerExtensions. cs*) IÇIN bir GET isteğinin günlük iletisini tanımlayacak bir alan oluşturur:</span><span class="sxs-lookup"><span data-stu-id="483d0-126">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="483d0-127"><xref:System.Action>İçin şunu belirtin:</span><span class="sxs-lookup"><span data-stu-id="483d0-127">For the <xref:System.Action>, specify:</span></span>

* <span data-ttu-id="483d0-128">Günlük düzeyi.</span><span class="sxs-lookup"><span data-stu-id="483d0-128">The log level.</span></span>
* <span data-ttu-id="483d0-129">Statik Uzantı yönteminin adı ile<xref:Microsoft.Extensions.Logging.EventId>benzersiz bir olay tanımlayıcısı ().</span><span class="sxs-lookup"><span data-stu-id="483d0-129">A unique event identifier (<xref:Microsoft.Extensions.Logging.EventId>) with the name of the static extension method.</span></span>
* <span data-ttu-id="483d0-130">İleti şablonu (biçim dizesi olarak adlandırılır).</span><span class="sxs-lookup"><span data-stu-id="483d0-130">The message template (named format string).</span></span> 

<span data-ttu-id="483d0-131">Örnek uygulamanın dizin sayfası için bir istek şunları ayarlar:</span><span class="sxs-lookup"><span data-stu-id="483d0-131">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="483d0-132">Günlük düzeyi `Information`.</span><span class="sxs-lookup"><span data-stu-id="483d0-132">Log level to `Information`.</span></span>
* <span data-ttu-id="483d0-133">Yöntemin`IndexPageRequested` adı `1` ile olay kimliği.</span><span class="sxs-lookup"><span data-stu-id="483d0-133">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="483d0-134">Bir dizeye ileti şablonu (biçim dizesi olarak adlandırılır).</span><span class="sxs-lookup"><span data-stu-id="483d0-134">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="483d0-135">Yapılandırılmış günlük depoları, olay kimliği ile birlikte verileri zenginleştirmek için sağlandığında olay adını kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="483d0-135">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="483d0-136">Örneğin, [Serilog](https://github.com/serilog/serilog-extensions-logging) olay adını kullanır.</span><span class="sxs-lookup"><span data-stu-id="483d0-136">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="483d0-137">, <xref:System.Action> Türü kesin belirlenmiş bir uzantı yöntemiyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="483d0-137">The <xref:System.Action> is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="483d0-138">Yöntemi `IndexPageRequested` , örnek uygulamada bir dizin sayfası get isteği için bir ileti kaydeder:</span><span class="sxs-lookup"><span data-stu-id="483d0-138">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="483d0-139">`IndexPageRequested`, `OnGetAsync` *sayfa/dizin. cshtml. cs*içindeki yöntemdeki günlükçü üzerinde çağrılır:</span><span class="sxs-lookup"><span data-stu-id="483d0-139">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="483d0-140">Uygulamanın konsol çıkışını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="483d0-140">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="483d0-141">Parametreleri bir günlük iletisine geçirmek için, statik alanı oluştururken en fazla altı tür tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="483d0-141">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="483d0-142">Örnek uygulama, `string` <xref:System.Action> alan için bir tür tanımlayarak bir alıntı eklerken bir dizeyi günlüğe kaydeder:</span><span class="sxs-lookup"><span data-stu-id="483d0-142">The sample app logs a string when adding a quote by defining a `string` type for the <xref:System.Action> field:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="483d0-143">Temsilcinin günlük iletisi şablonu, belirtilen türlerden yer tutucu değerlerini alır.</span><span class="sxs-lookup"><span data-stu-id="483d0-143">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="483d0-144">Örnek uygulama, quote parametresinin bir `string`tırnak işareti eklemek için bir temsilci tanımlar:</span><span class="sxs-lookup"><span data-stu-id="483d0-144">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="483d0-145">Tırnak `QuoteAdded`eklemek için statik genişletme yöntemi, quote bağımsız değişkeni değerini alır ve <xref:System.Action> temsilciye geçirir:</span><span class="sxs-lookup"><span data-stu-id="483d0-145">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the <xref:System.Action> delegate:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="483d0-146">Dizin sayfasının sayfa modelinde (*Sayfalar/Index. cshtml. cs*), `QuoteAdded` iletiyi günlüğe kaydetmek için çağrılır:</span><span class="sxs-lookup"><span data-stu-id="483d0-146">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="483d0-147">Uygulamanın konsol çıkışını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="483d0-147">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="483d0-148">Örnek uygulama, teklif silme için bir [TRY&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) kalıbı uygular.</span><span class="sxs-lookup"><span data-stu-id="483d0-148">The sample app implements a [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) pattern for quote deletion.</span></span> <span data-ttu-id="483d0-149">Başarılı silme işlemi için bir bilgilendirici ileti günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="483d0-149">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="483d0-150">Bir özel durum oluştuğunda silme işlemi için bir hata iletisi günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="483d0-150">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="483d0-151">Başarısız silme işleminin günlük iletisi, özel durum yığın izlemesini içerir (*iç/LoggerExtensions. cs*):</span><span class="sxs-lookup"><span data-stu-id="483d0-151">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="483d0-152">Özel durumun içindeki `QuoteDeleteFailed`temsilciye nasıl geçtiğini aklınızda yapın:</span><span class="sxs-lookup"><span data-stu-id="483d0-152">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="483d0-153">Dizin sayfasının sayfa modelinde, başarılı bir teklif silme işlemi günlükçü üzerindeki `QuoteDeleted` yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="483d0-153">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="483d0-154">Silinmek üzere bir teklif bulunamadığında, bir <xref:System.ArgumentNullException> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="483d0-154">When a quote isn't found for deletion, an <xref:System.ArgumentNullException> is thrown.</span></span> <span data-ttu-id="483d0-155">Özel durum [TRY&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) ifadesiyle yakalar ve [catch](/dotnet/csharp/language-reference/keywords/try-catch) bloğunda (*Pages/Index. cshtml. cs*) günlükçü üzerindeki `QuoteDeleteFailed` yöntemi çağırarak günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="483d0-155">The exception is trapped by the [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) statement and logged by calling the `QuoteDeleteFailed` method on the logger in the [catch](/dotnet/csharp/language-reference/keywords/try-catch) block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet5&highlight=9,13)]

<span data-ttu-id="483d0-156">Bir teklif başarıyla silindiğinde, uygulamanın konsol çıkışını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="483d0-156">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="483d0-157">Teklif silme başarısız olduğunda, uygulamanın konsol çıkışını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="483d0-157">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="483d0-158">Özel durumun günlük iletisine dahil edildiğini unutmayın:</span><span class="sxs-lookup"><span data-stu-id="483d0-158">Note that the exception is included in the log message:</span></span>

```console
LoggerMessageSample.Pages.IndexModel: Error: Quote delete failed (Id = 999)

System.NullReferenceException: Object reference not set to an instance of an object.
   at lambda_method(Closure , ValueBuffer )
   at System.Linq.Enumerable.SelectEnumerableIterator`2.MoveNext()
   at Microsoft.EntityFrameworkCore.InMemory.Query.Internal.InMemoryShapedQueryCompilingExpressionVisitor.AsyncQueryingEnumerable`1.AsyncEnumerator.MoveNextAsync()
   at Microsoft.EntityFrameworkCore.Query.ShapedQueryCompilingExpressionVisitor.SingleOrDefaultAsync[TSource](IAsyncEnumerable`1 asyncEnumerable, CancellationToken cancellationToken)
   at Microsoft.EntityFrameworkCore.Query.ShapedQueryCompilingExpressionVisitor.SingleOrDefaultAsync[TSource](IAsyncEnumerable`1 asyncEnumerable, CancellationToken cancellationToken)
   at LoggerMessageSample.Pages.IndexModel.OnPostDeleteQuoteAsync(Int32 id) in c:\Users\guard\Documents\GitHub\Docs\aspnetcore\fundamentals\logging\loggermessage\samples\3.x\LoggerMessageSample\Pages\Index.cshtml.cs:line 77
```

## <a name="loggermessagedefinescope"></a><span data-ttu-id="483d0-159">LoggerMessage. DefineScope</span><span class="sxs-lookup"><span data-stu-id="483d0-159">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="483d0-160">[DefineScope (String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) , <xref:System.Func%601> [günlük kapsamı](xref:fundamentals/logging/index#log-scopes)tanımlamak için bir temsilci oluşturur.</span><span class="sxs-lookup"><span data-stu-id="483d0-160">[DefineScope(String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) creates a <xref:System.Func%601> delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="483d0-161"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*>Aşırı Yüklemeler, adlandırılmış biçim dizesine (şablon) üç tür parametrenin geçirilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="483d0-161"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="483d0-162"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> Yönteminde olduğu gibi, yöntemi <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> için girilen dize, ilişkili dize değil, bir şablondur.</span><span class="sxs-lookup"><span data-stu-id="483d0-162">As is the case with the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> method, the string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> method is a template and not an interpolated string.</span></span> <span data-ttu-id="483d0-163">Yer tutucular, türlerin belirtilme sırasına göre doldurulur.</span><span class="sxs-lookup"><span data-stu-id="483d0-163">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="483d0-164">Şablondaki yer tutucu adları, şablonlar genelinde açıklayıcı ve tutarlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="483d0-164">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="483d0-165">Bunlar, yapılandırılmış günlük verileri içinde özellik adı olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="483d0-165">They serve as property names within structured log data.</span></span> <span data-ttu-id="483d0-166">Yer tutucu adları için [Pascal büyük harfleri](/dotnet/standard/design-guidelines/capitalization-conventions) öneririz.</span><span class="sxs-lookup"><span data-stu-id="483d0-166">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="483d0-167">Örneğin, `{Count}` `{FirstName}`,.</span><span class="sxs-lookup"><span data-stu-id="483d0-167">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="483d0-168"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> Yöntemini kullanarak bir dizi günlük mesajı için uygulanacak [günlük kapsamını](xref:fundamentals/logging/index#log-scopes) tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="483d0-168">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> method.</span></span>

<span data-ttu-id="483d0-169">Örnek uygulamanın, veritabanındaki tüm teklifleri silmek için **Tümünü Temizle** düğmesi vardır.</span><span class="sxs-lookup"><span data-stu-id="483d0-169">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="483d0-170">Tırnak işaretleri tek seferde kaldırılarak silinir.</span><span class="sxs-lookup"><span data-stu-id="483d0-170">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="483d0-171">Bir teklifin her silindiği `QuoteDeleted` her seferinde yöntemi günlükçü üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="483d0-171">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="483d0-172">Bu günlük iletilerine bir günlük kapsamı eklenir.</span><span class="sxs-lookup"><span data-stu-id="483d0-172">A log scope is added to these log messages.</span></span>

<span data-ttu-id="483d0-173">`IncludeScopes` *AppSettings. JSON*konsol günlükçü bölümünde etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="483d0-173">Enable `IncludeScopes` in the console logger section of *appsettings.json*:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/appsettings.json?highlight=3-5)]

<span data-ttu-id="483d0-174">Bir günlük kapsamı oluşturmak için, kapsam için bir <xref:System.Func%601> temsilci tutacak bir alan ekleyin.</span><span class="sxs-lookup"><span data-stu-id="483d0-174">To create a log scope, add a field to hold a <xref:System.Func%601> delegate for the scope.</span></span> <span data-ttu-id="483d0-175">Örnek uygulama ( `_allQuotesDeletedScope` *iç/loggerextensions. cs*) adlı bir alan oluşturur:</span><span class="sxs-lookup"><span data-stu-id="483d0-175">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="483d0-176">Temsilciyi <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="483d0-176">Use <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> to create the delegate.</span></span> <span data-ttu-id="483d0-177">Temsilci çağrıldığında Şablon bağımsız değişkenleri olarak kullanmak için en fazla üç tür belirlenebilir.</span><span class="sxs-lookup"><span data-stu-id="483d0-177">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="483d0-178">Örnek uygulama, silinen tekliflerin sayısını (bir `int` tür) içeren bir ileti şablonu kullanır:</span><span class="sxs-lookup"><span data-stu-id="483d0-178">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="483d0-179">Günlük iletisi için bir statik genişletme yöntemi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="483d0-179">Provide a static extension method for the log message.</span></span> <span data-ttu-id="483d0-180">İleti şablonunda görünen adlandırılmış özellikler için herhangi bir tür parametresi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="483d0-180">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="483d0-181">Örnek uygulama, silmek ve geri `count` `_allQuotesDeletedScope`döndürmek için tırnak içine alır:</span><span class="sxs-lookup"><span data-stu-id="483d0-181">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="483d0-182">Kapsam, oturum açma uzantısı çağrılarını bir [using](/dotnet/csharp/language-reference/keywords/using-statement) bloğunda sarmalar:</span><span class="sxs-lookup"><span data-stu-id="483d0-182">The scope wraps the logging extension calls in a [using](/dotnet/csharp/language-reference/keywords/using-statement) block:</span></span>

[!code-csharp[](loggermessage/samples/3.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="483d0-183">Uygulamanın konsol çıkışında günlük iletilerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="483d0-183">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="483d0-184">Aşağıdaki sonuç, günlük kapsamı iletisi dahil olmak üzere silinen üç tırnak gösterir:</span><span class="sxs-lookup"><span data-stu-id="483d0-184">The following result shows three quotes deleted with the log scope message included:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="483d0-185"><xref:Microsoft.Extensions.Logging.LoggerMessage>özellikler, <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> ve <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>gibi [günlükçü uzantısı yöntemlerine](xref:Microsoft.Extensions.Logging.LoggerExtensions)kıyasla daha az nesne ayırma ve daha düşük hesaplama yükü gerektiren önbelleğe alınabilir temsilciler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="483d0-185"><xref:Microsoft.Extensions.Logging.LoggerMessage> features create cacheable delegates that require fewer object allocations and reduced computational overhead compared to [logger extension methods](xref:Microsoft.Extensions.Logging.LoggerExtensions), such as <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> and <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug*>.</span></span> <span data-ttu-id="483d0-186">Yüksek performanslı günlük senaryoları için, bu <xref:Microsoft.Extensions.Logging.LoggerMessage> kalıbı kullanın.</span><span class="sxs-lookup"><span data-stu-id="483d0-186">For high-performance logging scenarios, use the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

<span data-ttu-id="483d0-187"><xref:Microsoft.Extensions.Logging.LoggerMessage>Günlükçü uzantı yöntemlerine göre aşağıdaki performans avantajlarını sağlar:</span><span class="sxs-lookup"><span data-stu-id="483d0-187"><xref:Microsoft.Extensions.Logging.LoggerMessage> provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="483d0-188">Günlükçü uzantı yöntemleri `int` `object`, gibi "kutulama" (dönüştürme) değer türlerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="483d0-188">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="483d0-189">Bu <xref:Microsoft.Extensions.Logging.LoggerMessage> model, kesin türü belirtilmiş parametrelerle <xref:System.Action> statik alanlar ve genişletme yöntemleri kullanarak kutulamayı önler.</span><span class="sxs-lookup"><span data-stu-id="483d0-189">The <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern avoids boxing by using static <xref:System.Action> fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="483d0-190">Günlükçü uzantısı yöntemlerinin her bir günlük iletisi yazıldığında ileti şablonunu (biçim dizesi olarak adlandırılır) ayrıştırması gerekir.</span><span class="sxs-lookup"><span data-stu-id="483d0-190">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="483d0-191"><xref:Microsoft.Extensions.Logging.LoggerMessage>yalnızca ileti tanımlandığında bir şablonu ayrıştırmayı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="483d0-191"><xref:Microsoft.Extensions.Logging.LoggerMessage> only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="483d0-192">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="483d0-192">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="483d0-193">Örnek uygulama, temel <xref:Microsoft.Extensions.Logging.LoggerMessage> bir Quote izleme sistemine sahip özellikleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="483d0-193">The sample app demonstrates <xref:Microsoft.Extensions.Logging.LoggerMessage> features with a basic quote tracking system.</span></span> <span data-ttu-id="483d0-194">Uygulama, bellek içi veritabanı kullanarak tırnak ekler ve siler.</span><span class="sxs-lookup"><span data-stu-id="483d0-194">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="483d0-195">Bu işlemler gerçekleştiğinde, günlük iletileri <xref:Microsoft.Extensions.Logging.LoggerMessage> model kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="483d0-195">As these operations occur, log messages are generated using the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="483d0-196">LoggerMessage. define</span><span class="sxs-lookup"><span data-stu-id="483d0-196">LoggerMessage.Define</span></span>

<span data-ttu-id="483d0-197">[Define (LogLevel, EventID, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) bir <xref:System.Action> iletiyi günlüğe kaydetmek için bir temsilci oluşturur.</span><span class="sxs-lookup"><span data-stu-id="483d0-197">[Define(LogLevel, EventId, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*) creates an <xref:System.Action> delegate for logging a message.</span></span> <span data-ttu-id="483d0-198"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*>Aşırı Yüklemeler, adlandırılmış biçim dizesine (şablon) altı tür parametre geçişine izin verir.</span><span class="sxs-lookup"><span data-stu-id="483d0-198"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="483d0-199"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> Yöntemine girilen dize, enterpolasyonlu bir dize değil, bir şablondur.</span><span class="sxs-lookup"><span data-stu-id="483d0-199">The string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> method is a template and not an interpolated string.</span></span> <span data-ttu-id="483d0-200">Yer tutucular, türlerin belirtilme sırasına göre doldurulur.</span><span class="sxs-lookup"><span data-stu-id="483d0-200">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="483d0-201">Şablondaki yer tutucu adları, şablonlar genelinde açıklayıcı ve tutarlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="483d0-201">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="483d0-202">Bunlar, yapılandırılmış günlük verileri içinde özellik adı olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="483d0-202">They serve as property names within structured log data.</span></span> <span data-ttu-id="483d0-203">Yer tutucu adları için [Pascal büyük harfleri](/dotnet/standard/design-guidelines/capitalization-conventions) öneririz.</span><span class="sxs-lookup"><span data-stu-id="483d0-203">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="483d0-204">Örneğin, `{Count}` `{FirstName}`,.</span><span class="sxs-lookup"><span data-stu-id="483d0-204">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="483d0-205">Her günlük iletisi, <xref:System.Action> [loggermessage. define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*)tarafından oluşturulan statik bir alanda tutulur.</span><span class="sxs-lookup"><span data-stu-id="483d0-205">Each log message is an <xref:System.Action> held in a static field created by [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define*).</span></span> <span data-ttu-id="483d0-206">Örneğin, örnek uygulama, Dizin sayfası (*iç/LoggerExtensions. cs*) IÇIN bir GET isteğinin günlük iletisini tanımlayacak bir alan oluşturur:</span><span class="sxs-lookup"><span data-stu-id="483d0-206">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="483d0-207"><xref:System.Action>İçin şunu belirtin:</span><span class="sxs-lookup"><span data-stu-id="483d0-207">For the <xref:System.Action>, specify:</span></span>

* <span data-ttu-id="483d0-208">Günlük düzeyi.</span><span class="sxs-lookup"><span data-stu-id="483d0-208">The log level.</span></span>
* <span data-ttu-id="483d0-209">Statik Uzantı yönteminin adı ile<xref:Microsoft.Extensions.Logging.EventId>benzersiz bir olay tanımlayıcısı ().</span><span class="sxs-lookup"><span data-stu-id="483d0-209">A unique event identifier (<xref:Microsoft.Extensions.Logging.EventId>) with the name of the static extension method.</span></span>
* <span data-ttu-id="483d0-210">İleti şablonu (biçim dizesi olarak adlandırılır).</span><span class="sxs-lookup"><span data-stu-id="483d0-210">The message template (named format string).</span></span> 

<span data-ttu-id="483d0-211">Örnek uygulamanın dizin sayfası için bir istek şunları ayarlar:</span><span class="sxs-lookup"><span data-stu-id="483d0-211">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="483d0-212">Günlük düzeyi `Information`.</span><span class="sxs-lookup"><span data-stu-id="483d0-212">Log level to `Information`.</span></span>
* <span data-ttu-id="483d0-213">Yöntemin`IndexPageRequested` adı `1` ile olay kimliği.</span><span class="sxs-lookup"><span data-stu-id="483d0-213">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="483d0-214">Bir dizeye ileti şablonu (biçim dizesi olarak adlandırılır).</span><span class="sxs-lookup"><span data-stu-id="483d0-214">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="483d0-215">Yapılandırılmış günlük depoları, olay kimliği ile birlikte verileri zenginleştirmek için sağlandığında olay adını kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="483d0-215">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="483d0-216">Örneğin, [Serilog](https://github.com/serilog/serilog-extensions-logging) olay adını kullanır.</span><span class="sxs-lookup"><span data-stu-id="483d0-216">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="483d0-217">, <xref:System.Action> Türü kesin belirlenmiş bir uzantı yöntemiyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="483d0-217">The <xref:System.Action> is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="483d0-218">Yöntemi `IndexPageRequested` , örnek uygulamada bir dizin sayfası get isteği için bir ileti kaydeder:</span><span class="sxs-lookup"><span data-stu-id="483d0-218">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="483d0-219">`IndexPageRequested`, `OnGetAsync` *sayfa/dizin. cshtml. cs*içindeki yöntemdeki günlükçü üzerinde çağrılır:</span><span class="sxs-lookup"><span data-stu-id="483d0-219">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="483d0-220">Uygulamanın konsol çıkışını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="483d0-220">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="483d0-221">Parametreleri bir günlük iletisine geçirmek için, statik alanı oluştururken en fazla altı tür tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="483d0-221">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="483d0-222">Örnek uygulama, `string` <xref:System.Action> alan için bir tür tanımlayarak bir alıntı eklerken bir dizeyi günlüğe kaydeder:</span><span class="sxs-lookup"><span data-stu-id="483d0-222">The sample app logs a string when adding a quote by defining a `string` type for the <xref:System.Action> field:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="483d0-223">Temsilcinin günlük iletisi şablonu, belirtilen türlerden yer tutucu değerlerini alır.</span><span class="sxs-lookup"><span data-stu-id="483d0-223">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="483d0-224">Örnek uygulama, quote parametresinin bir `string`tırnak işareti eklemek için bir temsilci tanımlar:</span><span class="sxs-lookup"><span data-stu-id="483d0-224">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="483d0-225">Tırnak `QuoteAdded`eklemek için statik genişletme yöntemi, quote bağımsız değişkeni değerini alır ve <xref:System.Action> temsilciye geçirir:</span><span class="sxs-lookup"><span data-stu-id="483d0-225">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the <xref:System.Action> delegate:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="483d0-226">Dizin sayfasının sayfa modelinde (*Sayfalar/Index. cshtml. cs*), `QuoteAdded` iletiyi günlüğe kaydetmek için çağrılır:</span><span class="sxs-lookup"><span data-stu-id="483d0-226">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="483d0-227">Uygulamanın konsol çıkışını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="483d0-227">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="483d0-228">Örnek uygulama, teklif silme için bir [TRY&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) kalıbı uygular.</span><span class="sxs-lookup"><span data-stu-id="483d0-228">The sample app implements a [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) pattern for quote deletion.</span></span> <span data-ttu-id="483d0-229">Başarılı silme işlemi için bir bilgilendirici ileti günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="483d0-229">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="483d0-230">Bir özel durum oluştuğunda silme işlemi için bir hata iletisi günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="483d0-230">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="483d0-231">Başarısız silme işleminin günlük iletisi, özel durum yığın izlemesini içerir (*iç/LoggerExtensions. cs*):</span><span class="sxs-lookup"><span data-stu-id="483d0-231">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="483d0-232">Özel durumun içindeki `QuoteDeleteFailed`temsilciye nasıl geçtiğini aklınızda yapın:</span><span class="sxs-lookup"><span data-stu-id="483d0-232">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="483d0-233">Dizin sayfasının sayfa modelinde, başarılı bir teklif silme işlemi günlükçü üzerindeki `QuoteDeleted` yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="483d0-233">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="483d0-234">Silinmek üzere bir teklif bulunamadığında, bir <xref:System.ArgumentNullException> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="483d0-234">When a quote isn't found for deletion, an <xref:System.ArgumentNullException> is thrown.</span></span> <span data-ttu-id="483d0-235">Özel durum [TRY&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) ifadesiyle yakalar ve [catch](/dotnet/csharp/language-reference/keywords/try-catch) bloğunda (*Pages/Index. cshtml. cs*) günlükçü üzerindeki `QuoteDeleteFailed` yöntemi çağırarak günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="483d0-235">The exception is trapped by the [try&ndash;catch](/dotnet/csharp/language-reference/keywords/try-catch) statement and logged by calling the `QuoteDeleteFailed` method on the logger in the [catch](/dotnet/csharp/language-reference/keywords/try-catch) block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="483d0-236">Bir teklif başarıyla silindiğinde, uygulamanın konsol çıkışını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="483d0-236">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the 
          consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="483d0-237">Teklif silme başarısız olduğunda, uygulamanın konsol çıkışını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="483d0-237">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="483d0-238">Özel durumun günlük iletisine dahil edildiğini unutmayın:</span><span class="sxs-lookup"><span data-stu-id="483d0-238">Note that the exception is included in the log message:</span></span>

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

## <a name="loggermessagedefinescope"></a><span data-ttu-id="483d0-239">LoggerMessage. DefineScope</span><span class="sxs-lookup"><span data-stu-id="483d0-239">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="483d0-240">[DefineScope (String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) , <xref:System.Func%601> [günlük kapsamı](xref:fundamentals/logging/index#log-scopes)tanımlamak için bir temsilci oluşturur.</span><span class="sxs-lookup"><span data-stu-id="483d0-240">[DefineScope(String)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*) creates a <xref:System.Func%601> delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="483d0-241"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*>Aşırı Yüklemeler, adlandırılmış biçim dizesine (şablon) üç tür parametrenin geçirilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="483d0-241"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="483d0-242"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> Yönteminde olduğu gibi, yöntemi <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> için girilen dize, ilişkili dize değil, bir şablondur.</span><span class="sxs-lookup"><span data-stu-id="483d0-242">As is the case with the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define*> method, the string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> method is a template and not an interpolated string.</span></span> <span data-ttu-id="483d0-243">Yer tutucular, türlerin belirtilme sırasına göre doldurulur.</span><span class="sxs-lookup"><span data-stu-id="483d0-243">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="483d0-244">Şablondaki yer tutucu adları, şablonlar genelinde açıklayıcı ve tutarlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="483d0-244">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="483d0-245">Bunlar, yapılandırılmış günlük verileri içinde özellik adı olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="483d0-245">They serve as property names within structured log data.</span></span> <span data-ttu-id="483d0-246">Yer tutucu adları için [Pascal büyük harfleri](/dotnet/standard/design-guidelines/capitalization-conventions) öneririz.</span><span class="sxs-lookup"><span data-stu-id="483d0-246">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="483d0-247">Örneğin, `{Count}` `{FirstName}`,.</span><span class="sxs-lookup"><span data-stu-id="483d0-247">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="483d0-248"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> Yöntemini kullanarak bir dizi günlük mesajı için uygulanacak [günlük kapsamını](xref:fundamentals/logging/index#log-scopes) tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="483d0-248">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> method.</span></span>

<span data-ttu-id="483d0-249">Örnek uygulamanın, veritabanındaki tüm teklifleri silmek için **Tümünü Temizle** düğmesi vardır.</span><span class="sxs-lookup"><span data-stu-id="483d0-249">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="483d0-250">Tırnak işaretleri tek seferde kaldırılarak silinir.</span><span class="sxs-lookup"><span data-stu-id="483d0-250">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="483d0-251">Bir teklifin her silindiği `QuoteDeleted` her seferinde yöntemi günlükçü üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="483d0-251">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="483d0-252">Bu günlük iletilerine bir günlük kapsamı eklenir.</span><span class="sxs-lookup"><span data-stu-id="483d0-252">A log scope is added to these log messages.</span></span>

<span data-ttu-id="483d0-253">`IncludeScopes` *AppSettings. JSON*konsol günlükçü bölümünde etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="483d0-253">Enable `IncludeScopes` in the console logger section of *appsettings.json*:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/appsettings.json?highlight=3-5)]

<span data-ttu-id="483d0-254">Bir günlük kapsamı oluşturmak için, kapsam için bir <xref:System.Func%601> temsilci tutacak bir alan ekleyin.</span><span class="sxs-lookup"><span data-stu-id="483d0-254">To create a log scope, add a field to hold a <xref:System.Func%601> delegate for the scope.</span></span> <span data-ttu-id="483d0-255">Örnek uygulama ( `_allQuotesDeletedScope` *iç/loggerextensions. cs*) adlı bir alan oluşturur:</span><span class="sxs-lookup"><span data-stu-id="483d0-255">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="483d0-256">Temsilciyi <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="483d0-256">Use <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope*> to create the delegate.</span></span> <span data-ttu-id="483d0-257">Temsilci çağrıldığında Şablon bağımsız değişkenleri olarak kullanmak için en fazla üç tür belirlenebilir.</span><span class="sxs-lookup"><span data-stu-id="483d0-257">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="483d0-258">Örnek uygulama, silinen tekliflerin sayısını (bir `int` tür) içeren bir ileti şablonu kullanır:</span><span class="sxs-lookup"><span data-stu-id="483d0-258">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="483d0-259">Günlük iletisi için bir statik genişletme yöntemi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="483d0-259">Provide a static extension method for the log message.</span></span> <span data-ttu-id="483d0-260">İleti şablonunda görünen adlandırılmış özellikler için herhangi bir tür parametresi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="483d0-260">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="483d0-261">Örnek uygulama, silmek ve geri `count` `_allQuotesDeletedScope`döndürmek için tırnak içine alır:</span><span class="sxs-lookup"><span data-stu-id="483d0-261">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="483d0-262">Kapsam, oturum açma uzantısı çağrılarını bir [using](/dotnet/csharp/language-reference/keywords/using-statement) bloğunda sarmalar:</span><span class="sxs-lookup"><span data-stu-id="483d0-262">The scope wraps the logging extension calls in a [using](/dotnet/csharp/language-reference/keywords/using-statement) block:</span></span>

[!code-csharp[](loggermessage/samples/2.x/LoggerMessageSample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="483d0-263">Uygulamanın konsol çıkışında günlük iletilerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="483d0-263">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="483d0-264">Aşağıdaki sonuç, günlük kapsamı iletisi dahil olmak üzere silinen üç tırnak gösterir:</span><span class="sxs-lookup"><span data-stu-id="483d0-264">The following result shows three quotes deleted with the log scope message included:</span></span>

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

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="483d0-265">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="483d0-265">Additional resources</span></span>

* [<span data-ttu-id="483d0-266">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="483d0-266">Logging</span></span>](xref:fundamentals/logging/index)
