---
title: ASP.NET core'da hatalarını işleme
author: tdykstra
description: ASP.NET Core uygulamaları hataları işlemek nasıl keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/03/2019
uid: fundamentals/error-handling
ms.openlocfilehash: 6b92cb6b68b1c70d67f42284d548729e598f9a8b
ms.sourcegitcommit: c5339594101d30b189f61761275b7d310e80d18a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2019
ms.locfileid: "66458438"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="a7902-103">ASP.NET core'da hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="a7902-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="a7902-104">Tarafından [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a7902-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a7902-105">Bu makalede, ASP.NET Core uygulamalarında hata işleme için ortak bir yaklaşım ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a7902-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="a7902-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="a7902-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span> <span data-ttu-id="a7902-107">([Nasıl indirileceğini](xref:index#how-to-download-a-sample).) Makale önişlemci yönergeleri ayarlama hakkında yönergeler içerir (`#if`, `#endif`, `#define`) farklı senaryoları etkinleştirmek için örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="a7902-107">([How to download](xref:index#how-to-download-a-sample).) The article includes instructions about how to set preprocessor directives (`#if`, `#endif`, `#define`) in the sample app to enable different scenarios.</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="a7902-108">Geliştirici özel durumu sayfası</span><span class="sxs-lookup"><span data-stu-id="a7902-108">Developer Exception Page</span></span>

<span data-ttu-id="a7902-109">*Geliştirici özel durum sayfasında* istek özel durumları hakkında ayrıntılı bilgiler görüntüler.</span><span class="sxs-lookup"><span data-stu-id="a7902-109">The *Developer Exception Page* displays detailed information about request exceptions.</span></span> <span data-ttu-id="a7902-110">Sayfa tarafından kullanılabilir hale getirileceğini [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a7902-110">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="a7902-111">Kodu `Startup.Configure` uygulama geliştirmesinde çalışırken sayfa etkinleştirme yöntemi [ortam](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="a7902-111">Add code to the `Startup.Configure` method to enable the page when the app is running in the Development [environment](xref:fundamentals/environments):</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=1-4)]

<span data-ttu-id="a7902-112">Çağrı yapmak <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> önce özel durumları yakalamak istediğiniz herhangi bir ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="a7902-112">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> before any middleware that you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="a7902-113">Geliştirici özel durumu sayfası etkinleştirme **yalnızca geliştirme ortamında uygulama çalışırken**.</span><span class="sxs-lookup"><span data-stu-id="a7902-113">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="a7902-114">Üretim ortamında uygulama çalıştığında ayrıntılı özel durum bilgileri herkese açık şekilde paylaşma istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7902-114">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="a7902-115">Ortamları yapılandırma hakkında daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="a7902-115">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="a7902-116">Sayfasında, özel durum ve isteği hakkında aşağıdaki bilgileri içerir:</span><span class="sxs-lookup"><span data-stu-id="a7902-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="a7902-117">Yığın izlemesi</span><span class="sxs-lookup"><span data-stu-id="a7902-117">Stack trace</span></span>
* <span data-ttu-id="a7902-118">Dize parametreleri (varsa) sorgulama</span><span class="sxs-lookup"><span data-stu-id="a7902-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="a7902-119">Tanımlama bilgileri (varsa)</span><span class="sxs-lookup"><span data-stu-id="a7902-119">Cookies (if any)</span></span>
* <span data-ttu-id="a7902-120">Üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="a7902-120">Headers</span></span>

<span data-ttu-id="a7902-121">Geliştirici özel durum sayfasında görmek için [örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), kullanın `DevEnvironment` önişlemci yönergesi ve select **bir özel durum harekete** giriş sayfasında.</span><span class="sxs-lookup"><span data-stu-id="a7902-121">To see the Developer Exception Page in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `DevEnvironment` preprocessor directive and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-page"></a><span data-ttu-id="a7902-122">Özel durum işleyicisi sayfası</span><span class="sxs-lookup"><span data-stu-id="a7902-122">Exception handler page</span></span>

<span data-ttu-id="a7902-123">Bir özel hata sayfası üretim ortamı için işleme yapılandırmak için özel durum işleme ara yazılım kullanın.</span><span class="sxs-lookup"><span data-stu-id="a7902-123">To configure a custom error handling page for the Production environment, use the Exception Handling Middleware.</span></span> <span data-ttu-id="a7902-124">Ara yazılım:</span><span class="sxs-lookup"><span data-stu-id="a7902-124">The middleware:</span></span>

* <span data-ttu-id="a7902-125">Yakalar ve özel durumları günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="a7902-125">Catches and logs exceptions.</span></span>
* <span data-ttu-id="a7902-126">Belirtilen denetleyici ve sayfa için alternatif bir işlem hattı istekte yeniden yürütür.</span><span class="sxs-lookup"><span data-stu-id="a7902-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="a7902-127">İstek, yanıt başladıysa, yeniden çalıştırılır değildir.</span><span class="sxs-lookup"><span data-stu-id="a7902-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="a7902-128">Aşağıdaki örnekte, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> geliştirme olmayan ortamlarda özel durum işleme ara yazılım ekler:</span><span class="sxs-lookup"><span data-stu-id="a7902-128">In the following example, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=5-9)]

<span data-ttu-id="a7902-129">Bir hata sayfası Razor sayfaları uygulaması şablonunu sunar ( *.cshtml*) ve <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> sınıfı (`ErrorModel`) içinde *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="a7902-129">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the *Pages* folder.</span></span> <span data-ttu-id="a7902-130">Bir MVC uygulaması için proje şablonu, bir hata eylem yöntemi ve bir hata görünümü içerir.</span><span class="sxs-lookup"><span data-stu-id="a7902-130">For an MVC app, the project template includes an Error action method and an Error view.</span></span> <span data-ttu-id="a7902-131">Eylem yöntemi aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a7902-131">Here's the action method:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="a7902-132">HTTP yöntemi öznitelikleriyle hata işleyicisi eylem yöntemi gibi süslemek yoksa `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="a7902-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="a7902-133">Açık fiilleri yöntemi bazı istekleri engellenir.</span><span class="sxs-lookup"><span data-stu-id="a7902-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="a7902-134">Kimliği doğrulanmamış kullanıcılar hata görünümünün alabilirsiniz, böylece yöntemi anonim erişime izin verin.</span><span class="sxs-lookup"><span data-stu-id="a7902-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

### <a name="access-the-exception"></a><span data-ttu-id="a7902-135">Erişim özel durumu</span><span class="sxs-lookup"><span data-stu-id="a7902-135">Access the exception</span></span>

<span data-ttu-id="a7902-136">Kullanım <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> özel durum ve hata işleyicisi denetleyici ya da sayfa özgün istek yolu erişmek için:</span><span class="sxs-lookup"><span data-stu-id="a7902-136">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to access the exception and the original request path in an error handler controller or page:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/Error.cshtml.cs?name=snippet_ExceptionHandlerPathFeature&3,7)]

> [!WARNING]
> <span data-ttu-id="a7902-137">Yapmak **değil** önemli hata bilgilerini istemcilere hizmet.</span><span class="sxs-lookup"><span data-stu-id="a7902-137">Do **not** serve sensitive error information to clients.</span></span> <span data-ttu-id="a7902-138">Hataları hizmet veren bir güvenlik riski oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a7902-138">Serving errors is a security risk.</span></span>

<span data-ttu-id="a7902-139">Özel durum işleme sayfasında görmek için [örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), kullanın `ProdEnvironment` ve `ErrorHandlerPage` önişlemci yönergeleri ve select **bir özel durum harekete** giriş sayfasında.</span><span class="sxs-lookup"><span data-stu-id="a7902-139">To see the exception handling page in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerPage` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-lambda"></a><span data-ttu-id="a7902-140">Özel durum işleyici lambda</span><span class="sxs-lookup"><span data-stu-id="a7902-140">Exception handler lambda</span></span>

<span data-ttu-id="a7902-141">Alternatif bir [özel durum işleyicisi sayfa](#exception-handler-page) lambda sağlamaktır <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span><span class="sxs-lookup"><span data-stu-id="a7902-141">An alternative to a [custom exception handler page](#exception-handler-page) is to provide a lambda to <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span></span> <span data-ttu-id="a7902-142">Bir lambda kullanarak yanıt döndürmeden önce hata erişim izni verir.</span><span class="sxs-lookup"><span data-stu-id="a7902-142">Using a lambda allows access to the error before returning the response.</span></span>

<span data-ttu-id="a7902-143">Özel durum işleme için bir lambda kullanma örneği aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a7902-143">Here's an example of using a lambda for exception handling:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_HandlerPageLambda)]

> [!WARNING]
> <span data-ttu-id="a7902-144">Yapmak **değil** önemli hata bilgileri hizmet <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> veya <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> istemcilere.</span><span class="sxs-lookup"><span data-stu-id="a7902-144">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="a7902-145">Hataları hizmet veren bir güvenlik riski oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a7902-145">Serving errors is a security risk.</span></span>

<span data-ttu-id="a7902-146">Özel durum işleme lambda içinde sonucunu görmek için [örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), kullanın `ProdEnvironment` ve `ErrorHandlerLambda` önişlemci yönergeleri ve select **bir özel durum harekete** giriş sayfasında.</span><span class="sxs-lookup"><span data-stu-id="a7902-146">To see the result of the exception handling lambda in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerLambda` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="usestatuscodepages"></a><span data-ttu-id="a7902-147">UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="a7902-147">UseStatusCodePages</span></span>

<span data-ttu-id="a7902-148">Varsayılan olarak, ASP.NET Core uygulaması durumu kod sayfası için HTTP durum kodları, gibi sağlamaz *404 - Bulunamadı*.</span><span class="sxs-lookup"><span data-stu-id="a7902-148">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="a7902-149">Uygulama, durum kodu ve boş yanıt gövdesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="a7902-149">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="a7902-150">Kod sayfaları durumu sağlamak için durum kod sayfası ara yazılımını kullanın.</span><span class="sxs-lookup"><span data-stu-id="a7902-150">To provide status code pages, use Status Code Pages middleware.</span></span>

<span data-ttu-id="a7902-151">Ara yazılım tarafından kullanılabilir hale getirileceğini [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a7902-151">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a7902-152">Genel hata durum kodları için varsayılan salt metin işleyicileri etkinleştirmek için çağrı <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> içinde `Startup.Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a7902-152">To enable default text-only handlers for common error status codes, call <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> in the `Startup.Configure` method:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="a7902-153">Çağrı `UseStatusCodePages` istek işleme Ara (örneğin, statik dosya ara yazılımlarını ve MVC ara yazılım) önce.</span><span class="sxs-lookup"><span data-stu-id="a7902-153">Call `UseStatusCodePages` before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="a7902-154">Varsayılan işleyici tarafından görüntülenen metnin bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a7902-154">Here's an example of text displayed by the default handlers:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="a7902-155">Çeşitli durumu kod sayfası biçimlerinden birini görmek için [örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), önişlemci yönergeleri ile başlayan birini kullanın `StatusCodePages`seçip **404 bir tetikleyici** giriş sayfasında.</span><span class="sxs-lookup"><span data-stu-id="a7902-155">To see one of the various status code page formats in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use one of the preprocessor directives that begin with `StatusCodePages`, and select **Trigger a 404** on the home page.</span></span>

## <a name="usestatuscodepages-with-format-string"></a><span data-ttu-id="a7902-156">Biçim dizesi ile UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="a7902-156">UseStatusCodePages with format string</span></span>

<span data-ttu-id="a7902-157">Yanıt içerik türü ve metin özelleştirmek için aşırı yüklemesini kullanın. <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> , bir içerik türü ve biçim dizesini alır:</span><span class="sxs-lookup"><span data-stu-id="a7902-157">To customize the response content type and text, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a content type and format string:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesFormatString)]

## <a name="usestatuscodepages-with-lambda"></a><span data-ttu-id="a7902-158">Lambda ile UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="a7902-158">UseStatusCodePages with lambda</span></span>

<span data-ttu-id="a7902-159">Özel hata işleme ve yazma yanıtını belirtmek için kod, aşırı yüklemesini kullanın <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> , bir lambda ifadesini alır:</span><span class="sxs-lookup"><span data-stu-id="a7902-159">To specify custom error-handling and response-writing code, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesLambda)]

## <a name="usestatuscodepageswithredirect"></a><span data-ttu-id="a7902-160">UseStatusCodePagesWithRedirect</span><span class="sxs-lookup"><span data-stu-id="a7902-160">UseStatusCodePagesWithRedirect</span></span>

<span data-ttu-id="a7902-161"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> Genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a7902-161">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> extension method:</span></span>

* <span data-ttu-id="a7902-162">Gönderen bir *302 bulundu -* istemciye durum kodu.</span><span class="sxs-lookup"><span data-stu-id="a7902-162">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="a7902-163">İstemci URL'si şablonda verilen konuma yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="a7902-163">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="a7902-164">URL şablonu içerebilir bir `{0}` için durum kodunu, örnekte gösterildiği gibi yer tutucu.</span><span class="sxs-lookup"><span data-stu-id="a7902-164">The URL template can include a `{0}` placeholder for the status code, as shown in the example.</span></span> <span data-ttu-id="a7902-165">URL şablonu bir tilde (~) ile başlıyorsa tilde uygulamanın tarafından değiştirilir `PathBase`.</span><span class="sxs-lookup"><span data-stu-id="a7902-165">If the URL template starts with a tilde (~), the tilde is replaced by the app's `PathBase`.</span></span> <span data-ttu-id="a7902-166">Uygulamadaki bir uç noktaya işaret ederseniz, bir MVC görünümü ya da uç noktası için Razor sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a7902-166">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="a7902-167">Razor sayfaları örnek için bkz: *Pages/StatusCode.cshtml* içinde [örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="a7902-167">For a Razor Pages example, see *Pages/StatusCode.cshtml* in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="a7902-168">Bu yaygın bir yöntemdir kullanılabilir uygulama:</span><span class="sxs-lookup"><span data-stu-id="a7902-168">This method is commonly used when the app:</span></span>

* <span data-ttu-id="a7902-169">Genellikle, farklı bir uygulama hatası burada işler durumlarda farklı bir uç noktası için istemci yönlendirmelidir.</span><span class="sxs-lookup"><span data-stu-id="a7902-169">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="a7902-170">Web apps için yeniden yönlendirilen uç nokta istemcinin tarayıcınızın adres çubuğuna yansıtır.</span><span class="sxs-lookup"><span data-stu-id="a7902-170">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="a7902-171">Olmamalıdır korumak ve özgün ilk yönlendirme yanıt durum koduyla döndürür.</span><span class="sxs-lookup"><span data-stu-id="a7902-171">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

## <a name="usestatuscodepageswithreexecute"></a><span data-ttu-id="a7902-172">UseStatusCodePagesWithReExecute</span><span class="sxs-lookup"><span data-stu-id="a7902-172">UseStatusCodePagesWithReExecute</span></span>

<span data-ttu-id="a7902-173"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> Genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a7902-173">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> extension method:</span></span>

* <span data-ttu-id="a7902-174">Özgün durum kodunu istemciye döndürür.</span><span class="sxs-lookup"><span data-stu-id="a7902-174">Returns the original status code to the client.</span></span>
* <span data-ttu-id="a7902-175">Yanıt gövdesi, alternatif bir yol kullanarak istek ardışık düzenini yeniden yürüterek oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a7902-175">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithReExecute)]

<span data-ttu-id="a7902-176">Uygulamadaki bir uç noktaya işaret ederseniz, bir MVC görünümü ya da uç noktası için Razor sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a7902-176">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="a7902-177">Razor sayfaları örnek için bkz: *Pages/StatusCode.cshtml* içinde [örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="a7902-177">For a Razor Pages example, see *Pages/StatusCode.cshtml* in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="a7902-178">Bu yöntem, uygulamayı gerektiğinde yaygın olarak kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a7902-178">This method is commonly used when the app should:</span></span>

* <span data-ttu-id="a7902-179">İstek farklı bir uç noktasına yönlendirme olmadan işlem.</span><span class="sxs-lookup"><span data-stu-id="a7902-179">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="a7902-180">Web apps için başlangıçta istenen uç noktası istemcinin tarayıcınızın adres çubuğuna yansıtır.</span><span class="sxs-lookup"><span data-stu-id="a7902-180">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="a7902-181">Korumak ve özgün durum koduyla bir yanıt döndürür.</span><span class="sxs-lookup"><span data-stu-id="a7902-181">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="a7902-182">URL ve sorgu dize şablonları, bir yer tutucu içerebilir (`{0}`) durum kodu.</span><span class="sxs-lookup"><span data-stu-id="a7902-182">The URL and query string templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="a7902-183">URL şablonu bir eğik çizgiyle başlamalıdır (`/`).</span><span class="sxs-lookup"><span data-stu-id="a7902-183">The URL template must start with a slash (`/`).</span></span> <span data-ttu-id="a7902-184">Bir yer tutucu yolunda kullanırken, uç noktayı (sayfa veya denetleyicisi) yol kesimi işleyebilir onaylayın.</span><span class="sxs-lookup"><span data-stu-id="a7902-184">When using a placeholder in the path, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="a7902-185">Örneğin, hatalar için bir Razor sayfası ile isteğe bağlı yol kesimi değerini kabul etmelidir `@page` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="a7902-185">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="a7902-186">Hata işleme uç noktasını, aşağıdaki örnekte gösterildiği gibi hatayı oluşturan özgün URL'yi alabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a7902-186">The endpoint that processes the error can get the original URL that generated the error, as shown in the following example:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/StatusCode.cshtml.cs?name=snippet_StatusCodeReExecute)]

## <a name="disable-status-code-pages"></a><span data-ttu-id="a7902-187">Durum kod sayfaları devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="a7902-187">Disable status code pages</span></span>

<span data-ttu-id="a7902-188">MVC denetleyici veya eylem yöntemi için kod sayfaları durumu devre dışı bırakmak için [[SkipStatusCodePages]](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a7902-188">To disable status code pages for an MVC controller or action method, use the [[SkipStatusCodePages]](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute) attribute.</span></span>

<span data-ttu-id="a7902-189">Razor sayfaları işleyicisi yöntemi veya MVC denetleyicisi belirli istekler için durum kod sayfaları devre dışı bırakmak için <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span><span class="sxs-lookup"><span data-stu-id="a7902-189">To disable status code pages for specific requests in a Razor Pages handler method or in an MVC controller, use <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="a7902-190">Özel durum işleme kodu</span><span class="sxs-lookup"><span data-stu-id="a7902-190">Exception-handling code</span></span>

<span data-ttu-id="a7902-191">Özel durum işleme sayfaları kodda özel durumlar.</span><span class="sxs-lookup"><span data-stu-id="a7902-191">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="a7902-192">Genellikle, tamamen statik içeriği oluşmalıdır üretim hata sayfaları için iyi bir fikir olduğunu.</span><span class="sxs-lookup"><span data-stu-id="a7902-192">It's often a good idea for production error pages to consist of purely static content.</span></span>

### <a name="response-headers"></a><span data-ttu-id="a7902-193">Yanıt Üstbilgileri</span><span class="sxs-lookup"><span data-stu-id="a7902-193">Response headers</span></span>

<span data-ttu-id="a7902-194">Yanıt Üstbilgileri gönderdikten sonra:</span><span class="sxs-lookup"><span data-stu-id="a7902-194">Once the headers for a response are sent:</span></span>

* <span data-ttu-id="a7902-195">Uygulama, yanıtın durum kodu değiştiremezsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7902-195">The app can't change the response's status code.</span></span>
* <span data-ttu-id="a7902-196">Herhangi bir özel durum sayfaları veya işleyicileri çalıştıramazsınız.</span><span class="sxs-lookup"><span data-stu-id="a7902-196">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="a7902-197">Yanıt tamamlanması gereken veya bağlantı kesildi.</span><span class="sxs-lookup"><span data-stu-id="a7902-197">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="a7902-198">Sunucu özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="a7902-198">Server exception handling</span></span>

<span data-ttu-id="a7902-199">Özel durum işleme mantığı, uygulamanıza ek olarak [HTTP sunucusu uygulamasını](xref:fundamentals/servers/index) bazı özel durumları işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="a7902-199">In addition to the exception handling logic in your app, the [HTTP server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="a7902-200">Yanıt üst bilgileri gönderilmeden önce sunucunun bir özel durumu yakalar, sunucunun gönderdiği bir *500 - İç sunucu hatası* yanıt gövdesi olmadan yanıt.</span><span class="sxs-lookup"><span data-stu-id="a7902-200">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="a7902-201">Yanıt üstbilgileri gönderildikten sonra sunucu bir özel durumu yakalar, sunucu bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="a7902-201">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="a7902-202">Uygulamanız tarafından işlenmeyen isteği sunucu tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="a7902-202">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="a7902-203">Sunucu isteği işlerken oluşan özel sunucu özel durum tarafından işlenir işleme.</span><span class="sxs-lookup"><span data-stu-id="a7902-203">Any exception that occurs when the server is handling the request is handled by the server's exception handling.</span></span> <span data-ttu-id="a7902-204">Uygulamanın özel hata sayfaları, özel durum işleme ara yazılım ve filtreler bu davranışı etkilemez.</span><span class="sxs-lookup"><span data-stu-id="a7902-204">The app's custom error pages, exception handling middleware, and filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="a7902-205">Başlangıç özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="a7902-205">Startup exception handling</span></span>

<span data-ttu-id="a7902-206">Yalnızca barındırma katmanı, uygulama başlatma sırasında gerçekleşmesi özel durumları işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="a7902-206">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="a7902-207">Konak için yapılandırılabilir [yakalama başlatma hataları](xref:fundamentals/host/web-host#capture-startup-errors) ve [ayrıntılı hataları yakalamaya](xref:fundamentals/host/web-host#detailed-errors).</span><span class="sxs-lookup"><span data-stu-id="a7902-207">The host can be configured to [capture startup errors](xref:fundamentals/host/web-host#capture-startup-errors) and [capture detailed errors](xref:fundamentals/host/web-host#detailed-errors).</span></span>

<span data-ttu-id="a7902-208">Ana bilgisayar adresi/bağlantı noktası sonra bağlama yalnızca hata oluşursa bir hata sayfası için yakalanan başlatma hatası barındırma katman gösterebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7902-208">The hosting layer can show an error page for a captured startup error only if the error occurs after host address/port binding.</span></span> <span data-ttu-id="a7902-209">Bağlama başarısız olursa:</span><span class="sxs-lookup"><span data-stu-id="a7902-209">If binding fails:</span></span>

* <span data-ttu-id="a7902-210">Barındırma katman kritik bir özel durumu günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="a7902-210">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="a7902-211">Dotnet işlem kilitleniyor.</span><span class="sxs-lookup"><span data-stu-id="a7902-211">The dotnet process crashes.</span></span>
* <span data-ttu-id="a7902-212">HTTP sunucusu olduğunda hiçbir hata sayfası görüntülenir [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="a7902-212">No error page is displayed when the HTTP server is [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="a7902-213">Çalışırken [IIS](/iis) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 - işlem hatası* tarafından döndürülen [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) işlemi başlatılamazsa .</span><span class="sxs-lookup"><span data-stu-id="a7902-213">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="a7902-214">Daha fazla bilgi için bkz. <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="a7902-214">For more information, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="a7902-215">Azure App Service ile başlatma sorunlarını giderme hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="a7902-215">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="database-error-page"></a><span data-ttu-id="a7902-216">Veritabanı hata sayfası</span><span class="sxs-lookup"><span data-stu-id="a7902-216">Database error page</span></span>

<span data-ttu-id="a7902-217">[Veritabanı hata sayfası](<xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*>) ara yazılım, Entity Framework geçişleri kullanarak çözülebilir, veritabanı ile ilgili özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="a7902-217">The [Database Error Page](<xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*>) middleware captures database-related exceptions that can be resolved by using Entity Framework migrations.</span></span> <span data-ttu-id="a7902-218">Bu özel durumlar oluştuğunda, bu sorunu çözmek için olası Eylemler ilgili ayrıntıları içeren bir HTML yanıtını oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a7902-218">When these exceptions occur, an HTML response with details of possible actions to resolve the issue is generated.</span></span> <span data-ttu-id="a7902-219">Bu sayfa yalnızca geliştirme ortamında etkinleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7902-219">This page should be enabled only in the Development environment.</span></span> <span data-ttu-id="a7902-220">Kod ekleyerek sayfası etkinleştirme `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a7902-220">Enable the page by adding code to `Startup.Configure`:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDatabaseErrorPage();
}
```

## <a name="exception-filters"></a><span data-ttu-id="a7902-221">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="a7902-221">Exception filters</span></span>

<span data-ttu-id="a7902-222">MVC uygulamalarında özel durum filtreleri genel olarak veya her denetleyici veya eylem başına temelinde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a7902-222">In MVC apps, exception filters can be configured globally or on a per-controller or per-action basis.</span></span> <span data-ttu-id="a7902-223">Razor sayfaları uygulamalar, bunlar genel olarak veya sayfa modeli başına yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a7902-223">In Razor Pages apps, they can be configured globally or per page model.</span></span> <span data-ttu-id="a7902-224">Bu filtreler bir denetleyici eylemi veya başka bir filtre yürütülmesi sırasında oluşan tüm işlenmeyen bir özel durumu işle.</span><span class="sxs-lookup"><span data-stu-id="a7902-224">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="a7902-225">Daha fazla bilgi için bkz. <xref:mvc/controllers/filters#exception-filters>.</span><span class="sxs-lookup"><span data-stu-id="a7902-225">For more information, see <xref:mvc/controllers/filters#exception-filters>.</span></span>

> [!TIP]
> <span data-ttu-id="a7902-226">Özel durum filtreleri, MVC Eylemler içinde oluşan özel durumları yakalama için yararlıdır, ancak bunlar özel durum işleme ara yazılımı kadar esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="a7902-226">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as the Exception Handling Middleware.</span></span> <span data-ttu-id="a7902-227">Ara yazılım kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="a7902-227">We recommend using the middleware.</span></span> <span data-ttu-id="a7902-228">MVC eylemi seçilen farklı tabanlı hata işleme gerçekleştirmek için yalnızca gerek duyduğunuz filtreleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="a7902-228">Use filters only where you need to perform error handling differently based on which MVC action is chosen.</span></span>

## <a name="model-state-errors"></a><span data-ttu-id="a7902-229">Model durumu hataları</span><span class="sxs-lookup"><span data-stu-id="a7902-229">Model state errors</span></span>

<span data-ttu-id="a7902-230">Model durumu hatalarının nasıl işleneceğini hakkında daha fazla bilgi için bkz. [Model bağlama](xref:mvc/models/model-binding) ve [Model doğrulama](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="a7902-230">For information about how to handle model state errors, see [Model binding](xref:mvc/models/model-binding) and [Model validation](xref:mvc/models/validation).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a7902-231">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a7902-231">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
