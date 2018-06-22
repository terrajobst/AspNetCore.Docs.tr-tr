---
title: ASP.NET Core hataları işlemek
author: ardalis
description: ASP.NET Core uygulamaları hataların nasıl işleneceğini bulur.
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
uid: fundamentals/error-handling
ms.openlocfilehash: 2fe46ecc32d61a7fafb2ad6e2a35456476608251
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273715"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="9b31f-103">ASP.NET Core hataları işlemek</span><span class="sxs-lookup"><span data-stu-id="9b31f-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="9b31f-104">Tarafından [Steve Smith](https://ardalis.com/) ve [zel Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="9b31f-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="9b31f-105">Bu makalede, ASP.NET Core uygulamaları hataları işlemek için ortak appoaches yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="9b31f-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="9b31f-106">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9b31f-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="9b31f-107">Geliştirici özel durumu sayfası</span><span class="sxs-lookup"><span data-stu-id="9b31f-107">The developer exception page</span></span>

<span data-ttu-id="9b31f-108">Özel durumlar hakkında ayrıntılı bilgiler gösterilmektedir bir sayfasını görüntülemek için uygulamayı yapılandırmak için yükleme `Microsoft.AspNetCore.Diagnostics` NuGet paketini ve bir satırı ekleyin [başlangıç sınıfında yöntemini yapılandırma](xref:fundamentals/startup):</span><span class="sxs-lookup"><span data-stu-id="9b31f-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](xref:fundamentals/startup):</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="9b31f-109">PUT `UseDeveloperExceptionPage` Ara yazılımların, istediğiniz gibi özel durumları yakalamak için önce `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="9b31f-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="9b31f-110">Geliştirici özel durum sayfasını etkinleştir **yalnızca uygulama geliştirme ortamında çalışırken**.</span><span class="sxs-lookup"><span data-stu-id="9b31f-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="9b31f-111">Uygulama üretimde çalıştırıldığında ayrıntılı özel durum bilgilerini herkese açık şekilde paylaşma istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="9b31f-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="9b31f-112">[Ortamlarını yapılandırma hakkında daha fazla bilgi](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="9b31f-112">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="9b31f-113">Geliştirici özel durum sayfasını görmek için ayarlanan ortam ile örnek uygulamayı çalıştırın `Development`ve ekleme `?throw=true` uygulamanın temel URL.</span><span class="sxs-lookup"><span data-stu-id="9b31f-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="9b31f-114">Sayfa özel durumu ve istek hakkında bilgi içeren birkaç sekme içerir.</span><span class="sxs-lookup"><span data-stu-id="9b31f-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="9b31f-115">İlk sekme yığın izlemesi içerir.</span><span class="sxs-lookup"><span data-stu-id="9b31f-115">The first tab includes a stack trace.</span></span> 

![Yığın izleme](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="9b31f-117">Sonraki sekme sorgu dizesi parametreleri varsa gösterir.</span><span class="sxs-lookup"><span data-stu-id="9b31f-117">The next tab shows the query string parameters, if any.</span></span>

![Sorgu dizesi parametreleri](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="9b31f-119">Bu istek tanımlama bilgilerine sahip oldu, ancak olsaydı, üzerinde görünecekleri **tanımlama bilgilerini** sekmesi. Son sekmede geçirilmiş üstbilgileri görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9b31f-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Üstbilgileri](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="9b31f-121">Sayfa işleme özel bir durum yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9b31f-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="9b31f-122">Uygulama çalışmadığında kullanmak için bir özel durum işleyici sayfasını yapılandırmak `Development` ortamı.</span><span class="sxs-lookup"><span data-stu-id="9b31f-122">Configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="9b31f-123">Bir Razor sayfalarının uygulamasında [dotnet yeni](/dotnet/core/tools/dotnet-new) Razor sayfalarının şablonu bir hata sayfası sağlar ve `ErrorModel` sayfasında modeli sınıfında *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="9b31f-123">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="9b31f-124">Bir MVC uygulamasında HTTP yöntemi öznitelikleriyle hata işleyici eylem yöntemi gibi tasarlamanız yok `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="9b31f-124">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="9b31f-125">Açık fiiller bazı istekleri yöntemi erişmesini engelleyin.</span><span class="sxs-lookup"><span data-stu-id="9b31f-125">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="9b31f-126">Kimliği doğrulanmamış kullanıcıların hata görünümünün alabilir; böylece yöntemi anonim erişime izin verin.</span><span class="sxs-lookup"><span data-stu-id="9b31f-126">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="9b31f-127">Örneğin, aşağıdaki hata işleyici yöntemi tarafından sağlanan [dotnet yeni](/dotnet/core/tools/dotnet-new) MVC şablonu ve giriş denetleyicide görünür:</span><span class="sxs-lookup"><span data-stu-id="9b31f-127">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="9b31f-128">Yapılandırma durumu kod sayfaları</span><span class="sxs-lookup"><span data-stu-id="9b31f-128">Configuring status code pages</span></span>

<span data-ttu-id="9b31f-129">Varsayılan olarak, bir uygulama zengin durum kod sayfası için HTTP durum kodları, gibi sağlamaz *404 Bulunamadı*.</span><span class="sxs-lookup"><span data-stu-id="9b31f-129">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="9b31f-130">Kod sayfaları durumu sağlamak için bir satıra ekleyerek durum kodu sayfaları ara yazılımı yapılandırmanız `Startup.Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="9b31f-130">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="9b31f-131">Varsayılan olarak, durum kodu sayfaları ara yazılımı 404 gibi ortak durum kodları için basit, yalnızca metin işleyiciler ekler:</span><span class="sxs-lookup"><span data-stu-id="9b31f-131">By default, Status Code Pages Middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404 sayfası](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="9b31f-133">Ara yazılım birkaç uzantı yöntemleri destekler.</span><span class="sxs-lookup"><span data-stu-id="9b31f-133">The middleware supports several extension methods.</span></span> <span data-ttu-id="9b31f-134">Lambda ifadesi bir yöntemi alır:</span><span class="sxs-lookup"><span data-stu-id="9b31f-134">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="9b31f-135">Başka bir yöntem içerik türü ve biçim dizesini alır:</span><span class="sxs-lookup"><span data-stu-id="9b31f-135">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="9b31f-136">Ayrıca yönlendirmek ve genişletme yöntemleri yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9b31f-136">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="9b31f-137">Yeniden yönlendirme yöntemi 302 durum kodunu istemciye gönderir:</span><span class="sxs-lookup"><span data-stu-id="9b31f-137">The redirect method sends a 302 status code to the client:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="9b31f-138">Yeniden çalıştırma yöntemi istemciye özgün durum kodunu döndüren ancak işleyicinin yeniden yönlendirme URL'si de yürütür:</span><span class="sxs-lookup"><span data-stu-id="9b31f-138">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="9b31f-139">Bir Razor sayfalarının işleyici yöntemi veya MVC denetleyicisi belirli istekleri için durum kod sayfaları devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="9b31f-139">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="9b31f-140">Durum kod sayfaları devre dışı bırakmak için alma denemesi [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) gelen isteğin [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) koleksiyonu ve kullanılabilir durumdaysa özelliği devre dışı bırakın:</span><span class="sxs-lookup"><span data-stu-id="9b31f-140">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="9b31f-141">Kullanılıyorsa bir `UseStatusCodePages*` uygulama içinde uç noktalarına oluşturduğunuz bir MVC görünümü veya Razor sayfasını uç nokta için aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="9b31f-141">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="9b31f-142">Örneğin, [dotnet yeni](/dotnet/core/tools/dotnet-new) şablonu için bir Razor sayfalarının uygulaması aşağıdaki sayfasını ve sayfa model sınıfı üretir:</span><span class="sxs-lookup"><span data-stu-id="9b31f-142">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="9b31f-143">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9b31f-143">*Error.cshtml*:</span></span>

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications</strong>, as it can result in sensitive information from exceptions being displayed to end users. For local debugging, development environment can be enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="9b31f-144">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="9b31f-144">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="9b31f-145">Özel durum işleme kodu</span><span class="sxs-lookup"><span data-stu-id="9b31f-145">Exception-handling code</span></span>

<span data-ttu-id="9b31f-146">Özel durum işleme sayfaları kodda istisnalar atabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9b31f-146">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="9b31f-147">Genellikle, yalnızca statik içeriği oluşması üretim hata sayfaları için iyi bir fikir olabilir.</span><span class="sxs-lookup"><span data-stu-id="9b31f-147">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="9b31f-148">Ayrıca, yanıt üstbilgileri gönderildikten sonra yanıtın durum kodu değişiklik yapamaz veya herhangi bir özel durum sayfaları veya işleyicileri çalıştırabilirsiniz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9b31f-148">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="9b31f-149">Yanıt doldurulmalıdır veya bağlantı durduruldu.</span><span class="sxs-lookup"><span data-stu-id="9b31f-149">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="9b31f-150">Sunucu özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="9b31f-150">Server exception handling</span></span>

<span data-ttu-id="9b31f-151">Özel durum işleme mantığı, uygulamanızda yanı sıra [server](xref:fundamentals/servers/index) uygulamanızı barındırma bazı özel durum işleme gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="9b31f-151">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="9b31f-152">Üst bilgileri gönderilmeden önce sunucunun bir özel durum yakalar, sunucunun gönderir. bir *500 İç sunucu hatası* hiçbir gövdesi olan yanıt.</span><span class="sxs-lookup"><span data-stu-id="9b31f-152">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="9b31f-153">Üstbilgileri gönderildikten sonra sunucu bir özel durum yakalar, sunucu bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="9b31f-153">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="9b31f-154">Uygulamanız tarafından işlenmeyen isteği sunucu tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="9b31f-154">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="9b31f-155">Sunucunun bir özel durum nedeniyle oluşan herhangi bir özel durum işlenmiş işleme.</span><span class="sxs-lookup"><span data-stu-id="9b31f-155">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="9b31f-156">Herhangi bir özel hata sayfaları yapılandırılabilir veya özel durum işleme ara yazılımı veya filtreler bu davranışını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="9b31f-156">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="9b31f-157">Başlangıç özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="9b31f-157">Startup exception handling</span></span>

<span data-ttu-id="9b31f-158">Yalnızca barındırma katmanı uygulama başlatma sırasında gerçekleşmesi özel durumlar işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="9b31f-158">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="9b31f-159">Kullanarak [Web ana bilgisayarı](xref:fundamentals/host/web-host), yapabilecekleriniz [nasıl konak hatalarına yanıt olarak başlatma sırasında davranacağını yapılandırmak](xref:fundamentals/host/web-host#detailed-errors) ile `captureStartupErrors` ve `detailedErrors` anahtarları.</span><span class="sxs-lookup"><span data-stu-id="9b31f-159">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="9b31f-160">Ana bilgisayar adresini/bağlantı noktası sonra bağlama hatası oluşursa Barındırma yakalanan başlatma hatası için bir hata sayfası yalnızca gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="9b31f-160">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="9b31f-161">Hiçbir bağlama herhangi bir nedenle başarısız olursa, barındırma katman bir kritik özel durumu, dotnet işlem çökmesi günlüğe kaydeder ve uygulama çalışırken bir hata sayfası görüntülenir [Kestrel](xref:fundamentals/servers/kestrel) sunucu.</span><span class="sxs-lookup"><span data-stu-id="9b31f-161">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="9b31f-162">Çalışırken [IIS](/iis) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 işlem hatası* tarafından döndürülen [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) işlemi olamazsa başladı.</span><span class="sxs-lookup"><span data-stu-id="9b31f-162">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="9b31f-163">Sorun giderme tavsiyesini takip [IIS üzerinde ASP.NET Core sorun giderme](xref:host-and-deploy/iis/troubleshoot) konu.</span><span class="sxs-lookup"><span data-stu-id="9b31f-163">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="9b31f-164">ASP.NET MVC hata işleme</span><span class="sxs-lookup"><span data-stu-id="9b31f-164">ASP.NET MVC error handling</span></span>

<span data-ttu-id="9b31f-165">[MVC](xref:mvc/overview) uygulamalara sahip özel durum filtreleri yapılandırma ve model doğrulama gerçekleştirme gibi hataları işlemek için bazı ek seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="9b31f-165">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="9b31f-166">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="9b31f-166">Exception Filters</span></span>

<span data-ttu-id="9b31f-167">Özel durum filtreleri, genel olarak veya bir MVC uygulamasında her denetleyici veya eylem başına temelinde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="9b31f-167">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="9b31f-168">Bu filtreler bir denetleyici eylemi veya başka bir filtre yürütülmesi sırasında oluşan tüm işlenmeyen bir özel durum işleme ve aksi durumda adlı değil.</span><span class="sxs-lookup"><span data-stu-id="9b31f-168">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="9b31f-169">Özel durum filtreleri hakkında daha fazla bilgi [filtreleri](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="9b31f-169">Learn more about exception filters in [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="9b31f-170">Özel durum filtreleri içinde MVC Eylemler oluşan özel durumlarını yakalama için iyi, ancak bunlar hata ara yazılım işleme kadar esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="9b31f-170">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="9b31f-171">Ara yazılımı genel örneği için tercih ettiğiniz ve hata işleme yapmak için yalnızca ihtiyaç duyacağınız filtreleri kullanın *farklı* MVC eylemi seçildi tabanlı.</span><span class="sxs-lookup"><span data-stu-id="9b31f-171">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="9b31f-172">İşleme Model durumu hataları</span><span class="sxs-lookup"><span data-stu-id="9b31f-172">Handling Model State Errors</span></span>

<span data-ttu-id="9b31f-173">[Model doğrulama](xref:mvc/models/validation) her denetleyici eylemi çağırma öncesi oluşur ve incelemek için eylem yönteminin sorumluluğu olan `ModelState.IsValid` ve uygun şekilde tepki.</span><span class="sxs-lookup"><span data-stu-id="9b31f-173">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="9b31f-174">Model doğrulama hataları, ilgilenmek için standart bir kural, bu durumda izlemeniz gereken bazı uygulamalar seçecektir bir [filtre](xref:mvc/controllers/filters) böyle bir ilke uygulamak için uygun bir yerdir olabilir.</span><span class="sxs-lookup"><span data-stu-id="9b31f-174">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="9b31f-175">Geçersiz model durumlarıyla eylemlerinizi nasıl davranacağını test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9b31f-175">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="9b31f-176">Daha fazla bilgi edinin [Test denetleyicisi mantığı](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="9b31f-176">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>
