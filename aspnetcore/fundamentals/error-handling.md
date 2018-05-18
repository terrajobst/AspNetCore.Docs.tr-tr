---
title: ASP.NET Core hataları işlemek
author: ardalis
description: ASP.NET Core uygulamaları hataların nasıl işleneceğini bulur.
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 3ff3a17d14d9ed7c438399191ffe3cf93d555d49
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="828e1-103">ASP.NET Core hataları işlemek</span><span class="sxs-lookup"><span data-stu-id="828e1-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="828e1-104">Tarafından [Steve Smith](https://ardalis.com/) ve [zel Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="828e1-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="828e1-105">Bu makalede, ASP.NET Core uygulamaları hataları işlemek için ortak appoaches yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="828e1-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="828e1-106">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="828e1-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="828e1-107">Geliştirici özel durumu sayfası</span><span class="sxs-lookup"><span data-stu-id="828e1-107">The developer exception page</span></span>

<span data-ttu-id="828e1-108">Özel durumlar hakkında ayrıntılı bilgiler gösterilmektedir bir sayfasını görüntülemek için uygulamayı yapılandırmak için yükleme `Microsoft.AspNetCore.Diagnostics` NuGet paketini ve bir satırı ekleyin [başlangıç sınıfında yöntemini yapılandırma](xref:fundamentals/startup):</span><span class="sxs-lookup"><span data-stu-id="828e1-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](xref:fundamentals/startup):</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="828e1-109">PUT `UseDeveloperExceptionPage` Ara yazılımların, istediğiniz gibi özel durumları yakalamak için önce `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="828e1-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="828e1-110">Geliştirici özel durum sayfasını etkinleştir **yalnızca uygulama geliştirme ortamında çalışırken**.</span><span class="sxs-lookup"><span data-stu-id="828e1-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="828e1-111">Uygulama üretimde çalıştırıldığında ayrıntılı özel durum bilgilerini herkese açık şekilde paylaşma istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="828e1-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="828e1-112">[Ortamlarını yapılandırma hakkında daha fazla bilgi](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="828e1-112">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="828e1-113">Geliştirici özel durum sayfasını görmek için ayarlanan ortam ile örnek uygulamayı çalıştırın `Development`ve ekleme `?throw=true` uygulamanın temel URL.</span><span class="sxs-lookup"><span data-stu-id="828e1-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="828e1-114">Sayfa özel durumu ve istek hakkında bilgi içeren birkaç sekme içerir.</span><span class="sxs-lookup"><span data-stu-id="828e1-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="828e1-115">İlk sekme yığın izlemesi içerir.</span><span class="sxs-lookup"><span data-stu-id="828e1-115">The first tab includes a stack trace.</span></span> 

![Yığın izleme](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="828e1-117">Sonraki sekme sorgu dizesi parametreleri varsa gösterir.</span><span class="sxs-lookup"><span data-stu-id="828e1-117">The next tab shows the query string parameters, if any.</span></span>

![Sorgu dizesi parametreleri](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="828e1-119">Bu istek tanımlama bilgilerine sahip oldu, ancak olsaydı, üzerinde görünecekleri **tanımlama bilgilerini** sekmesi. Son sekmede geçirilmiş üstbilgileri görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="828e1-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Üstbilgileri](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="828e1-121">Sayfa işleme özel bir durum yapılandırma</span><span class="sxs-lookup"><span data-stu-id="828e1-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="828e1-122">Uygulama çalışmadığında kullanmak için bir özel durum işleyici sayfasını yapılandırmak için iyi bir fikirdir `Development` ortamı.</span><span class="sxs-lookup"><span data-stu-id="828e1-122">It's a good idea to configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="828e1-123">Bir MVC uygulamasında açıkça HTTP yöntemi öznitelikleriyle hata işleyici eylem yöntemi gibi tasarlamanız yok `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="828e1-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="828e1-124">Açık fiillerini kullanarak bazı istekleri yöntemi ulaşmasını engellemeniz.</span><span class="sxs-lookup"><span data-stu-id="828e1-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="828e1-125">Yapılandırma durumu kod sayfaları</span><span class="sxs-lookup"><span data-stu-id="828e1-125">Configuring status code pages</span></span>

<span data-ttu-id="828e1-126">Varsayılan olarak, bir uygulama zengin durum kod sayfası için HTTP durum kodları, gibi sağlamaz *404 Bulunamadı*.</span><span class="sxs-lookup"><span data-stu-id="828e1-126">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="828e1-127">Kod sayfaları durumu sağlamak için bir satıra ekleyerek durum kodu sayfaları ara yazılımı yapılandırmanız `Startup.Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="828e1-127">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="828e1-128">Varsayılan olarak, durum kodu sayfaları ara yazılımı 404 gibi ortak durum kodları için basit, yalnızca metin işleyiciler ekler:</span><span class="sxs-lookup"><span data-stu-id="828e1-128">By default, Status Code Pages Middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404 sayfası](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="828e1-130">Ara yazılım birkaç uzantı yöntemleri destekler.</span><span class="sxs-lookup"><span data-stu-id="828e1-130">The middleware supports several extension methods.</span></span> <span data-ttu-id="828e1-131">Lambda ifadesi bir yöntemi alır:</span><span class="sxs-lookup"><span data-stu-id="828e1-131">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="828e1-132">Başka bir yöntem içerik türü ve biçim dizesini alır:</span><span class="sxs-lookup"><span data-stu-id="828e1-132">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="828e1-133">Ayrıca yönlendirmek ve genişletme yöntemleri yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="828e1-133">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="828e1-134">Yeniden yönlendirme yöntemi 302 durum kodunu istemciye gönderir:</span><span class="sxs-lookup"><span data-stu-id="828e1-134">The redirect method sends a 302 status code to the client:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="828e1-135">Yeniden çalıştırma yöntemi istemciye özgün durum kodunu döndüren ancak işleyicinin yeniden yönlendirme URL'si de yürütür:</span><span class="sxs-lookup"><span data-stu-id="828e1-135">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="828e1-136">Bir Razor sayfalarının işleyici yöntemi veya MVC denetleyicisi belirli istekleri için durum kod sayfaları devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="828e1-136">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="828e1-137">Durum kod sayfaları devre dışı bırakmak için alma denemesi [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) gelen isteğin [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) koleksiyonu ve kullanılabilir durumdaysa özelliği devre dışı bırakın:</span><span class="sxs-lookup"><span data-stu-id="828e1-137">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="828e1-138">Özel durum işleme kodu</span><span class="sxs-lookup"><span data-stu-id="828e1-138">Exception-handling code</span></span>

<span data-ttu-id="828e1-139">Özel durum işleme sayfaları kodda istisnalar atabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="828e1-139">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="828e1-140">Genellikle, yalnızca statik içeriği oluşması üretim hata sayfaları için iyi bir fikir olabilir.</span><span class="sxs-lookup"><span data-stu-id="828e1-140">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="828e1-141">Ayrıca, yanıt üstbilgileri gönderildikten sonra yanıtın durum kodu değişiklik yapamaz veya herhangi bir özel durum sayfaları veya işleyicileri çalıştırabilirsiniz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="828e1-141">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="828e1-142">Yanıt doldurulmalıdır veya bağlantı durduruldu.</span><span class="sxs-lookup"><span data-stu-id="828e1-142">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="828e1-143">Sunucu özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="828e1-143">Server exception handling</span></span>

<span data-ttu-id="828e1-144">Özel durum işleme mantığı, uygulamanızda yanı sıra [server](xref:fundamentals/servers/index) uygulamanızı barındırma bazı özel durum işleme gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="828e1-144">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="828e1-145">Üst bilgileri gönderilmeden önce sunucunun bir özel durum yakalar, sunucunun gönderir. bir *500 İç sunucu hatası* hiçbir gövdesi olan yanıt.</span><span class="sxs-lookup"><span data-stu-id="828e1-145">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="828e1-146">Üstbilgileri gönderildikten sonra sunucu bir özel durum yakalar, sunucu bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="828e1-146">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="828e1-147">Uygulamanız tarafından işlenmeyen isteği sunucu tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="828e1-147">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="828e1-148">Sunucunun bir özel durum nedeniyle oluşan herhangi bir özel durum işlenmiş işleme.</span><span class="sxs-lookup"><span data-stu-id="828e1-148">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="828e1-149">Herhangi bir özel hata sayfaları yapılandırılabilir veya özel durum işleme ara yazılımı veya filtreler bu davranışını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="828e1-149">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="828e1-150">Başlangıç özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="828e1-150">Startup exception handling</span></span>

<span data-ttu-id="828e1-151">Yalnızca barındırma katmanı uygulama başlatma sırasında gerçekleşmesi özel durumlar işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="828e1-151">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="828e1-152">Kullanarak [Web ana bilgisayarı](xref:fundamentals/host/web-host), yapabilecekleriniz [nasıl konak hatalarına yanıt olarak başlatma sırasında davranacağını yapılandırmak](xref:fundamentals/host/web-host#detailed-errors) ile `captureStartupErrors` ve `detailedErrors` anahtarları.</span><span class="sxs-lookup"><span data-stu-id="828e1-152">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="828e1-153">Ana bilgisayar adresini/bağlantı noktası sonra bağlama hatası oluşursa Barındırma yakalanan başlatma hatası için bir hata sayfası yalnızca gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="828e1-153">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="828e1-154">Hiçbir bağlama herhangi bir nedenle başarısız olursa, barındırma katman bir kritik özel durumu, dotnet işlem çökmesi günlüğe kaydeder ve uygulama çalışırken bir hata sayfası görüntülenir [Kestrel](xref:fundamentals/servers/kestrel) sunucu.</span><span class="sxs-lookup"><span data-stu-id="828e1-154">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="828e1-155">Çalışırken [IIS](/iis) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 işlem hatası* tarafından döndürülen [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) işlemi olamazsa başladı.</span><span class="sxs-lookup"><span data-stu-id="828e1-155">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="828e1-156">Sorun giderme tavsiyesini takip [IIS üzerinde ASP.NET Core sorun giderme](xref:host-and-deploy/iis/troubleshoot) konu.</span><span class="sxs-lookup"><span data-stu-id="828e1-156">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="828e1-157">ASP.NET MVC hata işleme</span><span class="sxs-lookup"><span data-stu-id="828e1-157">ASP.NET MVC error handling</span></span>

<span data-ttu-id="828e1-158">[MVC](xref:mvc/overview) uygulamalara sahip özel durum filtreleri yapılandırma ve model doğrulama gerçekleştirme gibi hataları işlemek için bazı ek seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="828e1-158">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="828e1-159">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="828e1-159">Exception Filters</span></span>

<span data-ttu-id="828e1-160">Özel durum filtreleri, genel olarak veya bir MVC uygulamasında her denetleyici veya eylem başına temelinde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="828e1-160">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="828e1-161">Bu filtreler bir denetleyici eylemi veya başka bir filtre yürütülmesi sırasında oluşan tüm işlenmeyen bir özel durum işleme ve aksi durumda adlı değil.</span><span class="sxs-lookup"><span data-stu-id="828e1-161">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="828e1-162">Özel durum filtreleri hakkında daha fazla bilgi [filtreleri](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="828e1-162">Learn more about exception filters in [Filters](xref:mvc/controllers/filters).</span></span>

>[!TIP]
> <span data-ttu-id="828e1-163">Özel durum filtreleri içinde MVC Eylemler oluşan özel durumlarını yakalama için iyi, ancak bunlar hata ara yazılım işleme kadar esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="828e1-163">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="828e1-164">Ara yazılımı genel örneği için tercih ettiğiniz ve hata işleme yapmak için yalnızca ihtiyaç duyacağınız filtreleri kullanın *farklı* MVC eylemi seçildi tabanlı.</span><span class="sxs-lookup"><span data-stu-id="828e1-164">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="828e1-165">İşleme Model durumu hataları</span><span class="sxs-lookup"><span data-stu-id="828e1-165">Handling Model State Errors</span></span>

<span data-ttu-id="828e1-166">[Model doğrulama](xref:mvc/models/validation) her denetleyici eylemi çağırma öncesi oluşur ve incelemek için eylem yönteminin sorumluluğu olan `ModelState.IsValid` ve uygun şekilde tepki.</span><span class="sxs-lookup"><span data-stu-id="828e1-166">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="828e1-167">Model doğrulama hataları, ilgilenmek için standart bir kural, bu durumda izlemeniz gereken bazı uygulamalar seçecektir bir [filtre](xref:mvc/controllers/filters) böyle bir ilke uygulamak için uygun bir yerdir olabilir.</span><span class="sxs-lookup"><span data-stu-id="828e1-167">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="828e1-168">Geçersiz model durumlarıyla eylemlerinizi nasıl davranacağını test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="828e1-168">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="828e1-169">Daha fazla bilgi edinin [Test denetleyicisi mantığı](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="828e1-169">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>



