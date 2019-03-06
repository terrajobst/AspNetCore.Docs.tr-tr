---
title: ASP.NET core'da hatalarını işleme
author: tdykstra
description: ASP.NET Core uygulamaları hataları işlemek nasıl keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/01/2019
uid: fundamentals/error-handling
ms.openlocfilehash: a2ae2cb25c8cc5048b189b4035abbfc32a29aaff
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345511"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="ba7bb-103">ASP.NET core'da hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="ba7bb-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="ba7bb-104">Tarafından [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ba7bb-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ba7bb-105">Bu makalede, ASP.NET Core uygulamalarında hata işleme için ortak bir yaklaşım ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="ba7bb-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ba7bb-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="ba7bb-107">Geliştirici özel durumu sayfası</span><span class="sxs-lookup"><span data-stu-id="ba7bb-107">Developer Exception Page</span></span>

<span data-ttu-id="ba7bb-108">Özel durumları hakkında ayrıntılı bilgi gösteren bir sayfasını görüntülemek için bir uygulamayı yapılandırmak için kullanın *Geliştirici özel durum sayfasında*.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-108">To configure an app to display a page that shows detailed information about exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="ba7bb-109">Sayfa tarafından kullanılabilir hale getirileceğini [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) kullanılabilir paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ba7bb-109">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="ba7bb-110">Bir satırı `Startup.Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-110">Add a line to the `Startup.Configure` method:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=5)]

<span data-ttu-id="ba7bb-111">Çağrı yapmak <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> önüne özel durumları yakalamak istediğiniz herhangi bir ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-111">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> in front of any middleware where you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="ba7bb-112">Geliştirici özel durumu sayfası etkinleştirme **yalnızca geliştirme ortamında uygulama çalışırken**.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-112">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="ba7bb-113">Üretim ortamında uygulama çalıştığında ayrıntılı özel durum bilgileri herkese açık şekilde paylaşma istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="ba7bb-114">Ortamları yapılandırma hakkında daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-114">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="ba7bb-115">Geliştirici özel durumu sayfasını görmek için ortamı ayarlamak örnek uygulamayı çalıştırma `Development` ve ekleme `?throw=true` uygulamasının temel URL'si.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-115">To see the Developer Exception Page, run the sample app with the environment set to `Development` and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="ba7bb-116">Sayfasında, özel durum ve isteği hakkında aşağıdaki bilgileri içerir:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="ba7bb-117">Yığın izlemesi</span><span class="sxs-lookup"><span data-stu-id="ba7bb-117">Stack trace</span></span>
* <span data-ttu-id="ba7bb-118">Dize parametreleri (varsa) sorgulama</span><span class="sxs-lookup"><span data-stu-id="ba7bb-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="ba7bb-119">Tanımlama bilgileri (varsa)</span><span class="sxs-lookup"><span data-stu-id="ba7bb-119">Cookies (if any)</span></span>
* <span data-ttu-id="ba7bb-120">Üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="ba7bb-120">Headers</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="ba7bb-121">Sayfa işleme özel bir özel durum yapılandırın</span><span class="sxs-lookup"><span data-stu-id="ba7bb-121">Configure a custom exception handling page</span></span>

<span data-ttu-id="ba7bb-122">Uygulama geliştirme ortamında çalışmadığı aramanızı <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> özel durum işleme ara yazılım eklemek için genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-122">When the app isn't running in the Development environment, call the <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> extension method to add Exception Handling Middleware.</span></span> <span data-ttu-id="ba7bb-123">Ara yazılım:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-123">The middleware:</span></span>

* <span data-ttu-id="ba7bb-124">Özel durumu yakalar.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-124">Catches exceptions.</span></span>
* <span data-ttu-id="ba7bb-125">Özel durumları günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-125">Logs exceptions.</span></span>
* <span data-ttu-id="ba7bb-126">Belirtilen denetleyici ve sayfa için alternatif bir işlem hattı istekte yeniden yürütür.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="ba7bb-127">İstek, yanıt başladıysa, yeniden çalıştırılır değildir.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="ba7bb-128">Aşağıdaki örnekte, örnek uygulamadan <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> geliştirme olmayan ortamlarda özel durum işleme ara yazılım ekler.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-128">In the following example from the sample app, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments.</span></span> <span data-ttu-id="ba7bb-129">Bir hata sayfası veya denetleyicisinde genişletme yöntemi belirler `/Error` özel durum yakalandı ve günlüğe sonra yeniden yürütülen istekler için uç nokta:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-129">The extension method specifies an error page or controller at the `/Error` endpoint for re-executed requests after exceptions are caught and logged:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=9)]

<span data-ttu-id="ba7bb-130">Bir hata sayfası Razor sayfaları uygulaması şablonunu sunar (*.cshtml*) ve <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> sınıfı (`ErrorModel`) sayfalar klasöründe.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-130">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the Pages folder.</span></span>

<span data-ttu-id="ba7bb-131">Aşağıdaki hata işleyicisi yöntemi bir MVC uygulamasında MVC uygulama şablonuna dahil edilir ve giriş denetleyicisine içinde görünür:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-131">In an MVC app, the following error handler method is included in the MVC app template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="ba7bb-132">HTTP yöntemi öznitelikleriyle hata işleyicisi eylem yöntemi gibi süslemek yoksa `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="ba7bb-133">Açık fiilleri yöntemi bazı istekleri engellenir.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="ba7bb-134">Kimliği doğrulanmamış kullanıcılar hata görünümünün alabilirsiniz, böylece yöntemi anonim erişime izin verin.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

## <a name="configure-status-code-pages"></a><span data-ttu-id="ba7bb-135">Durum kod sayfaları yapılandırın</span><span class="sxs-lookup"><span data-stu-id="ba7bb-135">Configure status code pages</span></span>

<span data-ttu-id="ba7bb-136">Varsayılan olarak, ASP.NET Core uygulaması durumu kod sayfası için HTTP durum kodları, gibi sağlamaz *404 - Bulunamadı*.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-136">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="ba7bb-137">Uygulama, durum kodu ve boş yanıt gövdesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-137">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="ba7bb-138">Kod sayfaları durumu sağlamak için durum kodu sayfa ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-138">To provide status code pages, use Status Code Pages Middleware.</span></span>

<span data-ttu-id="ba7bb-139">Ara yazılım tarafından kullanılabilir hale getirileceğini [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) kullanılabilir paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ba7bb-139">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="ba7bb-140">Bir satırı `Startup.Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-140">Add a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="ba7bb-141">Çağrı <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> önce ara (örneğin, statik dosya ara yazılımlarını ve MVC ara yazılım) işleme istek yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-141">Call the <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> method before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="ba7bb-142">Gibi yaygın durum kodları için salt metin işleyiciler varsayılan olarak, durum kodu sayfa ara yazılımı ekler *404 - Bulunamadı*:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-142">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as *404 - Not Found*:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="ba7bb-143">Ara yazılım davranışını özelleştirmenizi birkaç genişletme yöntemleri destekler.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-143">The middleware supports several extension methods that allow you to customize its behavior.</span></span>

<span data-ttu-id="ba7bb-144">Bir aşırı yüklemesini <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> özel hata işleme mantığı işlemek ve el ile yanıt yazmak için kullanabileceğiniz bir lambda ifadesini alır:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-144">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a lambda expression, which you can use to process custom error-handling logic and manually write the response:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="ba7bb-145">Bir aşırı yüklemesini <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> içerik türü ve yanıt metni özelleştirmek için kullanabileceğiniz bir içerik türü ve biçim dizesini alır:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-145">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a content type and format string, which you can use to customize the content type and response text:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a><span data-ttu-id="ba7bb-146">Yeniden yönlendirme uzantısı yöntemleri ve yeniden yürütme</span><span class="sxs-lookup"><span data-stu-id="ba7bb-146">Redirect and re-execute extension methods</span></span>

<span data-ttu-id="ba7bb-147"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-147"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span></span>

* <span data-ttu-id="ba7bb-148">Gönderen bir *302 bulundu -* istemciye durum kodu.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-148">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="ba7bb-149">İstemci URL'si şablonda verilen konuma yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-149">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="ba7bb-150"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> yaygın olarak kullanıldığında uygulama:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-150"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> is commonly used when the app:</span></span>

* <span data-ttu-id="ba7bb-151">Genellikle, farklı bir uygulama hatası burada işler durumlarda farklı bir uç noktası için istemci yönlendirmelidir.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-151">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="ba7bb-152">Web apps için yeniden yönlendirilen uç nokta istemcinin tarayıcınızın adres çubuğuna yansıtır.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-152">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="ba7bb-153">Olmamalıdır korumak ve özgün ilk yönlendirme yanıt durum koduyla döndürür.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-153">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

<span data-ttu-id="ba7bb-154"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-154"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span></span>

* <span data-ttu-id="ba7bb-155">Özgün durum kodunu istemciye döndürür.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-155">Returns the original status code to the client.</span></span>
* <span data-ttu-id="ba7bb-156">Yanıt gövdesi, alternatif bir yol kullanarak istek ardışık düzenini yeniden yürüterek oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-156">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<span data-ttu-id="ba7bb-157"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> Uygulama sırasında yaygın olarak kullanılır:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-157"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> is commonly used when the app should:</span></span>

* <span data-ttu-id="ba7bb-158">İstek farklı bir uç noktasına yönlendirme olmadan işlem.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-158">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="ba7bb-159">Web apps için başlangıçta istenen uç noktası istemcinin tarayıcınızın adres çubuğuna yansıtır.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-159">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="ba7bb-160">Korumak ve özgün durum koduyla bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-160">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="ba7bb-161">Şablonları, bir yer tutucu içerebilir (`{0}`) durum kodu.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-161">Templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="ba7bb-162">Şablonu bir eğik çizgi ile başlamalıdır (`/`).</span><span class="sxs-lookup"><span data-stu-id="ba7bb-162">The template must start with a forward slash (`/`).</span></span> <span data-ttu-id="ba7bb-163">Bir yer tutucu kullanırken, uç noktayı (sayfa veya denetleyicisi) yol kesimi işleyebilir onaylayın.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-163">When using a placeholder, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="ba7bb-164">Örneğin, hatalar için bir Razor sayfası ile isteğe bağlı yol kesimi değerini kabul etmelidir `@page` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-164">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="ba7bb-165">Durum kod sayfaları için Razor sayfaları işleyicisi yöntemi veya MVC denetleyicisi belirli isteklere devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-165">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="ba7bb-166">Durum kod sayfaları devre dışı bırakmak için alma denemesi <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> gelen isteğin [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) toplama ve varsa bu özelliği devre dışı bırak:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-166">To disable status code pages, attempt to retrieve the <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> from the request's [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="ba7bb-167">Kullanılacak bir <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> noktaları bir uç noktaya uygulama içinde oluşturduğunuz bir MVC görünümü ya da bir Razor sayfası uç nokta için aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-167">To use a <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="ba7bb-168">Razor sayfaları uygulama şablonu, örneğin, aşağıdaki sayfasını ve sayfa modeli sınıfı üretir:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-168">For example, the Razor Pages app template produces the following page and page model class:</span></span>

<span data-ttu-id="ba7bb-169">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-169">*Error.cshtml*:</span></span>

::: moniker range=">= aspnetcore-2.2"

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
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

<span data-ttu-id="ba7bb-170">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-170">*Error.cshtml.cs*:</span></span>

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

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

<span data-ttu-id="ba7bb-171">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-171">*Error.cshtml.cs*:</span></span>

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

::: moniker-end

## <a name="exception-handling-code"></a><span data-ttu-id="ba7bb-172">Özel durum işleme kodu</span><span class="sxs-lookup"><span data-stu-id="ba7bb-172">Exception-handling code</span></span>

<span data-ttu-id="ba7bb-173">Özel durum işleme sayfaları kodda özel durumlar.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-173">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="ba7bb-174">Genellikle, tamamen statik içeriği oluşmalıdır üretim hata sayfaları için iyi bir fikir olduğunu.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-174">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="ba7bb-175">Ayrıca, dikkat edin, yanıt üst bilgileri gönderdikten sonra:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-175">Also, be aware that once the headers for a response are sent:</span></span>

* <span data-ttu-id="ba7bb-176">Uygulama, yanıtın durum kodu değiştiremezsiniz.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-176">The app can't change the response's status code.</span></span>
* <span data-ttu-id="ba7bb-177">Herhangi bir özel durum sayfaları veya işleyicileri çalıştıramazsınız.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-177">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="ba7bb-178">Yanıt tamamlanması gereken veya bağlantı kesildi.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-178">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="ba7bb-179">Sunucu özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="ba7bb-179">Server exception handling</span></span>

<span data-ttu-id="ba7bb-180">Özel durum işleme mantığı, uygulamanıza ek olarak [sunucusu uygulaması](xref:fundamentals/servers/index) bazı özel durumları işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-180">In addition to the exception handling logic in your app, the [server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="ba7bb-181">Yanıt üst bilgileri gönderilmeden önce sunucunun bir özel durumu yakalar, sunucunun gönderdiği bir *500 - İç sunucu hatası* yanıt gövdesi olmadan yanıt.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-181">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="ba7bb-182">Yanıt üstbilgileri gönderildikten sonra sunucu bir özel durumu yakalar, sunucu bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-182">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="ba7bb-183">Uygulamanız tarafından işlenmeyen isteği sunucu tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-183">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="ba7bb-184">Oluşan özel sunucu özel durum tarafından işlenir işleme.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-184">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="ba7bb-185">Herhangi bir özel hata sayfaları yapılandırılmış veya özel durum işleme ara yazılım veya filtreleri bu davranışı etkilemez.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-185">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="ba7bb-186">Başlangıç özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="ba7bb-186">Startup exception handling</span></span>

<span data-ttu-id="ba7bb-187">Yalnızca barındırma katmanı, uygulama başlatma sırasında gerçekleşmesi özel durumları işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-187">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="ba7bb-188">Kullanarak [Web ana bilgisayarı](xref:fundamentals/host/web-host), yapabilecekleriniz [nasıl konak hatalara yanıt başlatma sırasında davranacağını yapılandırmak](xref:fundamentals/host/web-host#detailed-errors) ile `captureStartupErrors` ve `detailedErrors` anahtarları.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-188">Using [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="ba7bb-189">Ana bilgisayar adresi/bağlantı noktası sonra bağlama bir hata oluşursa bir hata sayfası için yakalanan başlatma hatası barındırma yalnızca gösterebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-189">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="ba7bb-190">Bağlama için herhangi bir nedenle başarısız olursa:</span><span class="sxs-lookup"><span data-stu-id="ba7bb-190">If any binding fails for any reason:</span></span>

* <span data-ttu-id="ba7bb-191">Barındırma katman kritik bir özel durumu günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-191">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="ba7bb-192">Dotnet işlem kilitleniyor.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-192">The dotnet process crashes.</span></span>
* <span data-ttu-id="ba7bb-193">Uygulama çalışırken, hiçbir hata sayfası görüntülenir [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-193">No error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="ba7bb-194">Çalışırken [IIS](/iis) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 - işlem hatası* tarafından döndürülen [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) işlemi başlatılamazsa .</span><span class="sxs-lookup"><span data-stu-id="ba7bb-194">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="ba7bb-195">Daha fazla bilgi için bkz. <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-195">For more information, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="ba7bb-196">Azure App Service ile başlatma sorunlarını giderme hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-196">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="ba7bb-197">ASP.NET Core MVC hata işleme</span><span class="sxs-lookup"><span data-stu-id="ba7bb-197">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="ba7bb-198">[MVC](xref:mvc/overview) uygulamalara sahip özel durum filtreleri yapılandırma ve model doğrulama gerçekleştirme gibi hataları işlemek için bazı ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-198">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="ba7bb-199">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="ba7bb-199">Exception filters</span></span>

<span data-ttu-id="ba7bb-200">Özel durum filtreleri, genel olarak veya bir MVC uygulamasında her denetleyici veya eylem başına temelinde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-200">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="ba7bb-201">Bu filtreler bir denetleyici eylemi veya başka bir filtre yürütülmesi sırasında oluşan tüm işlenmeyen bir özel durumu işle.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-201">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="ba7bb-202">Bu filtreler, aksi takdirde çağrılır değil.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-202">These filters aren't called otherwise.</span></span> <span data-ttu-id="ba7bb-203">Daha fazla bilgi için bkz: <xref:mvc/controllers/filters>.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-203">To learn more, see <xref:mvc/controllers/filters>.</span></span>

> [!TIP]
> <span data-ttu-id="ba7bb-204">Özel durum filtreleri MVC Eylemler içinde oluşan özel durumları yakalama için kullanışlıdır ancak bunlar ara yazılım işleme hata olarak kadar esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-204">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="ba7bb-205">Ara yazılım kullanılmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-205">We recommend the use of middleware.</span></span> <span data-ttu-id="ba7bb-206">Hata işleme gerçekleştirmek için yalnızca gerek duyduğunuz filtrelerini kullanma *farklı* göre MVC eylemi seçilir.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-206">Use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handle-model-state-errors"></a><span data-ttu-id="ba7bb-207">Model durumu hataları işleme</span><span class="sxs-lookup"><span data-stu-id="ba7bb-207">Handle model state errors</span></span>

<span data-ttu-id="ba7bb-208">[Model doğrulama](xref:mvc/models/validation) her denetleyici eylemi çağırma öncesinde gerçekleşir ve incelemek için eylem yönteminin sorumluluğu olan [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) ve uygun şekilde tepki verin.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-208">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) and react appropriately.</span></span>

<span data-ttu-id="ba7bb-209">Başa çıkmak için standart bir kural izlemek bazı uygulamalar seçin [model doğrulama](xref:mvc/models/validation) durumda hataları bir [filtre](xref:mvc/controllers/filters) böyle bir ilke uygulamak için uygun bir yere olabilir.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-209">Some apps choose to follow a standard convention for dealing with [model validation](xref:mvc/models/validation) errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="ba7bb-210">Eylemlerinizi ile geçersiz model durumlarının nasıl davranacağını test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-210">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="ba7bb-211">Daha fazla bilgi için bkz. <xref:mvc/controllers/testing>.</span><span class="sxs-lookup"><span data-stu-id="ba7bb-211">For more information, see <xref:mvc/controllers/testing>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ba7bb-212">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ba7bb-212">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
