---
title: ASP.NET Core hataları işleme
author: rick-anderson
description: ASP.NET Core uygulamalarında hataların nasıl işleneceğini öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/08/2019
uid: fundamentals/error-handling
ms.openlocfilehash: a610c42d75864259b609e11b8bf0776c5ab8e507
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2019
ms.locfileid: "72288855"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="747f3-103">ASP.NET Core hataları işleme</span><span class="sxs-lookup"><span data-stu-id="747f3-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="747f3-104">[Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex)ve [Steve Smith](https://ardalis.com/) tarafından</span><span class="sxs-lookup"><span data-stu-id="747f3-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="747f3-105">Bu makalede ASP.NET Core uygulamalardaki hataları işlemeye yönelik yaygın yaklaşımlar ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="747f3-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="747f3-106">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="747f3-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span> <span data-ttu-id="747f3-107">([Nasıl indirilir](xref:index#how-to-download-a-sample).) Makale, farklı senaryoları etkinleştirmek için örnek uygulamada Önişlemci yönergelerinin (`#if`, `#endif`, `#define`) nasıl ayarlanacağı hakkında yönergeler içerir.</span><span class="sxs-lookup"><span data-stu-id="747f3-107">([How to download](xref:index#how-to-download-a-sample).) The article includes instructions about how to set preprocessor directives (`#if`, `#endif`, `#define`) in the sample app to enable different scenarios.</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="747f3-108">Geliştirici özel durum sayfası</span><span class="sxs-lookup"><span data-stu-id="747f3-108">Developer Exception Page</span></span>

<span data-ttu-id="747f3-109">*Geliştirici özel durum sayfası* , istek özel durumları hakkında ayrıntılı bilgileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="747f3-109">The *Developer Exception Page* displays detailed information about request exceptions.</span></span> <span data-ttu-id="747f3-110">Sayfa, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içindeki [Microsoft. Aspnetcore. Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) paketi tarafından kullanılabilir hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="747f3-110">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="747f3-111">Uygulama geliştirme [ortamında](xref:fundamentals/environments)çalışırken sayfayı etkinleştirmek için `Startup.Configure` yöntemine kod ekleyin:</span><span class="sxs-lookup"><span data-stu-id="747f3-111">Add code to the `Startup.Configure` method to enable the page when the app is running in the Development [environment](xref:fundamentals/environments):</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=1-4)]

<span data-ttu-id="747f3-112">Özel durumları yakalamak istediğiniz herhangi bir ara yazılımın önüne <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> ' a çağrı koyun.</span><span class="sxs-lookup"><span data-stu-id="747f3-112">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> before any middleware that you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="747f3-113">Geliştirici özel durum sayfasını **yalnızca uygulama geliştirme ortamında çalışırken**etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="747f3-113">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="747f3-114">Uygulama üretimde çalıştırıldığında ayrıntılı özel durum bilgilerini herkese açık bir şekilde paylaşmak istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="747f3-114">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="747f3-115">Ortamları yapılandırma hakkında daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="747f3-115">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="747f3-116">Sayfa, özel durum ve istek hakkında şu bilgileri içerir:</span><span class="sxs-lookup"><span data-stu-id="747f3-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="747f3-117">Yığın izleme</span><span class="sxs-lookup"><span data-stu-id="747f3-117">Stack trace</span></span>
* <span data-ttu-id="747f3-118">Sorgu dizesi parametreleri (varsa)</span><span class="sxs-lookup"><span data-stu-id="747f3-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="747f3-119">Tanımlama bilgileri (varsa)</span><span class="sxs-lookup"><span data-stu-id="747f3-119">Cookies (if any)</span></span>
* <span data-ttu-id="747f3-120">Üst bilgiler</span><span class="sxs-lookup"><span data-stu-id="747f3-120">Headers</span></span>

<span data-ttu-id="747f3-121">[Örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples)geliştirici özel durum sayfasını görmek için `DevEnvironment` ön işlemci yönergesini kullanın ve giriş sayfasında **özel durum Tetikle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="747f3-121">To see the Developer Exception Page in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `DevEnvironment` preprocessor directive and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-page"></a><span data-ttu-id="747f3-122">Özel durum işleyici sayfası</span><span class="sxs-lookup"><span data-stu-id="747f3-122">Exception handler page</span></span>

<span data-ttu-id="747f3-123">Üretim ortamı için özel bir hata işleme sayfası yapılandırmak için özel durum Işleme ara yazılımını kullanın.</span><span class="sxs-lookup"><span data-stu-id="747f3-123">To configure a custom error handling page for the Production environment, use the Exception Handling Middleware.</span></span> <span data-ttu-id="747f3-124">Ara yazılım:</span><span class="sxs-lookup"><span data-stu-id="747f3-124">The middleware:</span></span>

* <span data-ttu-id="747f3-125">Özel durumları yakalar ve günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="747f3-125">Catches and logs exceptions.</span></span>
* <span data-ttu-id="747f3-126">İsteği, belirtilen sayfa veya denetleyici için alternatif bir ardışık düzende yeniden yürütür.</span><span class="sxs-lookup"><span data-stu-id="747f3-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="747f3-127">Yanıt başlatılmışsa istek yeniden yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="747f3-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="747f3-128">Aşağıdaki örnekte, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>, geliştirme dışı ortamlarda özel durum Işleme ara yazılımını ekler:</span><span class="sxs-lookup"><span data-stu-id="747f3-128">In the following example, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=5-9)]

<span data-ttu-id="747f3-129">Razor Pages App Template, *Sayfalar* klasöründe bir hata sayfası ( *. cshtml*) ve <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> sınıfı (`ErrorModel`) sağlar.</span><span class="sxs-lookup"><span data-stu-id="747f3-129">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the *Pages* folder.</span></span> <span data-ttu-id="747f3-130">MVC uygulaması için, proje şablonu bir hata eylemi yöntemi ve bir hata görünümü içerir.</span><span class="sxs-lookup"><span data-stu-id="747f3-130">For an MVC app, the project template includes an Error action method and an Error view.</span></span> <span data-ttu-id="747f3-131">Eylem yöntemi aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="747f3-131">Here's the action method:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="747f3-132">@No__t-0 gibi HTTP Yöntem öznitelikleriyle hata işleyicisi eylem yöntemini süslememe.</span><span class="sxs-lookup"><span data-stu-id="747f3-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="747f3-133">Açık fiiller bazı isteklerin yönteme ulaşmasını önler.</span><span class="sxs-lookup"><span data-stu-id="747f3-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="747f3-134">Kimliği doğrulanmamış kullanıcıların hata görünümünü alabilmesi için metoda anonim erişime izin verin.</span><span class="sxs-lookup"><span data-stu-id="747f3-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

### <a name="access-the-exception"></a><span data-ttu-id="747f3-135">Özel duruma erişin</span><span class="sxs-lookup"><span data-stu-id="747f3-135">Access the exception</span></span>

<span data-ttu-id="747f3-136">Bir hata işleyicisi denetleyicisi veya sayfasındaki özel duruma ve özgün istek yoluna erişmek için <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> kullanın:</span><span class="sxs-lookup"><span data-stu-id="747f3-136">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to access the exception and the original request path in an error handler controller or page:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/Error.cshtml.cs?name=snippet_ExceptionHandlerPathFeature&3,7)]

> [!WARNING]
> <span data-ttu-id="747f3-137">İstemcilere hassas hata bilgileri sunma.</span><span class="sxs-lookup"><span data-stu-id="747f3-137">Do **not** serve sensitive error information to clients.</span></span> <span data-ttu-id="747f3-138">Hatalara hizmet vermek bir güvenlik riskidir.</span><span class="sxs-lookup"><span data-stu-id="747f3-138">Serving errors is a security risk.</span></span>

<span data-ttu-id="747f3-139">[Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples)özel durum işleme sayfasını görmek için `ProdEnvironment` ve `ErrorHandlerPage` ön işlemci yönergelerini kullanın ve giriş sayfasında **bir özel durum Tetikle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="747f3-139">To see the exception handling page in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerPage` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-lambda"></a><span data-ttu-id="747f3-140">Özel durum işleyici lambda</span><span class="sxs-lookup"><span data-stu-id="747f3-140">Exception handler lambda</span></span>

<span data-ttu-id="747f3-141">Özel bir özel [durum işleyici sayfasına](#exception-handler-page) bir alternatif, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ' e bir lambda sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="747f3-141">An alternative to a [custom exception handler page](#exception-handler-page) is to provide a lambda to <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span></span> <span data-ttu-id="747f3-142">Lambda kullanılması, yanıtı döndürmeden önce hataya erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="747f3-142">Using a lambda allows access to the error before returning the response.</span></span>

<span data-ttu-id="747f3-143">Özel durum işleme için lambda kullanmanın bir örneği aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="747f3-143">Here's an example of using a lambda for exception handling:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_HandlerPageLambda)]

> [!WARNING]
> <span data-ttu-id="747f3-144">@No__t -1 veya <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> ' den istemcilere hassas hata bilgileri sunma.</span><span class="sxs-lookup"><span data-stu-id="747f3-144">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="747f3-145">Hatalara hizmet vermek bir güvenlik riskidir.</span><span class="sxs-lookup"><span data-stu-id="747f3-145">Serving errors is a security risk.</span></span>

<span data-ttu-id="747f3-146">[Örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples)lambda işlemenin özel durum işleme sonucunu görmek için `ProdEnvironment` ve `ErrorHandlerLambda` ön işlemci yönergelerini kullanın ve giriş sayfasında **bir özel durum Tetikle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="747f3-146">To see the result of the exception handling lambda in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerLambda` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="usestatuscodepages"></a><span data-ttu-id="747f3-147">UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="747f3-147">UseStatusCodePages</span></span>

<span data-ttu-id="747f3-148">Varsayılan olarak, bir ASP.NET Core uygulama HTTP durum kodları için *404-bulunamadı*gibi bir durum kodu sayfası sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="747f3-148">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="747f3-149">Uygulama bir durum kodu ve boş bir yanıt gövdesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="747f3-149">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="747f3-150">Durum kodu sayfaları sağlamak için durum kodu sayfaları ara yazılımını kullanın.</span><span class="sxs-lookup"><span data-stu-id="747f3-150">To provide status code pages, use Status Code Pages middleware.</span></span>

<span data-ttu-id="747f3-151">Ara yazılım, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içindeki [Microsoft. Aspnetcore. Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) paketi tarafından kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="747f3-151">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="747f3-152">Ortak hata durum kodları için varsayılan salt metin işleyicilerini etkinleştirmek üzere `Startup.Configure` yönteminde <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> ' ı çağırın:</span><span class="sxs-lookup"><span data-stu-id="747f3-152">To enable default text-only handlers for common error status codes, call <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> in the `Startup.Configure` method:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="747f3-153">İstek işleme ara yazılımı (örneğin, statik dosya ara yazılımı ve MVC ara yazılımı) öncesinde `UseStatusCodePages` ' ı çağırın.</span><span class="sxs-lookup"><span data-stu-id="747f3-153">Call `UseStatusCodePages` before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="747f3-154">Varsayılan işleyiciler tarafından görüntülenbir metin örneği aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="747f3-154">Here's an example of text displayed by the default handlers:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="747f3-155">[Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples)çeşitli durum kodu sayfası biçimlerinden birini görmek için, `StatusCodePages` ile başlayan Önişlemci yönergelerinden birini kullanın ve giriş sayfasında **404 Tetikle** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="747f3-155">To see one of the various status code page formats in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use one of the preprocessor directives that begin with `StatusCodePages`, and select **Trigger a 404** on the home page.</span></span>

## <a name="usestatuscodepages-with-format-string"></a><span data-ttu-id="747f3-156">Biçim dizesiyle UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="747f3-156">UseStatusCodePages with format string</span></span>

<span data-ttu-id="747f3-157">Yanıt içerik türünü ve metnini özelleştirmek için, içerik türü ve biçim dizesi alan <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> ' nın aşırı yüklemesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="747f3-157">To customize the response content type and text, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a content type and format string:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesFormatString)]

## <a name="usestatuscodepages-with-lambda"></a><span data-ttu-id="747f3-158">Lambda ile UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="747f3-158">UseStatusCodePages with lambda</span></span>

<span data-ttu-id="747f3-159">Özel hata işleme ve yanıt yazma kodu belirtmek için, lambda ifadesi alan <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> ' nın aşırı yüklemesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="747f3-159">To specify custom error-handling and response-writing code, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesLambda)]

## <a name="usestatuscodepageswithredirects"></a><span data-ttu-id="747f3-160">Usestatuscodepageswithyönlendirmeler</span><span class="sxs-lookup"><span data-stu-id="747f3-160">UseStatusCodePagesWithRedirects</span></span>

<span data-ttu-id="747f3-161">@No__t-0 genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="747f3-161">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> extension method:</span></span>

* <span data-ttu-id="747f3-162">İstemciye *302 tarafından bulunan* bir durum kodu gönderir.</span><span class="sxs-lookup"><span data-stu-id="747f3-162">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="747f3-163">İstemciyi, URL şablonunda belirtilen konuma yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="747f3-163">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="747f3-164">URL şablonu, örnekte gösterildiği gibi durum kodu için `{0}` yer tutucusu içerebilir.</span><span class="sxs-lookup"><span data-stu-id="747f3-164">The URL template can include a `{0}` placeholder for the status code, as shown in the example.</span></span> <span data-ttu-id="747f3-165">URL şablonu bir tilde (~) ile başlıyorsa, tilde uygulamanın `PathBase` ' dır.</span><span class="sxs-lookup"><span data-stu-id="747f3-165">If the URL template starts with a tilde (~), the tilde is replaced by the app's `PathBase`.</span></span> <span data-ttu-id="747f3-166">Uygulamanın içindeki bir uç noktayı işaret ederseniz, uç nokta için bir MVC görünümü veya Razor sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="747f3-166">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="747f3-167">Razor Pages bir örnek için bkz. [örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples) *Pages/StatusCode. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="747f3-167">For a Razor Pages example, see *Pages/StatusCode.cshtml* in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="747f3-168">Bu yöntem genellikle uygulama şu şekilde kullanılır:</span><span class="sxs-lookup"><span data-stu-id="747f3-168">This method is commonly used when the app:</span></span>

* <span data-ttu-id="747f3-169">, Genellikle farklı bir uygulamanın hatayı işlediği durumlarda istemciyi farklı bir uç noktaya yönlendirmelidir.</span><span class="sxs-lookup"><span data-stu-id="747f3-169">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="747f3-170">Web Apps için, istemcinin tarayıcı adres çubuğu yeniden yönlendirilen uç noktayı yansıtır.</span><span class="sxs-lookup"><span data-stu-id="747f3-170">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="747f3-171">İlk yeniden yönlendirme yanıtıyla birlikte özgün durum kodunu korumamalıdır ve döndürmemelidir.</span><span class="sxs-lookup"><span data-stu-id="747f3-171">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

## <a name="usestatuscodepageswithreexecute"></a><span data-ttu-id="747f3-172">UseStatusCodePagesWithReExecute</span><span class="sxs-lookup"><span data-stu-id="747f3-172">UseStatusCodePagesWithReExecute</span></span>

<span data-ttu-id="747f3-173">@No__t-0 genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="747f3-173">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> extension method:</span></span>

* <span data-ttu-id="747f3-174">İstemciye özgün durum kodunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="747f3-174">Returns the original status code to the client.</span></span>
* <span data-ttu-id="747f3-175">Alternatif bir yol kullanarak istek ardışık düzenini yeniden yürüterek yanıt gövdesini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="747f3-175">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithReExecute)]

<span data-ttu-id="747f3-176">Uygulamanın içindeki bir uç noktayı işaret ederseniz, uç nokta için bir MVC görünümü veya Razor sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="747f3-176">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="747f3-177">Razor Pages bir örnek için bkz. [örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples) *Pages/StatusCode. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="747f3-177">For a Razor Pages example, see *Pages/StatusCode.cshtml* in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="747f3-178">Bu yöntem genellikle uygulama şunları yaparken kullanılır:</span><span class="sxs-lookup"><span data-stu-id="747f3-178">This method is commonly used when the app should:</span></span>

* <span data-ttu-id="747f3-179">Farklı bir uç noktaya yönlendirmeye gerek kalmadan isteği işleyin.</span><span class="sxs-lookup"><span data-stu-id="747f3-179">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="747f3-180">Web Apps için, istemcinin tarayıcı adres çubuğu, ilk olarak istenen uç noktayı yansıtır.</span><span class="sxs-lookup"><span data-stu-id="747f3-180">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="747f3-181">Yanıt ile orijinal durum kodunu koruyun ve geri döndürün.</span><span class="sxs-lookup"><span data-stu-id="747f3-181">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="747f3-182">URL ve sorgu dizesi şablonları durum kodu için bir yer tutucu (`{0}`) içerebilir.</span><span class="sxs-lookup"><span data-stu-id="747f3-182">The URL and query string templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="747f3-183">URL şablonu eğik çizgiyle (`/`) başlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="747f3-183">The URL template must start with a slash (`/`).</span></span> <span data-ttu-id="747f3-184">Yolda bir yer tutucu kullanırken, uç noktanın (sayfa veya denetleyicinin) yol kesimini işleyediğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="747f3-184">When using a placeholder in the path, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="747f3-185">Örneğin, bir Razor sayfası, `@page` yönergesi ile isteğe bağlı yol segmenti değerini kabul etmelidir:</span><span class="sxs-lookup"><span data-stu-id="747f3-185">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="747f3-186">Hatayı işleyen uç nokta, aşağıdaki örnekte gösterildiği gibi, hatayı oluşturan özgün URL 'YI alabilir:</span><span class="sxs-lookup"><span data-stu-id="747f3-186">The endpoint that processes the error can get the original URL that generated the error, as shown in the following example:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/StatusCode.cshtml.cs?name=snippet_StatusCodeReExecute)]

## <a name="disable-status-code-pages"></a><span data-ttu-id="747f3-187">Durum kodu sayfalarını devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="747f3-187">Disable status code pages</span></span>

<span data-ttu-id="747f3-188">MVC denetleyicisi veya eylem yöntemi için durum kodu sayfalarını devre dışı bırakmak için [[Skipstatuscodepages]](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute) özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="747f3-188">To disable status code pages for an MVC controller or action method, use the [[SkipStatusCodePages]](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute) attribute.</span></span>

<span data-ttu-id="747f3-189">Razor Pages işleyici yöntemindeki veya bir MVC denetleyicisindeki belirli istekler için durum kodu sayfalarını devre dışı bırakmak için <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> kullanın:</span><span class="sxs-lookup"><span data-stu-id="747f3-189">To disable status code pages for specific requests in a Razor Pages handler method or in an MVC controller, use <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="747f3-190">Özel durum işleme kodu</span><span class="sxs-lookup"><span data-stu-id="747f3-190">Exception-handling code</span></span>

<span data-ttu-id="747f3-191">Özel durum işleme sayfalarındaki kod özel durumlar oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="747f3-191">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="747f3-192">Üretim hatası sayfalarının tamamen statik içerikten oluşması genellikle iyi bir fikirdir.</span><span class="sxs-lookup"><span data-stu-id="747f3-192">It's often a good idea for production error pages to consist of purely static content.</span></span>

### <a name="response-headers"></a><span data-ttu-id="747f3-193">Yanıt üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="747f3-193">Response headers</span></span>

<span data-ttu-id="747f3-194">Yanıt üstbilgileri gönderildikten sonra:</span><span class="sxs-lookup"><span data-stu-id="747f3-194">Once the headers for a response are sent:</span></span>

* <span data-ttu-id="747f3-195">Uygulama, yanıtın durum kodunu değiştiremiyor.</span><span class="sxs-lookup"><span data-stu-id="747f3-195">The app can't change the response's status code.</span></span>
* <span data-ttu-id="747f3-196">Tüm özel durum sayfaları veya işleyicileri çalıştırılamaz.</span><span class="sxs-lookup"><span data-stu-id="747f3-196">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="747f3-197">Yanıt tamamlanmalıdır veya bağlantı iptal edilmiş olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="747f3-197">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="747f3-198">Sunucu özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="747f3-198">Server exception handling</span></span>

<span data-ttu-id="747f3-199">Uygulamanızın özel durum işleme mantığına ek olarak, [http sunucusu uygulaması](xref:fundamentals/servers/index) bazı özel durumları işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="747f3-199">In addition to the exception handling logic in your app, the [HTTP server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="747f3-200">Sunucu yanıt üstbilgileri gönderilmeden önce bir özel durum yakalarsa, sunucu yanıt gövdesi olmadan *500-Iç sunucu hatası* yanıtı gönderir.</span><span class="sxs-lookup"><span data-stu-id="747f3-200">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="747f3-201">Yanıt üstbilgileri gönderildikten sonra sunucu bir özel durum yakalar, sunucu bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="747f3-201">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="747f3-202">Uygulamanız tarafından işlenmeyen istekler sunucu tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="747f3-202">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="747f3-203">Sunucu isteği gerçekleştirirken oluşan tüm özel durumlar sunucunun özel durum işleme tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="747f3-203">Any exception that occurs when the server is handling the request is handled by the server's exception handling.</span></span> <span data-ttu-id="747f3-204">Uygulamanın özel hata sayfaları, özel durum işleme ara yazılımı ve filtreler bu davranışı etkilemez.</span><span class="sxs-lookup"><span data-stu-id="747f3-204">The app's custom error pages, exception handling middleware, and filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="747f3-205">Başlatma özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="747f3-205">Startup exception handling</span></span>

<span data-ttu-id="747f3-206">Uygulama başlangıcında gerçekleşen özel durumları yalnızca barındırma katmanı işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="747f3-206">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="747f3-207">Konak, [Başlangıç hatalarını yakalamak](xref:fundamentals/host/web-host#capture-startup-errors) ve [ayrıntılı hataları yakalamak](xref:fundamentals/host/web-host#detailed-errors)için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="747f3-207">The host can be configured to [capture startup errors](xref:fundamentals/host/web-host#capture-startup-errors) and [capture detailed errors](xref:fundamentals/host/web-host#detailed-errors).</span></span>

<span data-ttu-id="747f3-208">Barındırma katmanı, yalnızca hatanın ana bilgisayar adresi/bağlantı noktası bağlamalarından sonra oluşması durumunda yakalanan başlatma hatası için bir hata sayfası gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="747f3-208">The hosting layer can show an error page for a captured startup error only if the error occurs after host address/port binding.</span></span> <span data-ttu-id="747f3-209">Bağlama başarısız olursa:</span><span class="sxs-lookup"><span data-stu-id="747f3-209">If binding fails:</span></span>

* <span data-ttu-id="747f3-210">Barındırma katmanı, kritik bir özel durumu günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="747f3-210">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="747f3-211">DotNet işlemi kilitleniyor.</span><span class="sxs-lookup"><span data-stu-id="747f3-211">The dotnet process crashes.</span></span>
* <span data-ttu-id="747f3-212">HTTP sunucusu [Kestrel](xref:fundamentals/servers/kestrel)olduğunda hata sayfası gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="747f3-212">No error page is displayed when the HTTP server is [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="747f3-213">[IIS](/iis) 'de (veya Azure App Service) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)çalışırken, işlem başlatılamazsa [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) tarafından *502,5 işlem hatası* döndürülür.</span><span class="sxs-lookup"><span data-stu-id="747f3-213">When running on [IIS](/iis) (or Azure App Service) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="747f3-214">Daha fazla bilgi için bkz. <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="747f3-214">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

## <a name="database-error-page"></a><span data-ttu-id="747f3-215">Veritabanı hata sayfası</span><span class="sxs-lookup"><span data-stu-id="747f3-215">Database error page</span></span>

<span data-ttu-id="747f3-216">Veritabanı hata sayfası ara yazılımı, Entity Framework geçişleri kullanılarak çözümleneyolabilecek veritabanıyla ilgili özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="747f3-216">Database Error Page Middleware captures database-related exceptions that can be resolved by using Entity Framework migrations.</span></span> <span data-ttu-id="747f3-217">Bu özel durumlar gerçekleştiğinde, sorunu çözmeye yönelik olası eylemlerin ayrıntılarını içeren bir HTML yanıtı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="747f3-217">When these exceptions occur, an HTML response with details of possible actions to resolve the issue is generated.</span></span> <span data-ttu-id="747f3-218">Bu sayfa yalnızca geliştirme ortamında etkinleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="747f3-218">This page should be enabled only in the Development environment.</span></span> <span data-ttu-id="747f3-219">@No__t-0 ' a kod ekleyerek sayfayı etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="747f3-219">Enable the page by adding code to `Startup.Configure`:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDatabaseErrorPage();
}
```

<!-- FUTURE UPDATE: On the next topic overhaul/release update, add API crosslink to this section for xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage* when available via the API docs. -->

## <a name="exception-filters"></a><span data-ttu-id="747f3-220">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="747f3-220">Exception filters</span></span>

<span data-ttu-id="747f3-221">MVC uygulamalarında özel durum filtreleri, genel olarak veya denetleyicide veya eylem temelinde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="747f3-221">In MVC apps, exception filters can be configured globally or on a per-controller or per-action basis.</span></span> <span data-ttu-id="747f3-222">Razor Pages uygulamalarda, genel olarak veya sayfa modeli başına yapılandırılabilirler.</span><span class="sxs-lookup"><span data-stu-id="747f3-222">In Razor Pages apps, they can be configured globally or per page model.</span></span> <span data-ttu-id="747f3-223">Bu filtreler, bir denetleyici eyleminin veya başka bir filtrenin yürütülmesi sırasında oluşan işlenmemiş özel durumları işler.</span><span class="sxs-lookup"><span data-stu-id="747f3-223">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="747f3-224">Daha fazla bilgi için bkz. <xref:mvc/controllers/filters#exception-filters>.</span><span class="sxs-lookup"><span data-stu-id="747f3-224">For more information, see <xref:mvc/controllers/filters#exception-filters>.</span></span>

> [!TIP]
> <span data-ttu-id="747f3-225">Özel durum filtreleri, MVC eylemleri içinde oluşan özel durumları yakalamak için yararlıdır, ancak özel durum Işleme ara yazılımı kadar esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="747f3-225">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as the Exception Handling Middleware.</span></span> <span data-ttu-id="747f3-226">Ara yazılımı kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="747f3-226">We recommend using the middleware.</span></span> <span data-ttu-id="747f3-227">Yalnızca hangi MVC eyleminin seçilmesinden ayrı olarak hata işleme yapmanız gereken durumlarda filtreleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="747f3-227">Use filters only where you need to perform error handling differently based on which MVC action is chosen.</span></span>

## <a name="model-state-errors"></a><span data-ttu-id="747f3-228">Model durumu hataları</span><span class="sxs-lookup"><span data-stu-id="747f3-228">Model state errors</span></span>

<span data-ttu-id="747f3-229">Model durumu hatalarını işleme hakkında daha fazla bilgi için bkz. [model bağlama](xref:mvc/models/model-binding) ve [model doğrulaması](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="747f3-229">For information about how to handle model state errors, see [Model binding](xref:mvc/models/model-binding) and [Model validation](xref:mvc/models/validation).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="747f3-230">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="747f3-230">Additional resources</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
