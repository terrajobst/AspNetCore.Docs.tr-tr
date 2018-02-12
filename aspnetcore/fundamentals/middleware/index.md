---
title: ASP.NET Core Middleware
author: rick-anderson
description: "ASP.NET Core ara yazılımı ve istek ardışık düzenini hakkında bilgi edinin."
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 158f11875f22f8f9dba6f7f109123717b9da8d18
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="e8a70-103">ASP.NET Core Middleware</span><span class="sxs-lookup"><span data-stu-id="e8a70-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="e8a70-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e8a70-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e8a70-105">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e8a70-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="e8a70-106">Ara yazılım nedir?</span><span class="sxs-lookup"><span data-stu-id="e8a70-106">What is middleware?</span></span>

<span data-ttu-id="e8a70-107">Ara yazılım istekleri ve yanıtları işlemek için bir uygulama ardışık düzenine birleştirilmiş bir yazılımdır.</span><span class="sxs-lookup"><span data-stu-id="e8a70-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="e8a70-108">Her bileşen:</span><span class="sxs-lookup"><span data-stu-id="e8a70-108">Each component:</span></span>

* <span data-ttu-id="e8a70-109">İstek ardışık düzende sonraki bileşene geçmek seçer.</span><span class="sxs-lookup"><span data-stu-id="e8a70-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="e8a70-110">İş önce ve ardışık düzende sonraki bileşene çağrıldıktan sonra gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e8a70-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="e8a70-111">İstek temsilciler istek ardışık düzenini oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e8a70-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="e8a70-112">İstek temsilcileri her HTTP isteği işler.</span><span class="sxs-lookup"><span data-stu-id="e8a70-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="e8a70-113">Temsilcileri kullanarak yapılandırılmış olan istek [çalıştırmak](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [harita](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), ve [kullanım](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) genişletme yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="e8a70-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="e8a70-114">Yeniden kullanılabilir bir sınıfta tanımlanabilir veya ayrı istek temsilci (satır içi ara yazılımı olarak adlandırılır) bir anonim yöntemi olarak belirtilen satır içi olabilir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="e8a70-115">Bu yeniden kullanılabilir sınıfları ve satır içi anonim yöntemler *ara yazılımı*, veya *ara yazılımı bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="e8a70-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="e8a70-116">Her ara yazılım bileşeni istek kanalında, ardışık düzende sonraki bileşene çağırma veya zincir uygunsa kısa devre sorumludur.</span><span class="sxs-lookup"><span data-stu-id="e8a70-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="e8a70-117">[Ara yazılım için geçirme HTTP modülleri](xref:migration/http-modules) istek ardışık düzenlerinde ASP.NET Core ve ASP.NET arasındaki fark açıklanır 4.x ve daha fazla ara yazılımı örnekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8a70-117">[Migrating HTTP Modules to Middleware](xref:migration/http-modules) explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="e8a70-118">Bir ara yazılım ardışık düzenini IApplicationBuilder ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="e8a70-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="e8a70-119">ASP.NET Core istek ardışık düzen isteği temsilciler (iş parçacığı yürütme aşağıdaki siyah ok) Bu diyagramda gösterildiği gibi birbiri ardından, olarak adlandırılan, bir dizi oluşur:</span><span class="sxs-lookup"><span data-stu-id="e8a70-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![İstek işleme düzeni ulaşan, üç middlewares ve uygulama bırakarak yanıt aracılığıyla işleme isteği gösteriliyor.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="e8a70-123">Her temsilci, önce ve sonra İleri temsilci işlemleri yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e8a70-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="e8a70-124">Ayrıca, bir temsilci bir istek istek ardışık düzenini kısa devre adlı bir sonraki temsilci değil geçmesine karar verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e8a70-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="e8a70-125">Gereksiz iş önler çünkü kısa devre genellikle iyi bir şeydir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="e8a70-126">Örneğin, statik dosya ara yazılımlarını statik bir dosya için bir istek dönün ve kalan ardışık düzenini kısa devre oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e8a70-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="e8a70-127">Özel durum işleme temsilciler kanalının sonraki aşamalarında oluşan özel durumlarını yakalayabilirsiniz ardışık düzeninde çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="e8a70-128">En basit olası ASP.NET Core uygulama tüm istekleri işleyen tek istek temsilci ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e8a70-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="e8a70-129">Bu durumda, gerçek istek ardışık düzenini içermez.</span><span class="sxs-lookup"><span data-stu-id="e8a70-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="e8a70-130">Bunun yerine, tek bir anonim işlevi her HTTP isteğine yanıt olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="e8a70-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](index/sample/Middleware/Startup.cs)]

<span data-ttu-id="e8a70-131">İlk [uygulama. Çalıştırma](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) temsilci ardışık sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="e8a70-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="e8a70-132">İle birlikte birden çok istek temsilcileri zincirleme [uygulama. Kullanım](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="e8a70-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="e8a70-133">`next` Parametresi ardışık düzende sonraki temsilci temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e8a70-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="e8a70-134">(Ardışık düzen tarafından kısa devre oluşturur olduğunu unutmayın *değil* çağırma *sonraki* parametresi.) Bu örnekte gösterilmiştir gibi öncesinde ve sonrasında sonraki temsilci genellikle eylemleri gerçekleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e8a70-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="e8a70-135">Çağrı yok `next.Invoke` yanıtı istemciye gönderildikten sonra.</span><span class="sxs-lookup"><span data-stu-id="e8a70-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="e8a70-136">Değişikliklerini `HttpResponse` yanıt başlatıldıktan sonra bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e8a70-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="e8a70-137">Örneğin, üst bilgileri, durum kodu, vb., ayarlama gibi değişiklikler, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e8a70-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="e8a70-138">Yanıt gövdesi çağrıldıktan sonra Yazma `next`:</span><span class="sxs-lookup"><span data-stu-id="e8a70-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="e8a70-139">Bir protokolü ihlali neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-139">May cause a protocol violation.</span></span> <span data-ttu-id="e8a70-140">Örneğin, birden çok belirtilen yazma `content-length`.</span><span class="sxs-lookup"><span data-stu-id="e8a70-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="e8a70-141">Gövde biçimi bozulmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-141">May corrupt the body format.</span></span> <span data-ttu-id="e8a70-142">Örneğin, bir HTML altbilgi CSS dosyaya yazma.</span><span class="sxs-lookup"><span data-stu-id="e8a70-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="e8a70-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) üstbilgileri gönderilen ve/veya gövdesi yazılmış varsa göstermek için yararlı bir ipucu olur.</span><span class="sxs-lookup"><span data-stu-id="e8a70-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="e8a70-144">Sıralama</span><span class="sxs-lookup"><span data-stu-id="e8a70-144">Ordering</span></span>

<span data-ttu-id="e8a70-145">Ara yazılım bileşenlerinin içinde eklendiğinden sipariş `Configure` , bunlar çağrılan isteklerinde sırası ve yanıtı için ters sırada yöntemi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e8a70-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="e8a70-146">Bu sıralama, güvenlik, performans ve işlevselliği için önemlidir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="e8a70-147">(Aşağıda gösterilen) yapılandırma yöntemi aşağıdaki ara yazılımı bileşenleri ekler:</span><span class="sxs-lookup"><span data-stu-id="e8a70-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="e8a70-148">Özel durum/hata işleme</span><span class="sxs-lookup"><span data-stu-id="e8a70-148">Exception/error handling</span></span>
2. <span data-ttu-id="e8a70-149">Statik dosya sunucusu</span><span class="sxs-lookup"><span data-stu-id="e8a70-149">Static file server</span></span>
3. <span data-ttu-id="e8a70-150">Kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e8a70-150">Authentication</span></span>
4. <span data-ttu-id="e8a70-151">MVC</span><span class="sxs-lookup"><span data-stu-id="e8a70-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8a70-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8a70-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8a70-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8a70-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="e8a70-154">Yukarıdaki kod `UseExceptionHandler` ardışık düzenine eklenen ilk ara yazılım bileşeni; bu nedenle, daha sonra çağrılarında oluşan özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="e8a70-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="e8a70-155">Böylece istekleri işlemek ve kalan bileşenleri geçmeden kısa devre oluşturur statik dosya ara yazılımlarını erken ardışık düzen adı verilir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="e8a70-156">Statik dosya ara yazılımlarını sağlar **hiçbir** yetkilendirme denetimleri.</span><span class="sxs-lookup"><span data-stu-id="e8a70-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="e8a70-157">Herhangi bir dosya sunulan işlem tarafından altında dahil olmak üzere *wwwroot*, genel olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="e8a70-158">Bkz: [statik dosyaları ile çalışma](xref:fundamentals/static-files) statik dosyaları güvenli bir yaklaşım için.</span><span class="sxs-lookup"><span data-stu-id="e8a70-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e8a70-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e8a70-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="e8a70-160">İstek statik dosya ara yazılım tarafından işlenen değil, bu kimlik Ara geçirildiğinde (`app.UseAuthentication`), kimlik doğrulaması gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="e8a70-161">Kimlik, kimliği doğrulanmamış istekler kısa devre oluşturur değil.</span><span class="sxs-lookup"><span data-stu-id="e8a70-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="e8a70-162">İstek kimliğini doğrular rağmen yalnızca bir özel Razor sayfasını veya denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) oluşur.</span><span class="sxs-lookup"><span data-stu-id="e8a70-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e8a70-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e8a70-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e8a70-164">İstek statik dosya ara yazılım tarafından işlenen değil, bu kimlik Ara geçirildiğinde (`app.UseIdentity`), kimlik doğrulaması gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="e8a70-165">Kimlik, kimliği doğrulanmamış istekler kısa devre oluşturur değil.</span><span class="sxs-lookup"><span data-stu-id="e8a70-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="e8a70-166">İstek kimliğini doğrular rağmen yalnızca belirli denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) oluşur.</span><span class="sxs-lookup"><span data-stu-id="e8a70-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="e8a70-167">Aşağıdaki örnek, burada statik dosyalar için istek yanıt sıkıştırma Ara önce statik dosya ara yazılımı tarafından işlenen sıralama bir ara yazılımı gösterir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="e8a70-168">Statik dosyalar bu ara yazılım sıralama ile sıkıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="e8a70-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="e8a70-169">MVC yanıtlarının [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) sıkıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

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

### <a name="use-run-and-map"></a><span data-ttu-id="e8a70-170">Kullanmak için çalıştırmak ve eşleme</span><span class="sxs-lookup"><span data-stu-id="e8a70-170">Use, Run, and Map</span></span>

<span data-ttu-id="e8a70-171">HTTP kullanarak ardışık düzen yapılandırma `Use`, `Run`, ve `Map`.</span><span class="sxs-lookup"><span data-stu-id="e8a70-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="e8a70-172">`Use` Yöntemi kısa devre oluşturur ardışık düzen (diğer bir deyişle, çağrı değil, bir `next` isteği temsilci).</span><span class="sxs-lookup"><span data-stu-id="e8a70-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="e8a70-173">`Run`bir kural ve bazı ara yazılımı bileşenleri getirebilir `Run[Middleware]` ardışık düzen sonunda çalışacak yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="e8a70-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="e8a70-174">`Map*`Uzantılar, ardışık düzen dallanma için bir kural kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e8a70-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="e8a70-175">[Harita](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) istek ardışık düzenini belirtilen istek yolu eşleşmeleri üzerinde göre dallandırır.</span><span class="sxs-lookup"><span data-stu-id="e8a70-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="e8a70-176">İstek yolu belirtilen yolun ile başlarsa, şube yürütülür.</span><span class="sxs-lookup"><span data-stu-id="e8a70-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](index/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="e8a70-177">Aşağıdaki tabloda isteklerinin ve yanıtlarının gösterilmektedir `http://localhost:1234` önceki kod kullanarak:</span><span class="sxs-lookup"><span data-stu-id="e8a70-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="e8a70-178">İstek</span><span class="sxs-lookup"><span data-stu-id="e8a70-178">Request</span></span> | <span data-ttu-id="e8a70-179">Yanıt</span><span class="sxs-lookup"><span data-stu-id="e8a70-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="e8a70-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="e8a70-180">localhost:1234</span></span> | <span data-ttu-id="e8a70-181">Merhaba harita olmayan temsilci gelen.</span><span class="sxs-lookup"><span data-stu-id="e8a70-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="e8a70-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="e8a70-182">localhost:1234/map1</span></span> | <span data-ttu-id="e8a70-183">Harita Test 1</span><span class="sxs-lookup"><span data-stu-id="e8a70-183">Map Test 1</span></span> |
| <span data-ttu-id="e8a70-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="e8a70-184">localhost:1234/map2</span></span> | <span data-ttu-id="e8a70-185">Harita Test 2</span><span class="sxs-lookup"><span data-stu-id="e8a70-185">Map Test 2</span></span> |
| <span data-ttu-id="e8a70-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="e8a70-186">localhost:1234/map3</span></span> | <span data-ttu-id="e8a70-187">Merhaba harita olmayan temsilci gelen.</span><span class="sxs-lookup"><span data-stu-id="e8a70-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="e8a70-188">Zaman `Map` olan kullanıldığında, eşleşen yolu segment(s) çıkarılır `HttpRequest.Path` ve için eklenen `HttpRequest.PathBase` her istek için.</span><span class="sxs-lookup"><span data-stu-id="e8a70-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="e8a70-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) istek ardışık düzenini belirtilen koşulun sonucuna göre dallandırır.</span><span class="sxs-lookup"><span data-stu-id="e8a70-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="e8a70-190">Herhangi bir koşul türü `Func<HttpContext, bool>` istekleri dalı ardışık eşlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e8a70-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="e8a70-191">Aşağıdaki örnekte, bir koşul bir sorgu dizesi değişkeni varolup olmadığını algılamak için kullanılan `branch`:</span><span class="sxs-lookup"><span data-stu-id="e8a70-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="e8a70-192">Aşağıdaki tabloda isteklerinin ve yanıtlarının gösterilmektedir `http://localhost:1234` önceki kod kullanarak:</span><span class="sxs-lookup"><span data-stu-id="e8a70-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="e8a70-193">İstek</span><span class="sxs-lookup"><span data-stu-id="e8a70-193">Request</span></span> | <span data-ttu-id="e8a70-194">Yanıt</span><span class="sxs-lookup"><span data-stu-id="e8a70-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="e8a70-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="e8a70-195">localhost:1234</span></span> | <span data-ttu-id="e8a70-196">Merhaba harita olmayan temsilci gelen.</span><span class="sxs-lookup"><span data-stu-id="e8a70-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="e8a70-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="e8a70-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="e8a70-198">Kullanılan şube Yöneticisi =</span><span class="sxs-lookup"><span data-stu-id="e8a70-198">Branch used = master</span></span>|

<span data-ttu-id="e8a70-199">`Map`iç içe, örneğin destekler:</span><span class="sxs-lookup"><span data-stu-id="e8a70-199">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="e8a70-200">`Map`Ayrıca birden çok parçalı bir kerede örneğin eşleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e8a70-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="e8a70-201">Yerleşik Ara</span><span class="sxs-lookup"><span data-stu-id="e8a70-201">Built-in middleware</span></span>

<span data-ttu-id="e8a70-202">ASP.NET Core aşağıdaki ara yazılımı bileşenleri yanı sıra ile bunlar eklenmesi gereken sırayı açıklaması gelir:</span><span class="sxs-lookup"><span data-stu-id="e8a70-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="e8a70-203">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="e8a70-203">Middleware</span></span> | <span data-ttu-id="e8a70-204">Açıklama</span><span class="sxs-lookup"><span data-stu-id="e8a70-204">Description</span></span> | <span data-ttu-id="e8a70-205">Sırası</span><span class="sxs-lookup"><span data-stu-id="e8a70-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="e8a70-206">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e8a70-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="e8a70-207">Kimlik doğrulama desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8a70-207">Provides authentication support.</span></span> | <span data-ttu-id="e8a70-208">Önce `HttpContext.User` gereklidir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="e8a70-209">Terminal OAuth geri aramalar için.</span><span class="sxs-lookup"><span data-stu-id="e8a70-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="e8a70-210">CORS</span><span class="sxs-lookup"><span data-stu-id="e8a70-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="e8a70-211">Çıkış noktaları arası kaynak paylaşımını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="e8a70-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="e8a70-212">CORS kullanan bileşenleri önce.</span><span class="sxs-lookup"><span data-stu-id="e8a70-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="e8a70-213">Tanılama</span><span class="sxs-lookup"><span data-stu-id="e8a70-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="e8a70-214">Tanılama yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="e8a70-214">Configures diagnostics.</span></span> | <span data-ttu-id="e8a70-215">Hatalar oluşturur bileşenlerini önce.</span><span class="sxs-lookup"><span data-stu-id="e8a70-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="e8a70-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="e8a70-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="e8a70-217">Geçerli istek üzerine yönlendirilirken üstbilgileri iletir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="e8a70-218">Güncelleştirilmiş alanları tüketen bileşenleri önce (örnek: düzeni, ana bilgisayar, ClientIP, yöntem).</span><span class="sxs-lookup"><span data-stu-id="e8a70-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="e8a70-219">Yanıtları Önbelleğe Alma</span><span class="sxs-lookup"><span data-stu-id="e8a70-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="e8a70-220">Yanıt önbelleğe alma işlemi için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8a70-220">Provides support for caching responses.</span></span> | <span data-ttu-id="e8a70-221">Önbelleğe alma gerektiren bileşenler önce.</span><span class="sxs-lookup"><span data-stu-id="e8a70-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="e8a70-222">Yanıt sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="e8a70-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="e8a70-223">Yanıtları sıkıştırma için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8a70-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="e8a70-224">Sıkıştırma iste bileşenleri önce.</span><span class="sxs-lookup"><span data-stu-id="e8a70-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="e8a70-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="e8a70-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="e8a70-226">Yerelleştirme desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8a70-226">Provides localization support.</span></span> | <span data-ttu-id="e8a70-227">Yerelleştirme önce hassas bileşenleri.</span><span class="sxs-lookup"><span data-stu-id="e8a70-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="e8a70-228">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="e8a70-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="e8a70-229">Tanımlar ve istek yolları kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="e8a70-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="e8a70-230">Yollar eşleştirmek için terminal.</span><span class="sxs-lookup"><span data-stu-id="e8a70-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="e8a70-231">Oturum</span><span class="sxs-lookup"><span data-stu-id="e8a70-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="e8a70-232">Kullanıcı oturumlarını yönetmek için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8a70-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="e8a70-233">Oturum gerektiren bileşenler önce.</span><span class="sxs-lookup"><span data-stu-id="e8a70-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="e8a70-234">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="e8a70-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="e8a70-235">Statik dosya ve Dizin tarama hizmet vermek için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8a70-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="e8a70-236">Bir isteği dosyaları eşleşirse terminal.</span><span class="sxs-lookup"><span data-stu-id="e8a70-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="e8a70-237">URL yeniden yazma işlemi</span><span class="sxs-lookup"><span data-stu-id="e8a70-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="e8a70-238">URL yeniden yazma işlemi ve istekleri yönlendirme için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8a70-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="e8a70-239">URL tüketen bileşenleri önce.</span><span class="sxs-lookup"><span data-stu-id="e8a70-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="e8a70-240">WebSockets</span><span class="sxs-lookup"><span data-stu-id="e8a70-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="e8a70-241">WebSockets Protokolü sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8a70-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="e8a70-242">WebSocket isteklerini kabul etmek için gerekli bileşenleri önce.</span><span class="sxs-lookup"><span data-stu-id="e8a70-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="e8a70-243">Yazma Ara</span><span class="sxs-lookup"><span data-stu-id="e8a70-243">Writing middleware</span></span>

<span data-ttu-id="e8a70-244">Ara yazılım genellikle bir sınıfta kapsüllenmiş ve bir genişletme yöntemi ile gösteriliyor.</span><span class="sxs-lookup"><span data-stu-id="e8a70-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="e8a70-245">Kültür geçerli istek için Sorgu dizesinden ayarlar aşağıdaki Ara göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="e8a70-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](index/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="e8a70-246">Not: Yukarıdaki örnek kod, bir ara yazılım bileşeni oluşturma göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e8a70-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="e8a70-247">Bkz: [ Genelleştirme ve Yerelleştirme](xref:fundamentals/localization) ASP.NET Core'nın yerleşik yerelleştirme desteği.</span><span class="sxs-lookup"><span data-stu-id="e8a70-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="e8a70-248">Kültürün, örneğin geçirerek ara yazılım sınayabilirsiniz `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="e8a70-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="e8a70-249">Aşağıdaki kod bir sınıfa ara yazılım temsilci taşır:</span><span class="sxs-lookup"><span data-stu-id="e8a70-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](index/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="e8a70-250">Ara yazılım aracılığıyla aşağıdaki uzantısı yöntemi gösterir [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="e8a70-250">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="e8a70-251">Aşağıdaki kod Ara çağırır `Configure`:</span><span class="sxs-lookup"><span data-stu-id="e8a70-251">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="e8a70-252">Ara yazılım izlemelidir [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/) bağımlılıklarını kendi oluşturucusuna gösterme tarafından.</span><span class="sxs-lookup"><span data-stu-id="e8a70-252">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="e8a70-253">Ara yazılım yapılandırılmıştır kez başına *uygulama ömrü*.</span><span class="sxs-lookup"><span data-stu-id="e8a70-253">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="e8a70-254">Bkz: *istek başına bağımlılıkları* üstündeyse ara yazılım istek içinde Hizmetleri paylaşmasına gerekir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-254">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="e8a70-255">Ara yazılımı bileşenleri bağımlılıklarını bağımlılık ekleme Oluşturucu parametreleri üzerinden gelen çözebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e8a70-255">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="e8a70-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)Ayrıca ek parametreler doğrudan kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="e8a70-257">İstek başına bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="e8a70-257">Per-request dependencies</span></span>

<span data-ttu-id="e8a70-258">Ara yazılım değil istek başına, uygulama başlatma sırasında oluşturulur çünkü *kapsamlı* ara yazılım Oluşturucu tarafından kullanılan yaşam süresi olmayan paylaşılan hizmetler diğer bağımlılık tarafından eklenen türleriyle her isteği sırasında.</span><span class="sxs-lookup"><span data-stu-id="e8a70-258">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="e8a70-259">Gereken paylaşıyorsanız bir *kapsamlı* , Ara ve diğer türleri arasında hizmet, bu hizmetlere ekleme `Invoke` yöntemin imzası.</span><span class="sxs-lookup"><span data-stu-id="e8a70-259">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="e8a70-260">`Invoke` Yöntemi tarafından bağımlılık ekleme doldurulur ek parametreleri kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="e8a70-260">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="e8a70-261">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e8a70-261">For example:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="e8a70-262">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e8a70-262">Additional resources</span></span>

* [<span data-ttu-id="e8a70-263">Ara yazılım için geçirme HTTP modülleri</span><span class="sxs-lookup"><span data-stu-id="e8a70-263">Migrating HTTP Modules to Middleware</span></span>](xref:migration/http-modules)
* [<span data-ttu-id="e8a70-264">Uygulama Başlatma</span><span class="sxs-lookup"><span data-stu-id="e8a70-264">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="e8a70-265">İstek Özellikleri</span><span class="sxs-lookup"><span data-stu-id="e8a70-265">Request Features</span></span>](xref:fundamentals/request-features)
* [<span data-ttu-id="e8a70-266">Ara yazılımı Fabrika tabanlı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="e8a70-266">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="e8a70-267">Bir üçüncü taraf kapsayıcısı ile ara yazılımı Fabrika tabanlı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="e8a70-267">Factory-based middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
