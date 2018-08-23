---
title: ASP.NET core'da hatalarını işleme
author: ardalis
description: ASP.NET Core uygulamaları hataları işlemek nasıl keşfedin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: ff04ebeb6a682ec924afe896fd6716010a63f7cd
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753948"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="8a7de-103">ASP.NET core'da hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="8a7de-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="8a7de-104">Tarafından [Steve Smith](https://ardalis.com/) ve [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="8a7de-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="8a7de-105">Bu makalede, ASP.NET Core uygulamalarında hata işleme için ortak bir yaklaşım ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="8a7de-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="8a7de-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8a7de-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="8a7de-107">Geliştirici özel durumu sayfası</span><span class="sxs-lookup"><span data-stu-id="8a7de-107">The developer exception page</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8a7de-108">Özel durumları hakkında ayrıntılı bilgi gösteren bir sayfasını görüntülemek için bir uygulamayı yapılandırmak için kullanın *Geliştirici özel durum sayfasında*.</span><span class="sxs-lookup"><span data-stu-id="8a7de-108">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="8a7de-109">Tarafından kullanıma sunulan sayfayı [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) kullanılabilir paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="8a7de-109">The page made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="8a7de-110">Bir satırı `Startup.Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8a7de-110">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8a7de-111">Özel durumları hakkında ayrıntılı bilgi gösteren bir sayfasını görüntülemek için bir uygulamayı yapılandırmak için kullanın *Geliştirici özel durum sayfasında*.</span><span class="sxs-lookup"><span data-stu-id="8a7de-111">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="8a7de-112">Sayfa tarafından kullanılabilir hale getirileceğini [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) kullanılabilir paketini [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="8a7de-112">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="8a7de-113">Bir satırı `Startup.Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8a7de-113">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8a7de-114">Özel durumları hakkında ayrıntılı bilgi gösteren bir sayfasını görüntülemek için bir uygulamayı yapılandırmak için kullanın *Geliştirici özel durum sayfasında*.</span><span class="sxs-lookup"><span data-stu-id="8a7de-114">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="8a7de-115">Sayfa için bir paket başvurusu ekleme tarafından kullanılabilir hale getirileceğini [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) proje dosyasındaki paket.</span><span class="sxs-lookup"><span data-stu-id="8a7de-115">The page is made available by adding a package reference for the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package in the project file.</span></span> <span data-ttu-id="8a7de-116">Bir satırı `Startup.Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8a7de-116">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="8a7de-117">Çağrı yapmak [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) Ara yazılımların, istediğiniz gibi özel durumları yakalamak için önüne `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="8a7de-117">Place the call to [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) in front of any middleware where you want to catch exceptions, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="8a7de-118">Geliştirici özel durumu sayfası etkinleştirme **yalnızca geliştirme ortamında uygulama çalışırken**.</span><span class="sxs-lookup"><span data-stu-id="8a7de-118">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="8a7de-119">Üretim ortamında uygulama çalıştığında ayrıntılı özel durum bilgileri herkese açık şekilde paylaşma istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="8a7de-119">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="8a7de-120">[Ortamları yapılandırma hakkında daha fazla bilgi edinin](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="8a7de-120">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="8a7de-121">Geliştirici özel durumu sayfasını görmek için ortamı ayarlamak örnek uygulamayı çalıştırma `Development`ve ekleme `?throw=true` uygulamasının temel URL'si.</span><span class="sxs-lookup"><span data-stu-id="8a7de-121">To see the developer exception page, run the sample app with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="8a7de-122">Sayfanın birkaç sekme özel durum ve isteği hakkında bilgi içerir.</span><span class="sxs-lookup"><span data-stu-id="8a7de-122">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="8a7de-123">Bir yığın izlemesi ilk sekme içerir:</span><span class="sxs-lookup"><span data-stu-id="8a7de-123">The first tab includes a stack trace:</span></span>

![Yığın izlemesi](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="8a7de-125">Sonraki sekme varsa sorgu dizesi parametreleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="8a7de-125">The next tab shows the query string parameters, if any:</span></span>

![Sorgu dizesi parametreleri](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="8a7de-127">İstek tanımlama bilgisi varsa, göründükleri **tanımlama bilgilerini** sekmesi. Üst son sekmede görülür:</span><span class="sxs-lookup"><span data-stu-id="8a7de-127">If the request has cookies, they appear on the **Cookies** tab. Headers are seen in the last tab:</span></span>

![Üst bilgileri](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="8a7de-129">Sayfa işleme bir özel durum yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8a7de-129">Configuring a custom exception handling page</span></span>

<span data-ttu-id="8a7de-130">Uygulama çalışmadığında kullanmak için bir özel durum işleyicisi sayfası yapılandırma `Development` ortam:</span><span class="sxs-lookup"><span data-stu-id="8a7de-130">Configure an exception handler page to use when the app isn't running in the `Development` environment:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="8a7de-131">Bir Razor sayfaları uygulamasında [yeni dotnet](/dotnet/core/tools/dotnet-new) Razor sayfaları şablonu, bir hata sayfası sağlar ve `ErrorModel` sayfa modeli sınıfında *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="8a7de-131">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="8a7de-132">Bir MVC uygulamasında HTTP yöntemi öznitelikleriyle hata işleyicisi eylem yöntemi gibi süslemek yoksa `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="8a7de-132">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="8a7de-133">Açık fiilleri yöntemi bazı istekleri engellenir.</span><span class="sxs-lookup"><span data-stu-id="8a7de-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="8a7de-134">Kimliği doğrulanmamış kullanıcılar hata görünümünün alabilirsiniz, böylece yöntemi anonim erişime izin verin.</span><span class="sxs-lookup"><span data-stu-id="8a7de-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="8a7de-135">Örneğin, aşağıdaki hata işleyicisi yöntemi tarafından sağlanan [yeni dotnet](/dotnet/core/tools/dotnet-new) MVC şablonu ve giriş denetleyicisine içinde görünür:</span><span class="sxs-lookup"><span data-stu-id="8a7de-135">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="8a7de-136">Yapılandırma durumu kod sayfaları</span><span class="sxs-lookup"><span data-stu-id="8a7de-136">Configuring status code pages</span></span>

<span data-ttu-id="8a7de-137">Varsayılan olarak, bir uygulama durumu zengin kod sayfası için HTTP durum kodları, gibi sağlamaz *404 Bulunamadı*.</span><span class="sxs-lookup"><span data-stu-id="8a7de-137">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="8a7de-138">Kod sayfaları durumu sağlamak için bir satıra ekleyerek durumu kod sayfaları ara yazılım yapılandırma `Startup.Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8a7de-138">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="8a7de-139">Varsayılan olarak, yalnızca metin işleyicileri 404 gibi yaygın durum kodları için durum kod sayfaları ara yazılım ekler:</span><span class="sxs-lookup"><span data-stu-id="8a7de-139">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as 404:</span></span>

![404 sayfa](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="8a7de-141">Ara yazılım birkaç uzantı yöntemleri destekler.</span><span class="sxs-lookup"><span data-stu-id="8a7de-141">The middleware supports several extension methods.</span></span> <span data-ttu-id="8a7de-142">Bir yöntem, lambda ifadesi alır:</span><span class="sxs-lookup"><span data-stu-id="8a7de-142">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="8a7de-143">Başka bir yöntemi, bir içerik türü ve biçim dizesini alır:</span><span class="sxs-lookup"><span data-stu-id="8a7de-143">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="8a7de-144">Ayrıca yeniden yönlendirme ve genişletme yöntemlerini yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8a7de-144">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="8a7de-145">Yeniden yönlendirme yöntemini gönderen bir *302 bulundu* istemciye durum kodunu ve istemci için belirtilen konum URL şablonu yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="8a7de-145">The redirect method sends a *302 Found* status code to the client and redirects the client to the provided location URL template.</span></span> <span data-ttu-id="8a7de-146">Şablon içerebilir bir `{0}` durum kodu için yer tutucu.</span><span class="sxs-lookup"><span data-stu-id="8a7de-146">The template may include a `{0}` placeholder for the status code.</span></span> <span data-ttu-id="8a7de-147">İle başlayan URL'ler `~` başına temel yol vardır.</span><span class="sxs-lookup"><span data-stu-id="8a7de-147">URLs starting with `~` have the base path prepended.</span></span> <span data-ttu-id="8a7de-148">İle başlamaz bir URL `~` olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8a7de-148">A URL that doesn't start with `~` is used as is.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="8a7de-149">Yeniden çalıştırma yöntemi istemciye özgün durum kodu döndürür ve yanıt gövdesinin başka bir yol kullanarak istek ardışık düzenini yeniden yürüterek oluşturulacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="8a7de-149">The re-execute method returns the original status code to the client and specifies that the response body should be generated by re-executing the request pipeline using an alternate path.</span></span> <span data-ttu-id="8a7de-150">Bu yolu içerebilir bir `{0}` durum kodu için yer tutucu:</span><span class="sxs-lookup"><span data-stu-id="8a7de-150">This path may contain a `{0}` placeholder for the status code:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="8a7de-151">Durum kod sayfaları için Razor sayfaları işleyicisi yöntemi veya MVC denetleyicisi belirli isteklere devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="8a7de-151">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="8a7de-152">Durum kod sayfaları devre dışı bırakmak için alma denemesi [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) gelen isteğin [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) toplama ve varsa bu özelliği devre dışı bırak:</span><span class="sxs-lookup"><span data-stu-id="8a7de-152">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="8a7de-153">Kullanılıyorsa bir `UseStatusCodePages*` noktaları bir uç noktaya uygulama içinde oluşturduğunuz bir MVC görünümü ya da bir Razor sayfası uç nokta için aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="8a7de-153">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="8a7de-154">Örneğin, [yeni dotnet](/dotnet/core/tools/dotnet-new) aşağıdaki sayfasını ve sayfa modeli sınıfı için bir Razor sayfaları uygulaması şablonu üretir:</span><span class="sxs-lookup"><span data-stu-id="8a7de-154">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="8a7de-155">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8a7de-155">*Error.cshtml*:</span></span>

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
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="8a7de-156">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="8a7de-156">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="8a7de-157">Özel durum işleme kodu</span><span class="sxs-lookup"><span data-stu-id="8a7de-157">Exception-handling code</span></span>

<span data-ttu-id="8a7de-158">Özel durum işleme sayfaları kodda özel durumlar.</span><span class="sxs-lookup"><span data-stu-id="8a7de-158">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="8a7de-159">Genellikle, tamamen statik içeriği oluşmalıdır üretim hata sayfaları için iyi bir fikir olduğunu.</span><span class="sxs-lookup"><span data-stu-id="8a7de-159">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="8a7de-160">Ayrıca, yanıt üstbilgileri gönderildikten sonra yanıtın durum kodu değişiklik yapamaz veya herhangi bir özel durum sayfaları veya işleyicileri çalıştırabilirsiniz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="8a7de-160">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="8a7de-161">Yanıt tamamlanması gereken veya bağlantı kesildi.</span><span class="sxs-lookup"><span data-stu-id="8a7de-161">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="8a7de-162">Sunucu özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="8a7de-162">Server exception handling</span></span>

<span data-ttu-id="8a7de-163">Özel durum işleme mantığı, uygulamanıza ek olarak [sunucu](xref:fundamentals/servers/index) uygulamanızı barındıran bazı özel durum işleme gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="8a7de-163">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="8a7de-164">Üst bilgileri gönderilmeden önce sunucunun bir özel durumu yakalar, sunucunun gönderdiği bir *500 İç sunucu hatası* hiçbir gövdesi olan yanıt.</span><span class="sxs-lookup"><span data-stu-id="8a7de-164">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="8a7de-165">Üstbilgileri gönderildikten sonra sunucu bir özel durumu yakalar, sunucu bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="8a7de-165">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="8a7de-166">Uygulamanız tarafından işlenmeyen isteği sunucu tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="8a7de-166">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="8a7de-167">Oluşan özel sunucu özel durum tarafından işlenir işleme.</span><span class="sxs-lookup"><span data-stu-id="8a7de-167">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="8a7de-168">Herhangi bir özel hata sayfaları yapılandırılmış veya özel durum işleme ara yazılım veya filtreleri bu davranışı etkilemez.</span><span class="sxs-lookup"><span data-stu-id="8a7de-168">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="8a7de-169">Başlangıç özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="8a7de-169">Startup exception handling</span></span>

<span data-ttu-id="8a7de-170">Yalnızca barındırma katmanı, uygulama başlatma sırasında gerçekleşmesi özel durumları işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="8a7de-170">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="8a7de-171">Kullanarak [Web ana bilgisayarı](xref:fundamentals/host/web-host), yapabilecekleriniz [nasıl konak hatalara yanıt başlatma sırasında davranacağını yapılandırmak](xref:fundamentals/host/web-host#detailed-errors) ile `captureStartupErrors` ve `detailedErrors` anahtarları.</span><span class="sxs-lookup"><span data-stu-id="8a7de-171">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="8a7de-172">Ana bilgisayar adresi/bağlantı noktası sonra bağlama bir hata oluşursa bir hata sayfası için yakalanan başlatma hatası barındırma yalnızca gösterebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8a7de-172">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="8a7de-173">Bağlama için herhangi bir nedenle başarısız olursa, barındırma katman kritik özel durum, dotnet işlem çökmesi günlüğe kaydeder ve uygulamayı çalıştıran herhangi bir hata sayfası görüntülenir [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="8a7de-173">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="8a7de-174">Çalışırken [IIS](/iis) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 işlem hatası* tarafından döndürülen [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) işlem yoksa başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="8a7de-174">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="8a7de-175">IIS ile barındırırken başlatma sorunlarını giderme hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="8a7de-175">For information on troubleshooting startup issues when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="8a7de-176">Azure App Service ile başlatma sorunlarını giderme hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="8a7de-176">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="8a7de-177">ASP.NET Core MVC hata işleme</span><span class="sxs-lookup"><span data-stu-id="8a7de-177">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="8a7de-178">[MVC](xref:mvc/overview) uygulamalara sahip özel durum filtreleri yapılandırma ve model doğrulama gerçekleştirme gibi hataları işlemek için bazı ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="8a7de-178">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="8a7de-179">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="8a7de-179">Exception filters</span></span>

<span data-ttu-id="8a7de-180">Özel durum filtreleri, genel olarak veya bir MVC uygulamasında her denetleyici veya eylem başına temelinde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="8a7de-180">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="8a7de-181">Bu filtreler bir denetleyici eylemi veya başka bir filtre yürütülmesi sırasında oluşan tüm işlenmeyen bir özel durumu işle.</span><span class="sxs-lookup"><span data-stu-id="8a7de-181">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="8a7de-182">Bu filtreler, aksi takdirde çağrılır değil.</span><span class="sxs-lookup"><span data-stu-id="8a7de-182">These filters aren't called otherwise.</span></span> <span data-ttu-id="8a7de-183">Daha fazla bilgi için bkz. [filtreleri](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="8a7de-183">To learn more, see [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="8a7de-184">Özel durum filtreleri, MVC Eylemler içinde oluşan özel durumları yakalama için uygundur, ancak bunlar ara yazılım işleme hata olarak kadar esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="8a7de-184">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="8a7de-185">Genel olarak ara yazılım kullanımını tercih ve hata işleme gerçekleştirmek için yalnızca gerek duyduğunuz filtrelerini kullanma *farklı* göre MVC eylemi seçilir.</span><span class="sxs-lookup"><span data-stu-id="8a7de-185">Generally prefer the use of middleware, and use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="8a7de-186">Model durumu hataları işleme</span><span class="sxs-lookup"><span data-stu-id="8a7de-186">Handling model state errors</span></span>

<span data-ttu-id="8a7de-187">[Model doğrulama](xref:mvc/models/validation) her denetleyici eylemi çağırma öncesinde gerçekleşir ve incelemek için eylem yönteminin sorumluluğu olan `ModelState.IsValid` ve uygun şekilde tepki verin.</span><span class="sxs-lookup"><span data-stu-id="8a7de-187">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="8a7de-188">Model doğrulama hataları, başa çıkmak için standart bir kural, bu durumda izlemek bazı uygulamalar seçin bir [filtre](xref:mvc/controllers/filters) böyle bir ilke uygulamak için uygun bir yere olabilir.</span><span class="sxs-lookup"><span data-stu-id="8a7de-188">Some apps choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="8a7de-189">Eylemlerinizi ile geçersiz model durumlarının nasıl davranacağını test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a7de-189">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="8a7de-190">Daha fazla bilgi [Test denetleyicisi mantığı](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="8a7de-190">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8a7de-191">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8a7de-191">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
