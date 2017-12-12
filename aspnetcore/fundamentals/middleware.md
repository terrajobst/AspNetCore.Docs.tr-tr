---
title: ASP.NET Core Ara
author: rick-anderson
description: "ASP.NET Core ara yazılımı ve istek ardışık düzenini hakkında bilgi edinin."
keywords: "ASP.NET Core, ara yazılım, ardışık düzen, temsilci"
ms.author: riande
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: db9a86ab-46c2-40e0-baed-86e38c16af1f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: ad8d207b1e6de396f16d098fb07ddc89bea2c520
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="a2725-104">ASP.NET Core ara yazılım temelleri</span><span class="sxs-lookup"><span data-stu-id="a2725-104">ASP.NET Core Middleware Fundamentals</span></span>

<a name="fundamentals-middleware"></a>

<span data-ttu-id="a2725-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a2725-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a2725-106">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a2725-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="a2725-107">Ara yazılım nedir</span><span class="sxs-lookup"><span data-stu-id="a2725-107">What is middleware</span></span>

<span data-ttu-id="a2725-108">Ara yazılım istekleri ve yanıtları işlemek için bir uygulama ardışık düzenine birleştirilmiş bir yazılımdır.</span><span class="sxs-lookup"><span data-stu-id="a2725-108">Middleware is software that is assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="a2725-109">Her bileşen:</span><span class="sxs-lookup"><span data-stu-id="a2725-109">Each component:</span></span>

* <span data-ttu-id="a2725-110">İstek ardışık düzende sonraki bileşene geçmek seçer.</span><span class="sxs-lookup"><span data-stu-id="a2725-110">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="a2725-111">İş önce ve ardışık düzende sonraki bileşene çağrıldıktan sonra gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2725-111">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="a2725-112">İstek temsilciler istek ardışık düzenini oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2725-112">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="a2725-113">İstek temsilcileri her HTTP isteği işler.</span><span class="sxs-lookup"><span data-stu-id="a2725-113">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="a2725-114">Temsilcileri kullanarak yapılandırılmış olan istek [çalıştırmak](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [harita](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), ve [kullanım](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) genişletme yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="a2725-114">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="a2725-115">Yeniden kullanılabilir bir sınıfta tanımlanabilir veya ayrı istek temsilci (satır içi ara yazılımı olarak adlandırılır) bir anonim yöntemi olarak belirtilen satır içi olabilir.</span><span class="sxs-lookup"><span data-stu-id="a2725-115">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="a2725-116">Bu yeniden kullanılabilir sınıfları ve satır içi anonim yöntemler *ara yazılımı*, veya *ara yazılımı bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="a2725-116">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="a2725-117">Her ara yazılım bileşeni istek kanalında, ardışık düzende sonraki bileşene çağırma veya zincir uygunsa kısa devre sorumludur.</span><span class="sxs-lookup"><span data-stu-id="a2725-117">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="a2725-118">[Ara yazılım için geçirme HTTP modülleri](../migration/http-modules.md) daha fazla ara yazılımı örnekleri sağlar ve ASP.NET Core içindeki istek ardışık düzen ve önceki sürümler arasındaki farkı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a2725-118">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="a2725-119">Bir ara yazılım ardışık düzenini IApplicationBuilder ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="a2725-119">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="a2725-120">ASP.NET Core istek ardışık düzen isteği temsilciler (iş parçacığı yürütme aşağıdaki siyah ok) Bu diyagramda gösterildiği gibi birbiri ardından, olarak adlandırılan, bir dizi oluşur:</span><span class="sxs-lookup"><span data-stu-id="a2725-120">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![İstek işleme düzeni ulaşan, üç middlewares ve uygulama bırakarak yanıt aracılığıyla işleme isteği gösteriliyor.](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="a2725-124">Her temsilci, önce ve sonra İleri temsilci işlemleri yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2725-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="a2725-125">Ayrıca, bir temsilci bir istek istek ardışık düzenini kısa devre adlı bir sonraki temsilci değil geçmesine karar verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2725-125">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="a2725-126">Gereksiz iş önler çünkü kısa devre genellikle iyi bir şeydir.</span><span class="sxs-lookup"><span data-stu-id="a2725-126">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="a2725-127">Örneğin, statik dosya ara yazılımlarını statik bir dosya için bir istek dönün ve kalan ardışık düzenini kısa devre oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2725-127">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="a2725-128">Özel durum işleme temsilciler kanalının sonraki aşamalarında oluşan özel durumlarını yakalayabilirsiniz ardışık düzeninde çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a2725-128">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="a2725-129">En basit olası ASP.NET Core uygulama tüm istekleri işleyen tek istek temsilci ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a2725-129">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="a2725-130">Bu durumda, gerçek istek ardışık düzenini içermez.</span><span class="sxs-lookup"><span data-stu-id="a2725-130">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="a2725-131">Bunun yerine, tek bir anonim işlevi her HTTP isteğine yanıt olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="a2725-131">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

<span data-ttu-id="a2725-132">İlk [uygulama. Çalıştırma](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) temsilci ardışık sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="a2725-132">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="a2725-133">İle birlikte birden çok istek temsilcileri zincirleme [uygulama. Kullanım](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="a2725-133">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="a2725-134">`next` Parametresi ardışık düzende sonraki temsilci temsil eder.</span><span class="sxs-lookup"><span data-stu-id="a2725-134">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="a2725-135">(Ardışık düzen tarafından kısa devre oluşturur olduğunu unutmayın *değil* çağırma *sonraki* parametresi.) Bu örnekte gösterilmiştir gibi öncesinde ve sonrasında sonraki temsilci genellikle eylemleri gerçekleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a2725-135">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="a2725-136">Çağırmayın `next.Invoke` yanıtı istemciye gönderildikten sonra.</span><span class="sxs-lookup"><span data-stu-id="a2725-136">Do not call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="a2725-137">Değişikliklerini `HttpResponse` yanıt başlatıldıktan sonra bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2725-137">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="a2725-138">Örneğin, üst bilgileri, durum kodu, vb., ayarlama gibi değişiklikler, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2725-138">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="a2725-139">Yanıt gövdesi çağrıldıktan sonra Yazma `next`:</span><span class="sxs-lookup"><span data-stu-id="a2725-139">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="a2725-140">Bir protokolü ihlali neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="a2725-140">May cause a protocol violation.</span></span> <span data-ttu-id="a2725-141">Örneğin, birden çok belirtilen yazma `content-length`.</span><span class="sxs-lookup"><span data-stu-id="a2725-141">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="a2725-142">Gövde biçimi bozulmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="a2725-142">May corrupt the body format.</span></span> <span data-ttu-id="a2725-143">Örneğin, bir HTML altbilgi CSS dosyaya yazma.</span><span class="sxs-lookup"><span data-stu-id="a2725-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="a2725-144">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) üstbilgileri gönderilen ve/veya gövdesi yazılmış varsa göstermek için yararlı bir ipucu olur.</span><span class="sxs-lookup"><span data-stu-id="a2725-144">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="a2725-145">Sıralama</span><span class="sxs-lookup"><span data-stu-id="a2725-145">Ordering</span></span>

<span data-ttu-id="a2725-146">Ara yazılım bileşenlerinin içinde eklendiğinden sipariş `Configure` , bunlar çağrılır isteklerinde sırası ve yanıtı için ters sırada yöntemi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a2725-146">The order that middleware components are added in the `Configure` method defines the order in which they are invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="a2725-147">Bu sıralama, güvenlik, performans ve işlevselliği için önemlidir.</span><span class="sxs-lookup"><span data-stu-id="a2725-147">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="a2725-148">(Aşağıda gösterilen) yapılandırma yöntemi aşağıdaki ara yazılımı bileşenleri ekler:</span><span class="sxs-lookup"><span data-stu-id="a2725-148">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="a2725-149">Özel durum/hata işleme</span><span class="sxs-lookup"><span data-stu-id="a2725-149">Exception/error handling</span></span>
2. <span data-ttu-id="a2725-150">Statik dosya sunucusu</span><span class="sxs-lookup"><span data-stu-id="a2725-150">Static file server</span></span>
3. <span data-ttu-id="a2725-151">Kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="a2725-151">Authentication</span></span>
4. <span data-ttu-id="a2725-152">MVC</span><span class="sxs-lookup"><span data-stu-id="a2725-152">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a2725-153">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="a2725-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a2725-154">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="a2725-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

<span data-ttu-id="a2725-155">Yukarıdaki kod `UseExceptionHandler` ardışık düzenine eklenen ilk ara yazılım bileşeni; bu nedenle, daha sonra çağrılarında oluşan özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="a2725-155">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="a2725-156">Böylece istekleri işlemek ve kalan bileşenleri geçmeden kısa devre oluşturur statik dosya ara yazılımlarını erken ardışık düzen adı verilir.</span><span class="sxs-lookup"><span data-stu-id="a2725-156">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="a2725-157">Statik dosya ara yazılımlarını sağlar **hiçbir** yetkilendirme denetimleri.</span><span class="sxs-lookup"><span data-stu-id="a2725-157">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="a2725-158">Herhangi bir dosya sunulan işlem tarafından altında dahil olmak üzere *wwwroot*, genel olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a2725-158">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="a2725-159">Bkz: [statik dosyaları ile çalışma](xref:fundamentals/static-files) statik dosyaları güvenli bir yaklaşım için.</span><span class="sxs-lookup"><span data-stu-id="a2725-159">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a2725-160">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="a2725-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="a2725-161">İstek statik dosya ara yazılımı tarafından işlenmemiş varsa, bu kimlik Ara geçirildiğinde (`app.UseAuthentication`), kimlik doğrulaması gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="a2725-161">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="a2725-162">Kimliği doğrulanmamış istekler kısa devre oluşturur değil.</span><span class="sxs-lookup"><span data-stu-id="a2725-162">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="a2725-163">İstek kimliğini doğrular rağmen yalnızca bir özel Razor sayfasını veya denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) oluşur.</span><span class="sxs-lookup"><span data-stu-id="a2725-163">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a2725-164">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="a2725-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a2725-165">İstek statik dosya ara yazılımı tarafından işlenmemiş varsa, bu kimlik Ara geçirildiğinde (`app.UseIdentity`), kimlik doğrulaması gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="a2725-165">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="a2725-166">Kimliği doğrulanmamış istekler kısa devre oluşturur değil.</span><span class="sxs-lookup"><span data-stu-id="a2725-166">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="a2725-167">İstek kimliğini doğrular rağmen yalnızca belirli denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) oluşur.</span><span class="sxs-lookup"><span data-stu-id="a2725-167">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="a2725-168">Aşağıdaki örnek, burada statik dosyalar için istek yanıt sıkıştırma Ara önce statik dosya ara yazılımı tarafından işlenen sıralama bir ara yazılımı gösterir.</span><span class="sxs-lookup"><span data-stu-id="a2725-168">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="a2725-169">Statik dosyalar bu ara yazılım sıralama ile sıkıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="a2725-169">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="a2725-170">MVC yanıtlarının [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) sıkıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a2725-170">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="a2725-171">Kullanmak için çalıştırmak ve eşleme</span><span class="sxs-lookup"><span data-stu-id="a2725-171">Use, Run, and Map</span></span>

<span data-ttu-id="a2725-172">HTTP kullanarak ardışık düzen yapılandırma `Use`, `Run`, ve `Map`.</span><span class="sxs-lookup"><span data-stu-id="a2725-172">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="a2725-173">`Use` Yöntemi kısa devre oluşturur ardışık düzen (diğer bir deyişle, arama, bir `next` isteği temsilci).</span><span class="sxs-lookup"><span data-stu-id="a2725-173">The `Use` method can short-circuit the pipeline (that is, if it does not call a `next` request delegate).</span></span> <span data-ttu-id="a2725-174">`Run`bir kural ve bazı ara yazılımı bileşenleri getirebilir `Run[Middleware]` ardışık düzen sonunda çalışacak yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="a2725-174">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="a2725-175">`Map*`Uzantılar, ardışık düzen dallanma için bir kural kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2725-175">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="a2725-176">[Harita](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) istek ardışık düzenini belirtilen istek yolu eşleşmeleri üzerinde göre dallandırır.</span><span class="sxs-lookup"><span data-stu-id="a2725-176">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="a2725-177">İstek yolu belirtilen yolun ile başlarsa, şube yürütülür.</span><span class="sxs-lookup"><span data-stu-id="a2725-177">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="a2725-178">Aşağıdaki tabloda isteklerinin ve yanıtlarının gösterilmektedir `http://localhost:1234` önceki kod kullanarak:</span><span class="sxs-lookup"><span data-stu-id="a2725-178">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="a2725-179">İstek</span><span class="sxs-lookup"><span data-stu-id="a2725-179">Request</span></span> | <span data-ttu-id="a2725-180">Yanıt</span><span class="sxs-lookup"><span data-stu-id="a2725-180">Response</span></span> |
| --- | --- |
| <span data-ttu-id="a2725-181">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="a2725-181">localhost:1234</span></span> | <span data-ttu-id="a2725-182">Merhaba harita olmayan temsilci gelen.</span><span class="sxs-lookup"><span data-stu-id="a2725-182">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="a2725-183">localhost:1234 / map1</span><span class="sxs-lookup"><span data-stu-id="a2725-183">localhost:1234/map1</span></span> | <span data-ttu-id="a2725-184">Harita Test 1</span><span class="sxs-lookup"><span data-stu-id="a2725-184">Map Test 1</span></span> |
| <span data-ttu-id="a2725-185">localhost:1234 / map2</span><span class="sxs-lookup"><span data-stu-id="a2725-185">localhost:1234/map2</span></span> | <span data-ttu-id="a2725-186">Harita Test 2</span><span class="sxs-lookup"><span data-stu-id="a2725-186">Map Test 2</span></span> |
| <span data-ttu-id="a2725-187">localhost:1234 / map3</span><span class="sxs-lookup"><span data-stu-id="a2725-187">localhost:1234/map3</span></span> | <span data-ttu-id="a2725-188">Merhaba harita olmayan temsilci gelen.</span><span class="sxs-lookup"><span data-stu-id="a2725-188">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="a2725-189">Zaman `Map` olan kullanıldığında, eşleşen yolu segment(s) çıkarılır `HttpRequest.Path` ve için eklenen `HttpRequest.PathBase` her istek için.</span><span class="sxs-lookup"><span data-stu-id="a2725-189">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="a2725-190">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) istek ardışık düzenini belirtilen koşulun sonucuna göre dallandırır.</span><span class="sxs-lookup"><span data-stu-id="a2725-190">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="a2725-191">Herhangi bir koşul türü `Func<HttpContext, bool>` istekleri dalı ardışık eşlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2725-191">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="a2725-192">Aşağıdaki örnekte, bir koşul bir sorgu dizesi değişkeni varolup olmadığını algılamak için kullanılan `branch`:</span><span class="sxs-lookup"><span data-stu-id="a2725-192">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="a2725-193">Aşağıdaki tabloda isteklerinin ve yanıtlarının gösterilmektedir `http://localhost:1234` önceki kod kullanarak:</span><span class="sxs-lookup"><span data-stu-id="a2725-193">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="a2725-194">İstek</span><span class="sxs-lookup"><span data-stu-id="a2725-194">Request</span></span> | <span data-ttu-id="a2725-195">Yanıt</span><span class="sxs-lookup"><span data-stu-id="a2725-195">Response</span></span> |
| --- | --- |
| <span data-ttu-id="a2725-196">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="a2725-196">localhost:1234</span></span> | <span data-ttu-id="a2725-197">Merhaba harita olmayan temsilci gelen.</span><span class="sxs-lookup"><span data-stu-id="a2725-197">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="a2725-198">localhost:1234 /? şube Yöneticisi =</span><span class="sxs-lookup"><span data-stu-id="a2725-198">localhost:1234/?branch=master</span></span> | <span data-ttu-id="a2725-199">Kullanılan şube Yöneticisi =</span><span class="sxs-lookup"><span data-stu-id="a2725-199">Branch used = master</span></span>|

<span data-ttu-id="a2725-200">`Map`iç içe, örneğin destekler:</span><span class="sxs-lookup"><span data-stu-id="a2725-200">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="a2725-201">`Map`Ayrıca birden çok parçalı bir kerede örneğin eşleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a2725-201">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="a2725-202">Yerleşik Ara</span><span class="sxs-lookup"><span data-stu-id="a2725-202">Built-in middleware</span></span>

<span data-ttu-id="a2725-203">ASP.NET Core aşağıdaki ara yazılımı bileşenleri ile birlikte gelir:</span><span class="sxs-lookup"><span data-stu-id="a2725-203">ASP.NET Core ships with the following middleware components:</span></span>

| <span data-ttu-id="a2725-204">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="a2725-204">Middleware</span></span> | <span data-ttu-id="a2725-205">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a2725-205">Description</span></span> |
| ----- | ------- |
| [<span data-ttu-id="a2725-206">Kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="a2725-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="a2725-207">Kimlik doğrulama desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2725-207">Provides authentication support.</span></span> |
| [<span data-ttu-id="a2725-208">CORS</span><span class="sxs-lookup"><span data-stu-id="a2725-208">CORS</span></span>](xref:security/cors) | <span data-ttu-id="a2725-209">Çıkış noktaları arası kaynak paylaşımını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a2725-209">Configures Cross-Origin Resource Sharing.</span></span> |
| [<span data-ttu-id="a2725-210">Yanıt önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="a2725-210">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="a2725-211">Yanıt önbelleğe alma işlemi için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2725-211">Provides support for caching responses.</span></span> |
| [<span data-ttu-id="a2725-212">Yanıt sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="a2725-212">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="a2725-213">Yanıtları sıkıştırma için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2725-213">Provides support for compressing responses.</span></span> |
| [<span data-ttu-id="a2725-214">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="a2725-214">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="a2725-215">Tanımlar ve istek yolları kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="a2725-215">Defines and constrains request routes.</span></span> |
| [<span data-ttu-id="a2725-216">Oturumu</span><span class="sxs-lookup"><span data-stu-id="a2725-216">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="a2725-217">Kullanıcı oturumlarını yönetmek için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2725-217">Provides support for managing user sessions.</span></span> |
| [<span data-ttu-id="a2725-218">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="a2725-218">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="a2725-219">Statik dosya ve Dizin tarama hizmet vermek için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2725-219">Provides support for serving static files and directory browsing.</span></span> |
| [<span data-ttu-id="a2725-220">URL yeniden yazma işlemi Ara</span><span class="sxs-lookup"><span data-stu-id="a2725-220">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="a2725-221">URL yeniden yazma işlemi ve istekleri yönlendirme için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2725-221">Provides support for rewriting URLs and redirecting requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="a2725-222">Yazma Ara</span><span class="sxs-lookup"><span data-stu-id="a2725-222">Writing middleware</span></span>

<span data-ttu-id="a2725-223">Ara yazılım genellikle bir sınıfta kapsüllenmiş ve bir genişletme yöntemi ile gösteriliyor.</span><span class="sxs-lookup"><span data-stu-id="a2725-223">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="a2725-224">Kültür geçerli istek için Sorgu dizesinden ayarlar aşağıdaki Ara göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="a2725-224">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="a2725-225">Not: Yukarıdaki örnek kod, bir ara yazılım bileşeni oluşturma göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2725-225">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="a2725-226">Bkz: [ Genelleştirme ve Yerelleştirme](xref:fundamentals/localization) ASP.NET Core'nın yerleşik yerelleştirme desteği.</span><span class="sxs-lookup"><span data-stu-id="a2725-226">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="a2725-227">Kültürün, örneğin geçirerek ara yazılım sınayabilirsiniz `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="a2725-227">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="a2725-228">Aşağıdaki kod bir sınıfa ara yazılım temsilci taşır:</span><span class="sxs-lookup"><span data-stu-id="a2725-228">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="a2725-229">Ara yazılım aracılığıyla aşağıdaki uzantısı yöntemi gösterir [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="a2725-229">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="a2725-230">Aşağıdaki kod Ara çağırır `Configure`:</span><span class="sxs-lookup"><span data-stu-id="a2725-230">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="a2725-231">Ara yazılım izlemelidir [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/) bağımlılıklarını kendi oluşturucusuna gösterme tarafından.</span><span class="sxs-lookup"><span data-stu-id="a2725-231">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="a2725-232">Ara yazılım yapılandırılmıştır kez başına *uygulama ömrü*.</span><span class="sxs-lookup"><span data-stu-id="a2725-232">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="a2725-233">Bkz: *istek başına bağımlılıkları* üstündeyse ara yazılım istek içinde Hizmetleri paylaşmasına gerekir.</span><span class="sxs-lookup"><span data-stu-id="a2725-233">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="a2725-234">Ara yazılımı bileşenleri bağımlılıklarını bağımlılık ekleme Oluşturucu parametreleri üzerinden gelen çözebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2725-234">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="a2725-235">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)Ayrıca ek parametreler doğrudan kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="a2725-235">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="a2725-236">İstek başına bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="a2725-236">Per-request dependencies</span></span>

<span data-ttu-id="a2725-237">Ara yazılım değil istek başına, uygulama başlatma sırasında oluşturulur çünkü *kapsamlı* ara yazılım Oluşturucu tarafından kullanılan yaşam süresi olmayan paylaşılan hizmetler diğer bağımlılık tarafından eklenen türleriyle her isteği sırasında.</span><span class="sxs-lookup"><span data-stu-id="a2725-237">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="a2725-238">Gereken paylaşıyorsanız bir *kapsamlı* , Ara ve diğer türleri arasında hizmet, bu hizmetlere ekleme `Invoke` yöntemin imzası.</span><span class="sxs-lookup"><span data-stu-id="a2725-238">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="a2725-239">`Invoke` Yöntemi tarafından bağımlılık ekleme doldurulur ek parametreleri kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="a2725-239">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="a2725-240">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a2725-240">For example:</span></span>

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a><span data-ttu-id="a2725-241">Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a2725-241">Resources</span></span>

* [<span data-ttu-id="a2725-242">Bu belge kullanılan örnek kod</span><span class="sxs-lookup"><span data-stu-id="a2725-242">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="a2725-243">Ara yazılım için geçirme HTTP modülleri</span><span class="sxs-lookup"><span data-stu-id="a2725-243">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="a2725-244">Uygulama başlatma</span><span class="sxs-lookup"><span data-stu-id="a2725-244">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="a2725-245">İstek özellikleri</span><span class="sxs-lookup"><span data-stu-id="a2725-245">Request Features</span></span>](request-features.md)
