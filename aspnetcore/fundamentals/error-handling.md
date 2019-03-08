---
title: ASP.NET core'da hatalarını işleme
author: tdykstra
description: ASP.NET Core uygulamaları hataları işlemek nasıl keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/05/2019
uid: fundamentals/error-handling
ms.openlocfilehash: d809c70b3fae6b2d21d5ec0871298d905b873d5d
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665369"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="e6c09-103">ASP.NET core'da hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="e6c09-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="e6c09-104">Tarafından [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e6c09-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e6c09-105">Bu makalede, ASP.NET Core uygulamalarında hata işleme için ortak bir yaklaşım ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="e6c09-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="e6c09-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e6c09-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="e6c09-107">Geliştirici özel durumu sayfası</span><span class="sxs-lookup"><span data-stu-id="e6c09-107">Developer Exception Page</span></span>

<span data-ttu-id="e6c09-108">İstek özel durumları hakkında ayrıntılı bilgi gösteren bir sayfasını görüntülemek için bir uygulamayı yapılandırmak için kullanın *Geliştirici özel durum sayfasında*.</span><span class="sxs-lookup"><span data-stu-id="e6c09-108">To configure an app to display a page that shows detailed information about request exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="e6c09-109">Sayfa tarafından kullanılabilir hale getirileceğini [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) kullanılabilir paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e6c09-109">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="e6c09-110">Bir satırı `Startup.Configure` uygulama geliştirmesinde çalışırken yöntemi [ortam](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="e6c09-110">Add a line to the `Startup.Configure` method when the app is running in the Development [environment](xref:fundamentals/environments):</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseDeveloperExceptionPage)]

<span data-ttu-id="e6c09-111">Çağrı yapmak <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> önüne özel durumları yakalamak istediğiniz herhangi bir ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="e6c09-111">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> in front of any middleware where you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="e6c09-112">Geliştirici özel durumu sayfası etkinleştirme **yalnızca geliştirme ortamında uygulama çalışırken**.</span><span class="sxs-lookup"><span data-stu-id="e6c09-112">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="e6c09-113">Üretim ortamında uygulama çalıştığında ayrıntılı özel durum bilgileri herkese açık şekilde paylaşma istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="e6c09-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="e6c09-114">Ortamları yapılandırma hakkında daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="e6c09-114">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="e6c09-115">Geliştirici özel durumu sayfasını görmek için ortamı ayarlamak örnek uygulamayı çalıştırma `Development` ve ekleme `?throw=true` uygulamasının temel URL'si.</span><span class="sxs-lookup"><span data-stu-id="e6c09-115">To see the Developer Exception Page, run the sample app with the environment set to `Development` and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="e6c09-116">Sayfasında, özel durum ve isteği hakkında aşağıdaki bilgileri içerir:</span><span class="sxs-lookup"><span data-stu-id="e6c09-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="e6c09-117">Yığın izlemesi</span><span class="sxs-lookup"><span data-stu-id="e6c09-117">Stack trace</span></span>
* <span data-ttu-id="e6c09-118">Dize parametreleri (varsa) sorgulama</span><span class="sxs-lookup"><span data-stu-id="e6c09-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="e6c09-119">Tanımlama bilgileri (varsa)</span><span class="sxs-lookup"><span data-stu-id="e6c09-119">Cookies (if any)</span></span>
* <span data-ttu-id="e6c09-120">Üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="e6c09-120">Headers</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="e6c09-121">Sayfa işleme özel bir özel durum yapılandırın</span><span class="sxs-lookup"><span data-stu-id="e6c09-121">Configure a custom exception handling page</span></span>

<span data-ttu-id="e6c09-122">Uygulama geliştirme ortamında çalışmadığı aramanızı <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> özel durum işleme ara yazılım eklemek için genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e6c09-122">When the app isn't running in the Development environment, call the <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> extension method to add Exception Handling Middleware.</span></span> <span data-ttu-id="e6c09-123">Ara yazılım:</span><span class="sxs-lookup"><span data-stu-id="e6c09-123">The middleware:</span></span>

* <span data-ttu-id="e6c09-124">Özel durumu yakalar.</span><span class="sxs-lookup"><span data-stu-id="e6c09-124">Catches exceptions.</span></span>
* <span data-ttu-id="e6c09-125">Özel durumları günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="e6c09-125">Logs exceptions.</span></span>
* <span data-ttu-id="e6c09-126">Belirtilen denetleyici ve sayfa için alternatif bir işlem hattı istekte yeniden yürütür.</span><span class="sxs-lookup"><span data-stu-id="e6c09-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="e6c09-127">İstek, yanıt başladıysa, yeniden çalıştırılır değildir.</span><span class="sxs-lookup"><span data-stu-id="e6c09-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="e6c09-128">Aşağıdaki örnekte, örnek uygulamadan <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> geliştirme olmayan ortamlarda özel durum işleme ara yazılım ekler.</span><span class="sxs-lookup"><span data-stu-id="e6c09-128">In the following example from the sample app, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments.</span></span> <span data-ttu-id="e6c09-129">Bir hata sayfası veya denetleyicisinde genişletme yöntemi belirler `/Error` özel durum yakalandı ve günlüğe sonra yeniden yürütülen istekler için uç nokta:</span><span class="sxs-lookup"><span data-stu-id="e6c09-129">The extension method specifies an error page or controller at the `/Error` endpoint for re-executed requests after exceptions are caught and logged:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler1)]

<span data-ttu-id="e6c09-130">Bir hata sayfası Razor sayfaları uygulaması şablonunu sunar (*.cshtml*) ve <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> sınıfı (`ErrorModel`) sayfalar klasöründe.</span><span class="sxs-lookup"><span data-stu-id="e6c09-130">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the Pages folder.</span></span>

<span data-ttu-id="e6c09-131">Aşağıdaki hata işleyicisi yöntemi bir MVC uygulamasında MVC uygulama şablonuna dahil edilir ve giriş denetleyicisine içinde görünür:</span><span class="sxs-lookup"><span data-stu-id="e6c09-131">In an MVC app, the following error handler method is included in the MVC app template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="e6c09-132">HTTP yöntemi öznitelikleriyle hata işleyicisi eylem yöntemi gibi süslemek yoksa `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="e6c09-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="e6c09-133">Açık fiilleri yöntemi bazı istekleri engellenir.</span><span class="sxs-lookup"><span data-stu-id="e6c09-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="e6c09-134">Kimliği doğrulanmamış kullanıcılar hata görünümünün alabilirsiniz, böylece yöntemi anonim erişime izin verin.</span><span class="sxs-lookup"><span data-stu-id="e6c09-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

## <a name="access-the-exception"></a><span data-ttu-id="e6c09-135">Erişim özel durumu</span><span class="sxs-lookup"><span data-stu-id="e6c09-135">Access the exception</span></span>

<span data-ttu-id="e6c09-136">Kullanım <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> özel durumu veya özgün istek yolu bir denetleyici veya sayfasına erişmek için:</span><span class="sxs-lookup"><span data-stu-id="e6c09-136">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to access the exception or the original request path in a controller or page:</span></span>

* <span data-ttu-id="e6c09-137">Kullanılabilir yoldur <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path> özelliği.</span><span class="sxs-lookup"><span data-stu-id="e6c09-137">The path is available from the <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path> property.</span></span>
* <span data-ttu-id="e6c09-138">Okuma <xref:System.Exception?displayProperty=fullName> öğesinden devralınan [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error) özelliği.</span><span class="sxs-lookup"><span data-stu-id="e6c09-138">Read the <xref:System.Exception?displayProperty=fullName> from the inherited [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error) property.</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics;

var exceptionHandlerPathFeature = 
    HttpContext.Features.Get<IExceptionHandlerPathFeature>();
var path = exceptionHandlerPathFeature?.Path;
var error = exceptionHandlerPathFeature?.Error;
```

> [!WARNING]
> <span data-ttu-id="e6c09-139">Yapmak **değil** önemli hata bilgileri hizmet <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> veya <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> istemcilere.</span><span class="sxs-lookup"><span data-stu-id="e6c09-139">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="e6c09-140">Hataları hizmet veren bir güvenlik riski oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e6c09-140">Serving errors is a security risk.</span></span>

## <a name="configure-custom-exception-handling-code"></a><span data-ttu-id="e6c09-141">Özel durum işleme kodunu yapılandırın</span><span class="sxs-lookup"><span data-stu-id="e6c09-141">Configure custom exception handling code</span></span>

<span data-ttu-id="e6c09-142">Hizmet veren bir uç nokta ile hatalar için alternatif bir [özel durum işleme sayfası](#configure-a-custom-exception-handling-page) lambda sağlamaktır <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span><span class="sxs-lookup"><span data-stu-id="e6c09-142">An alternative to serving an endpoint for errors with a [custom exception handling page](#configure-a-custom-exception-handling-page) is to provide a lambda to <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span></span> <span data-ttu-id="e6c09-143">Bir lambda ile kullanarak <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> yanıt döndürmeden önce hata erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="e6c09-143">Using a lambda with <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> allows access to the error before returning the response.</span></span>

<span data-ttu-id="e6c09-144">Özel durum işleme kodunu, örnek uygulamayı gösterir `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="e6c09-144">The sample app demonstrates custom exception handling code in `Startup.Configure`.</span></span> <span data-ttu-id="e6c09-145">İle bir özel durum harekete **özel durum Throw** bağlantısı dizin sayfası.</span><span class="sxs-lookup"><span data-stu-id="e6c09-145">Trigger an exception with the **Throw Exception** link on the Index page.</span></span> <span data-ttu-id="e6c09-146">Aşağıdaki lambda çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="e6c09-146">The following lambda runs:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler2)]

> [!WARNING]
> <span data-ttu-id="e6c09-147">Yapmak **değil** önemli hata bilgileri hizmet <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> veya <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> istemcilere.</span><span class="sxs-lookup"><span data-stu-id="e6c09-147">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="e6c09-148">Hataları hizmet veren bir güvenlik riski oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e6c09-148">Serving errors is a security risk.</span></span>

## <a name="configure-status-code-pages"></a><span data-ttu-id="e6c09-149">Durum kod sayfaları yapılandırın</span><span class="sxs-lookup"><span data-stu-id="e6c09-149">Configure status code pages</span></span>

<span data-ttu-id="e6c09-150">Varsayılan olarak, ASP.NET Core uygulaması durumu kod sayfası için HTTP durum kodları, gibi sağlamaz *404 - Bulunamadı*.</span><span class="sxs-lookup"><span data-stu-id="e6c09-150">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="e6c09-151">Uygulama, durum kodu ve boş yanıt gövdesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="e6c09-151">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="e6c09-152">Kod sayfaları durumu sağlamak için durum kodu sayfa ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="e6c09-152">To provide status code pages, use Status Code Pages Middleware.</span></span>

<span data-ttu-id="e6c09-153">Ara yazılım tarafından kullanılabilir hale getirileceğini [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) kullanılabilir paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e6c09-153">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="e6c09-154">Bir satırı `Startup.Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e6c09-154">Add a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="e6c09-155">Çağrı <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> önce ara (örneğin, statik dosya ara yazılımlarını ve MVC ara yazılım) işleme istek yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e6c09-155">Call the <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> method before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="e6c09-156">Gibi yaygın durum kodları için salt metin işleyiciler varsayılan olarak, durum kodu sayfa ara yazılımı ekler *404 - Bulunamadı*:</span><span class="sxs-lookup"><span data-stu-id="e6c09-156">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as *404 - Not Found*:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="e6c09-157">Ara yazılım davranışını özelleştirmenizi birkaç genişletme yöntemleri destekler.</span><span class="sxs-lookup"><span data-stu-id="e6c09-157">The middleware supports several extension methods that allow you to customize its behavior.</span></span>

<span data-ttu-id="e6c09-158">Bir aşırı yüklemesini <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> özel hata işleme mantığı işlemek ve el ile yanıt yazmak için kullanabileceğiniz bir lambda ifadesini alır:</span><span class="sxs-lookup"><span data-stu-id="e6c09-158">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a lambda expression, which you can use to process custom error-handling logic and manually write the response:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="e6c09-159">Bir aşırı yüklemesini <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> içerik türü ve yanıt metni özelleştirmek için kullanabileceğiniz bir içerik türü ve biçim dizesini alır:</span><span class="sxs-lookup"><span data-stu-id="e6c09-159">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a content type and format string, which you can use to customize the content type and response text:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a><span data-ttu-id="e6c09-160">Yeniden yönlendirme uzantısı yöntemleri ve yeniden yürütme</span><span class="sxs-lookup"><span data-stu-id="e6c09-160">Redirect and re-execute extension methods</span></span>

<span data-ttu-id="e6c09-161"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span><span class="sxs-lookup"><span data-stu-id="e6c09-161"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span></span>

* <span data-ttu-id="e6c09-162">Gönderen bir *302 bulundu -* istemciye durum kodu.</span><span class="sxs-lookup"><span data-stu-id="e6c09-162">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="e6c09-163">İstemci URL'si şablonda verilen konuma yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="e6c09-163">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="e6c09-164"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> yaygın olarak kullanıldığında uygulama:</span><span class="sxs-lookup"><span data-stu-id="e6c09-164"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> is commonly used when the app:</span></span>

* <span data-ttu-id="e6c09-165">Genellikle, farklı bir uygulama hatası burada işler durumlarda farklı bir uç noktası için istemci yönlendirmelidir.</span><span class="sxs-lookup"><span data-stu-id="e6c09-165">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="e6c09-166">Web apps için yeniden yönlendirilen uç nokta istemcinin tarayıcınızın adres çubuğuna yansıtır.</span><span class="sxs-lookup"><span data-stu-id="e6c09-166">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="e6c09-167">Olmamalıdır korumak ve özgün ilk yönlendirme yanıt durum koduyla döndürür.</span><span class="sxs-lookup"><span data-stu-id="e6c09-167">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

<span data-ttu-id="e6c09-168"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span><span class="sxs-lookup"><span data-stu-id="e6c09-168"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span></span>

* <span data-ttu-id="e6c09-169">Özgün durum kodunu istemciye döndürür.</span><span class="sxs-lookup"><span data-stu-id="e6c09-169">Returns the original status code to the client.</span></span>
* <span data-ttu-id="e6c09-170">Yanıt gövdesi, alternatif bir yol kullanarak istek ardışık düzenini yeniden yürüterek oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e6c09-170">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<span data-ttu-id="e6c09-171"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> Uygulama sırasında yaygın olarak kullanılır:</span><span class="sxs-lookup"><span data-stu-id="e6c09-171"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> is commonly used when the app should:</span></span>

* <span data-ttu-id="e6c09-172">İstek farklı bir uç noktasına yönlendirme olmadan işlem.</span><span class="sxs-lookup"><span data-stu-id="e6c09-172">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="e6c09-173">Web apps için başlangıçta istenen uç noktası istemcinin tarayıcınızın adres çubuğuna yansıtır.</span><span class="sxs-lookup"><span data-stu-id="e6c09-173">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="e6c09-174">Korumak ve özgün durum koduyla bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="e6c09-174">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="e6c09-175">Şablonları, bir yer tutucu içerebilir (`{0}`) durum kodu.</span><span class="sxs-lookup"><span data-stu-id="e6c09-175">Templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="e6c09-176">Şablonu bir eğik çizgi ile başlamalıdır (`/`).</span><span class="sxs-lookup"><span data-stu-id="e6c09-176">The template must start with a forward slash (`/`).</span></span> <span data-ttu-id="e6c09-177">Bir yer tutucu kullanırken, uç noktayı (sayfa veya denetleyicisi) yol kesimi işleyebilir onaylayın.</span><span class="sxs-lookup"><span data-stu-id="e6c09-177">When using a placeholder, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="e6c09-178">Örneğin, hatalar için bir Razor sayfası ile isteğe bağlı yol kesimi değerini kabul etmelidir `@page` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="e6c09-178">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="e6c09-179">Durum kod sayfaları için Razor sayfaları işleyicisi yöntemi veya MVC denetleyicisi belirli isteklere devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="e6c09-179">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="e6c09-180">Durum kod sayfaları devre dışı bırakmak için alma denemesi <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> gelen isteğin [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) toplama ve varsa bu özelliği devre dışı bırak:</span><span class="sxs-lookup"><span data-stu-id="e6c09-180">To disable status code pages, attempt to retrieve the <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> from the request's [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="e6c09-181">Kullanılacak bir <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> noktaları bir uç noktaya uygulama içinde oluşturduğunuz bir MVC görünümü ya da bir Razor sayfası uç nokta için aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="e6c09-181">To use a <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="e6c09-182">Razor sayfaları uygulama şablonu, örneğin, aşağıdaki sayfasını ve sayfa modeli sınıfı üretir:</span><span class="sxs-lookup"><span data-stu-id="e6c09-182">For example, the Razor Pages app template produces the following page and page model class:</span></span>

<span data-ttu-id="e6c09-183">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e6c09-183">*Error.cshtml*:</span></span>

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

<span data-ttu-id="e6c09-184">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6c09-184">*Error.cshtml.cs*:</span></span>

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

<span data-ttu-id="e6c09-185">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6c09-185">*Error.cshtml.cs*:</span></span>

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

## <a name="exception-handling-code"></a><span data-ttu-id="e6c09-186">Özel durum işleme kodu</span><span class="sxs-lookup"><span data-stu-id="e6c09-186">Exception-handling code</span></span>

<span data-ttu-id="e6c09-187">Özel durum işleme sayfaları kodda özel durumlar.</span><span class="sxs-lookup"><span data-stu-id="e6c09-187">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="e6c09-188">Genellikle, tamamen statik içeriği oluşmalıdır üretim hata sayfaları için iyi bir fikir olduğunu.</span><span class="sxs-lookup"><span data-stu-id="e6c09-188">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="e6c09-189">Ayrıca, dikkat edin, yanıt üst bilgileri gönderdikten sonra:</span><span class="sxs-lookup"><span data-stu-id="e6c09-189">Also, be aware that once the headers for a response are sent:</span></span>

* <span data-ttu-id="e6c09-190">Uygulama, yanıtın durum kodu değiştiremezsiniz.</span><span class="sxs-lookup"><span data-stu-id="e6c09-190">The app can't change the response's status code.</span></span>
* <span data-ttu-id="e6c09-191">Herhangi bir özel durum sayfaları veya işleyicileri çalıştıramazsınız.</span><span class="sxs-lookup"><span data-stu-id="e6c09-191">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="e6c09-192">Yanıt tamamlanması gereken veya bağlantı kesildi.</span><span class="sxs-lookup"><span data-stu-id="e6c09-192">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="e6c09-193">Sunucu özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="e6c09-193">Server exception handling</span></span>

<span data-ttu-id="e6c09-194">Özel durum işleme mantığı, uygulamanıza ek olarak [sunucusu uygulaması](xref:fundamentals/servers/index) bazı özel durumları işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="e6c09-194">In addition to the exception handling logic in your app, the [server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="e6c09-195">Yanıt üst bilgileri gönderilmeden önce sunucunun bir özel durumu yakalar, sunucunun gönderdiği bir *500 - İç sunucu hatası* yanıt gövdesi olmadan yanıt.</span><span class="sxs-lookup"><span data-stu-id="e6c09-195">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="e6c09-196">Yanıt üstbilgileri gönderildikten sonra sunucu bir özel durumu yakalar, sunucu bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="e6c09-196">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="e6c09-197">Uygulamanız tarafından işlenmeyen isteği sunucu tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="e6c09-197">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="e6c09-198">Sunucu isteği işlerken oluşan özel sunucu özel durum tarafından işlenir işleme.</span><span class="sxs-lookup"><span data-stu-id="e6c09-198">Any exception that occurs when the server is handling the request is handled by the server's exception handling.</span></span> <span data-ttu-id="e6c09-199">Uygulamanın özel hata sayfaları, özel durum işleme ara yazılım ve filtreler bu davranışı etkilemez.</span><span class="sxs-lookup"><span data-stu-id="e6c09-199">The app's custom error pages, exception handling middleware, and filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="e6c09-200">Başlangıç özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="e6c09-200">Startup exception handling</span></span>

<span data-ttu-id="e6c09-201">Yalnızca barındırma katmanı, uygulama başlatma sırasında gerçekleşmesi özel durumları işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="e6c09-201">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="e6c09-202">Kullanarak [Web ana bilgisayarı](xref:fundamentals/host/web-host), yapabilecekleriniz [nasıl konak hatalara yanıt başlatma sırasında davranacağını yapılandırmak](xref:fundamentals/host/web-host#detailed-errors) ile `captureStartupErrors` ve `detailedErrors` anahtarları.</span><span class="sxs-lookup"><span data-stu-id="e6c09-202">Using [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="e6c09-203">Ana bilgisayar adresi/bağlantı noktası sonra bağlama bir hata oluşursa bir hata sayfası için yakalanan başlatma hatası barındırma yalnızca gösterebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e6c09-203">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="e6c09-204">Bağlama için herhangi bir nedenle başarısız olursa:</span><span class="sxs-lookup"><span data-stu-id="e6c09-204">If any binding fails for any reason:</span></span>

* <span data-ttu-id="e6c09-205">Barındırma katman kritik bir özel durumu günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="e6c09-205">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="e6c09-206">Dotnet işlem kilitleniyor.</span><span class="sxs-lookup"><span data-stu-id="e6c09-206">The dotnet process crashes.</span></span>
* <span data-ttu-id="e6c09-207">Uygulama çalışırken, hiçbir hata sayfası görüntülenir [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="e6c09-207">No error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="e6c09-208">Çalışırken [IIS](/iis) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 - işlem hatası* tarafından döndürülen [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) işlemi başlatılamazsa .</span><span class="sxs-lookup"><span data-stu-id="e6c09-208">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="e6c09-209">Daha fazla bilgi için bkz. <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="e6c09-209">For more information, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="e6c09-210">Azure App Service ile başlatma sorunlarını giderme hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="e6c09-210">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="e6c09-211">ASP.NET Core MVC hata işleme</span><span class="sxs-lookup"><span data-stu-id="e6c09-211">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="e6c09-212">[MVC](xref:mvc/overview) uygulamalara sahip özel durum filtreleri yapılandırma ve model doğrulama gerçekleştirme gibi hataları işlemek için bazı ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="e6c09-212">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="e6c09-213">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="e6c09-213">Exception filters</span></span>

<span data-ttu-id="e6c09-214">Özel durum filtreleri, genel olarak veya bir MVC uygulamasında her denetleyici veya eylem başına temelinde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="e6c09-214">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="e6c09-215">Bu filtreler bir denetleyici eylemi veya başka bir filtre yürütülmesi sırasında oluşan tüm işlenmeyen bir özel durumu işle.</span><span class="sxs-lookup"><span data-stu-id="e6c09-215">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="e6c09-216">Bu filtreler, aksi takdirde çağrılır değil.</span><span class="sxs-lookup"><span data-stu-id="e6c09-216">These filters aren't called otherwise.</span></span> <span data-ttu-id="e6c09-217">Daha fazla bilgi için bkz. <xref:mvc/controllers/filters#exception-filters>.</span><span class="sxs-lookup"><span data-stu-id="e6c09-217">For more information, see <xref:mvc/controllers/filters#exception-filters>.</span></span>

> [!TIP]
> <span data-ttu-id="e6c09-218">Özel durum filtreleri, MVC Eylemler içinde oluşan özel durumları yakalama için yararlıdır, ancak bunlar özel durum işleme ara yazılımı kadar esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="e6c09-218">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as the Exception Handling Middleware.</span></span> <span data-ttu-id="e6c09-219">Ara yazılım kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="e6c09-219">We recommend using the middleware.</span></span> <span data-ttu-id="e6c09-220">Hata işleme gerçekleştirmek için yalnızca gerek duyduğunuz filtrelerini kullanma *farklı* göre MVC eylemi seçilir.</span><span class="sxs-lookup"><span data-stu-id="e6c09-220">Use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handle-model-state-errors"></a><span data-ttu-id="e6c09-221">Model durumu hataları işleme</span><span class="sxs-lookup"><span data-stu-id="e6c09-221">Handle model state errors</span></span>

<span data-ttu-id="e6c09-222">[Model doğrulama](xref:mvc/models/validation) her denetleyici eylemi çağırma öncesinde gerçekleşir ve incelemek için eylem yönteminin sorumluluğu olan [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) ve uygun şekilde tepki verin.</span><span class="sxs-lookup"><span data-stu-id="e6c09-222">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) and react appropriately.</span></span>

<span data-ttu-id="e6c09-223">Başa çıkmak için standart bir kural izlemek bazı uygulamalar seçin [model doğrulama](xref:mvc/models/validation) durumda hataları bir [filtre](xref:mvc/controllers/filters) böyle bir ilke uygulamak için uygun bir yere olabilir.</span><span class="sxs-lookup"><span data-stu-id="e6c09-223">Some apps choose to follow a standard convention for dealing with [model validation](xref:mvc/models/validation) errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="e6c09-224">Eylemlerinizi ile geçersiz model durumlarının nasıl davranacağını test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e6c09-224">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="e6c09-225">Daha fazla bilgi için bkz. <xref:mvc/controllers/testing>.</span><span class="sxs-lookup"><span data-stu-id="e6c09-225">For more information, see <xref:mvc/controllers/testing>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e6c09-226">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e6c09-226">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
