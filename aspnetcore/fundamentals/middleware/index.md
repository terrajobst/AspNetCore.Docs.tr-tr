---
title: ASP.NET Core ara yazılımı
author: rick-anderson
description: ASP.NET Core ara yazılım ve istek ardışık düzenini hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: e6dc76b7cb80e0dfda102df5aefb5d9ce9b821ed
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055812"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="bd115-103">ASP.NET Core ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="bd115-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="bd115-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="bd115-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="bd115-105">Ara yazılım isteklerini ve yanıtlarını işlemek için bir uygulama ardışık birleştirilmiş bir yazılımdır.</span><span class="sxs-lookup"><span data-stu-id="bd115-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="bd115-106">Her bileşen için:</span><span class="sxs-lookup"><span data-stu-id="bd115-106">Each component:</span></span>

* <span data-ttu-id="bd115-107">İstek ardışık düzende sonraki bileşene geçmek bu seçeneği seçer.</span><span class="sxs-lookup"><span data-stu-id="bd115-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="bd115-108">İş, önce ve ardışık düzende sonraki bileşene çağrıldıktan sonra gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bd115-108">Can perform work before and after the next component in the pipeline is invoked.</span></span>

<span data-ttu-id="bd115-109">İstek Temsilciler, istek ardışık düzenini oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bd115-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="bd115-110">İstek temsilcileri her HTTP isteği işler.</span><span class="sxs-lookup"><span data-stu-id="bd115-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="bd115-111">Temsilcileri kullanarak yapılandırılmış olan istek <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, ve <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> genişletme yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="bd115-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="bd115-112">Tek tek istekler temsilci (satır içi ara yazılımı olarak adlandırılır) bir anonim yöntem belirtilen satır içi olabilir veya yeniden kullanılabilir bir sınıf içinde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="bd115-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="bd115-113">Bu yeniden kullanılabilir sınıfları ve satır içi anonim yöntemler *ara yazılım*ayrıca adlı *ara yazılımı bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="bd115-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="bd115-114">Her ara yazılım bileşeni istek ardışık düzende, ardışık düzende sonraki bileşene çağırma veya işlem hattı kısa devre sorumludur.</span><span class="sxs-lookup"><span data-stu-id="bd115-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span>

<span data-ttu-id="bd115-115"><xref:migration/http-modules> İstek hatlarında ASP.NET Core ve ASP.NET arasındaki farkı açıklar 4.x ve daha fazla ara yazılım örnekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd115-115"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="bd115-116">Bir ara yazılım ardışık düzenini IApplicationBuilder ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="bd115-116">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="bd115-117">İstek Temsilciler, birbiri ardına adlı bir dizi ASP.NET Core istek ardışık düzenini oluşur.</span><span class="sxs-lookup"><span data-stu-id="bd115-117">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="bd115-118">Kavram Aşağıdaki diyagramda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bd115-118">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="bd115-119">Yürütme iş parçacığını siyah okları izler.</span><span class="sxs-lookup"><span data-stu-id="bd115-119">The thread of execution follows the black arrows.</span></span>

![İstek işleme düzeni ulaşan, işlem üç middlewares ve uygulamadan ayrılmasını yanıt bir istek gösteriliyor.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="bd115-123">Her temsilci önce ve sonra İleri temsilci işlemleri gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="bd115-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="bd115-124">Bir istek çağrılır sonraki temsilcisine geçirmemesi bir temsilci da karar verebilirsiniz *istek ardışık düzenini kısa devre*.</span><span class="sxs-lookup"><span data-stu-id="bd115-124">A delegate can also decide to not pass a request to the next delegate, which is called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="bd115-125">Gereksiz iş önlediği için kısa devre genellikle tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="bd115-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="bd115-126">Örneğin, statik dosya ara yazılımı statik bir dosya için bir istek dönün ve kalan ardışık düzenini kısa devre oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bd115-126">For example, Static Files Middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="bd115-127">Bunlar işlem hattının sonraki aşamasında oluşan özel durumları yakalayabilirsiniz özel durum işleme temsilciler kanal içinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="bd115-127">Exception-handling delegates are called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="bd115-128">Tüm istekleri işleyen bir tek istek temsilci basit olası ASP.NET Core uygulaması ayarlar.</span><span class="sxs-lookup"><span data-stu-id="bd115-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="bd115-129">Bu durumda, bir gerçek istek ardışık düzeni dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="bd115-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="bd115-130">Bunun yerine, tek bir anonim işlev, her bir HTTP isteğine yanıt olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="bd115-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="bd115-131">İlk <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> temsilci işlem hattı sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="bd115-131">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="bd115-132">Birden çok istek temsilciler birlikte zincirleme <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="bd115-132">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="bd115-133">`next` Parametresi ardışık düzende sonraki temsilciyi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="bd115-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="bd115-134">İşlem hattı tarafından kısa devre oluşturur *değil* çağırma *sonraki* parametresi.</span><span class="sxs-lookup"><span data-stu-id="bd115-134">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="bd115-135">Aşağıdaki örnekte de gösterildiği gibi öncesinde ve sonrasında sonraki temsilci, genellikle eylemleri gerçekleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bd115-135">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> <span data-ttu-id="bd115-136">Remove() çağırmayın `next.Invoke` istemciye yanıt gönderildikten sonra.</span><span class="sxs-lookup"><span data-stu-id="bd115-136">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="bd115-137">Değişikliklerini <xref:Microsoft.AspNetCore.Http.HttpResponse> yanıt başlatıldıktan sonra bir özel durum.</span><span class="sxs-lookup"><span data-stu-id="bd115-137">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="bd115-138">Örneğin, üst bilgileri ve durum kodu ayarlama gibi değişiklikler, bir özel durum.</span><span class="sxs-lookup"><span data-stu-id="bd115-138">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="bd115-139">Yanıt gövdesi için çağırdıktan sonra Yazma `next`:</span><span class="sxs-lookup"><span data-stu-id="bd115-139">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="bd115-140">Protokol ihlali neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="bd115-140">May cause a protocol violation.</span></span> <span data-ttu-id="bd115-141">Örneğin, belirtilen birden fazla yazma `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="bd115-141">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="bd115-142">Gövde biçimi bozuk.</span><span class="sxs-lookup"><span data-stu-id="bd115-142">May corrupt the body format.</span></span> <span data-ttu-id="bd115-143">Örneğin, bir CSS dosyası için bir HTML altbilgi yazma.</span><span class="sxs-lookup"><span data-stu-id="bd115-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="bd115-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> üstbilgileri gönderildikten veya için gövde yazılmadan belirtmek için kullanışlı bir ipucudur.</span><span class="sxs-lookup"><span data-stu-id="bd115-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="bd115-145">Sırası</span><span class="sxs-lookup"><span data-stu-id="bd115-145">Order</span></span>

<span data-ttu-id="bd115-146">Ara yazılım bileşenleri içinde eklenen sırasını `Startup.Configure` yöntemi, istekler ve yanıt için ters sırada ara yazılımı bileşenleri çağrılır sırasını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="bd115-146">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="bd115-147">Güvenlik, performans ve işlev için sırasını kritiktir.</span><span class="sxs-lookup"><span data-stu-id="bd115-147">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="bd115-148">Aşağıdaki `Configure` yöntemi aşağıdaki ara yazılım bileşenlerini ekler:</span><span class="sxs-lookup"><span data-stu-id="bd115-148">The following `Configure` method adds the following middleware components:</span></span>

1. <span data-ttu-id="bd115-149">Özel durum/hata işleme</span><span class="sxs-lookup"><span data-stu-id="bd115-149">Exception/error handling</span></span>
2. <span data-ttu-id="bd115-150">Statický souborový server</span><span class="sxs-lookup"><span data-stu-id="bd115-150">Static file server</span></span>
3. <span data-ttu-id="bd115-151">Kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="bd115-151">Authentication</span></span>
4. <span data-ttu-id="bd115-152">MVC</span><span class="sxs-lookup"><span data-stu-id="bd115-152">MVC</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    //   Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="bd115-153">Yukarıdaki örnek kodda, her bir ara yazılım genişletme yöntemi üzerinde kullanıma sunulan <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> aracılığıyla <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> ad alanı.</span><span class="sxs-lookup"><span data-stu-id="bd115-153">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="bd115-154"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ardışık düzene ilk ara yazılım bileşeni eklenir.</span><span class="sxs-lookup"><span data-stu-id="bd115-154"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="bd115-155">Bu nedenle, özel durum işleyicisi Ara sonraki çağrılarında oluşan özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="bd115-155">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="bd115-156">İstekleri işlemek ve kalan bileşenleri olmadan iki statik dosya ara yazılımı erken işlem hattında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="bd115-156">Static Files Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="bd115-157">Statik dosya ara yazılım sağlar **hiçbir** yetkilendirme denetimleri.</span><span class="sxs-lookup"><span data-stu-id="bd115-157">The Static Files Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="bd115-158">Tüm dosyaları sunulan işlem tarafından altında dahil olmak üzere *wwwroot*, genel olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bd115-158">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="bd115-159">Statik dosyaların güvenliğini sağlamak bir yaklaşım için bkz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="bd115-159">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bd115-160">Statik dosya ara yazılımı tarafından istek işlenmez, bu kimlik doğrulaması Ara yazılımıyla aktarılır (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), kimlik doğrulaması gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="bd115-160">If the request isn't handled by the Static Files Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="bd115-161">Kimlik doğrulaması, kimliği doğrulanmamış istekler kısa devre oluşturur değil.</span><span class="sxs-lookup"><span data-stu-id="bd115-161">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="bd115-162">Kimlik doğrulaması ara yazılım kimlik doğrulaması istekleri olsa da, yalnızca belirli bir Razor sayfası veya MVC denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="bd115-162">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bd115-163">Statik dosya ara yazılımı tarafından istek işlenmez, bu kimlik Ara yazılımıyla aktarılır (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), kimlik doğrulaması gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="bd115-163">If the request isn't handled by Static Files Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="bd115-164">Kimlik, kimliği doğrulanmamış istekler kısa devre oluşturur değil.</span><span class="sxs-lookup"><span data-stu-id="bd115-164">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="bd115-165">İstek kimlik doğrular olsa da, yalnızca belirli bir denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="bd115-165">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="bd115-166">Aşağıdaki örnek, statik dosyaların nerede yanıt sıkıştırma ara yazılımı önce statik dosya ara yazılımı tarafından işlenen bir ara yazılım sırasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="bd115-166">The following example demonstrates a middleware order where requests for static files are handled by Static Files Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="bd115-167">Bu ara yazılım siparişle sıkıştırılmış statik dosyaları değildir.</span><span class="sxs-lookup"><span data-stu-id="bd115-167">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="bd115-168">MVC yanıtlarından <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="bd115-168">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static Files Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="bd115-169">Harita kullanın ve çalıştırma</span><span class="sxs-lookup"><span data-stu-id="bd115-169">Use, Run, and Map</span></span>

<span data-ttu-id="bd115-170">HTTP kullanarak işlem hattını yapılandırmak `Use`, `Run`, ve `Map`.</span><span class="sxs-lookup"><span data-stu-id="bd115-170">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="bd115-171">`Use` Yöntemi iki işlem hattı (diğer bir deyişle, çağırma değil, bir `next` istek temsilci).</span><span class="sxs-lookup"><span data-stu-id="bd115-171">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="bd115-172">`Run` bir kuralı ve bazı ara yazılımı bileşenleri getirebilir `Run[Middleware]` ardışık düzen sonunda çalışan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="bd115-172">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="bd115-173"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> Uzantılar, işlem hattı dallanma için bir kural kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bd115-173"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="bd115-174">`Map*` dalları istek ardışık düzenini belirtilen istek yolu eşleşmeleri üzerinde temel.</span><span class="sxs-lookup"><span data-stu-id="bd115-174">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="bd115-175">İstek yolu belirtilen yol ile başlarsa, dalı çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="bd115-175">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="bd115-176">Aşağıdaki tablo istekleri ve gelen yanıtları gösterir `http://localhost:1234` önceki kod kullanarak.</span><span class="sxs-lookup"><span data-stu-id="bd115-176">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="bd115-177">İstek</span><span class="sxs-lookup"><span data-stu-id="bd115-177">Request</span></span>             | <span data-ttu-id="bd115-178">Yanıt</span><span class="sxs-lookup"><span data-stu-id="bd115-178">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="bd115-179">1234</span><span class="sxs-lookup"><span data-stu-id="bd115-179">localhost:1234</span></span>      | <span data-ttu-id="bd115-180">Harita olmayan temsilci gelen Merhaba.</span><span class="sxs-lookup"><span data-stu-id="bd115-180">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="bd115-181">1234 / map1</span><span class="sxs-lookup"><span data-stu-id="bd115-181">localhost:1234/map1</span></span> | <span data-ttu-id="bd115-182">Test 1 eşleme</span><span class="sxs-lookup"><span data-stu-id="bd115-182">Map Test 1</span></span>                   |
| <span data-ttu-id="bd115-183">1234 / map2</span><span class="sxs-lookup"><span data-stu-id="bd115-183">localhost:1234/map2</span></span> | <span data-ttu-id="bd115-184">Harita Test 2</span><span class="sxs-lookup"><span data-stu-id="bd115-184">Map Test 2</span></span>                   |
| <span data-ttu-id="bd115-185">1234 / map3</span><span class="sxs-lookup"><span data-stu-id="bd115-185">localhost:1234/map3</span></span> | <span data-ttu-id="bd115-186">Harita olmayan temsilci gelen Merhaba.</span><span class="sxs-lookup"><span data-stu-id="bd115-186">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="bd115-187">Zaman `Map` olan kullanıldığında, eşleşen yolu segment(s) çıkarılır `HttpRequest.Path` ve için eklenen `HttpRequest.PathBase` her istek için.</span><span class="sxs-lookup"><span data-stu-id="bd115-187">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="bd115-188">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) dalları istek ardışık düzenini belirli bir koşul sonucuna göre.</span><span class="sxs-lookup"><span data-stu-id="bd115-188">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="bd115-189">Herhangi bir koşul türü `Func<HttpContext, bool>` istekleri işlem hattının yeni bir dala eşlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bd115-189">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="bd115-190">Aşağıdaki örnekte, bir koşul bir sorgu dizesi değişkeni varolup olmadığını algılamak için kullanılan `branch`:</span><span class="sxs-lookup"><span data-stu-id="bd115-190">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="bd115-191">Aşağıdaki tablo istekleri ve gelen yanıtları gösterir `http://localhost:1234` önceki kod kullanarak.</span><span class="sxs-lookup"><span data-stu-id="bd115-191">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="bd115-192">İstek</span><span class="sxs-lookup"><span data-stu-id="bd115-192">Request</span></span>                       | <span data-ttu-id="bd115-193">Yanıt</span><span class="sxs-lookup"><span data-stu-id="bd115-193">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="bd115-194">1234</span><span class="sxs-lookup"><span data-stu-id="bd115-194">localhost:1234</span></span>                | <span data-ttu-id="bd115-195">Harita olmayan temsilci gelen Merhaba.</span><span class="sxs-lookup"><span data-stu-id="bd115-195">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="bd115-196">1234 /? dal ana =</span><span class="sxs-lookup"><span data-stu-id="bd115-196">localhost:1234/?branch=master</span></span> | <span data-ttu-id="bd115-197">Dal kullanılan ana =</span><span class="sxs-lookup"><span data-stu-id="bd115-197">Branch used = master</span></span>         |

<span data-ttu-id="bd115-198">`Map` Örneğin, içe destekler:</span><span class="sxs-lookup"><span data-stu-id="bd115-198">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

<span data-ttu-id="bd115-199">`Map` Ayrıca birden fazla bölüm aynı anda eşleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bd115-199">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="bd115-200">Yerleşik ara yazılım</span><span class="sxs-lookup"><span data-stu-id="bd115-200">Built-in middleware</span></span>

<span data-ttu-id="bd115-201">ASP.NET Core aşağıdaki ara yazılımı bileşenleri ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="bd115-201">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="bd115-202">*Sipariş* sütun Ara yerleştirme istek ardışık düzenini ve ara yazılım hangi koşullar altında ilgili notlar istek sonlandırmak ve diğer ara yazılımdan, bir isteğin işlenmesini önlemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd115-202">The *Order* column provides notes on the middleware's placement in the request pipeline and under what conditions the middleware may terminate the request and prevent other middleware from processing a request.</span></span>

| <span data-ttu-id="bd115-203">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="bd115-203">Middleware</span></span> | <span data-ttu-id="bd115-204">Açıklama</span><span class="sxs-lookup"><span data-stu-id="bd115-204">Description</span></span> | <span data-ttu-id="bd115-205">Sırası</span><span class="sxs-lookup"><span data-stu-id="bd115-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="bd115-206">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="bd115-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="bd115-207">Kimlik doğrulama desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd115-207">Provides authentication support.</span></span> | <span data-ttu-id="bd115-208">Önce `HttpContext.User` gereklidir.</span><span class="sxs-lookup"><span data-stu-id="bd115-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="bd115-209">Terminal OAuth geri çağırmalar için.</span><span class="sxs-lookup"><span data-stu-id="bd115-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="bd115-210">CORS</span><span class="sxs-lookup"><span data-stu-id="bd115-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="bd115-211">Çıkış noktaları arası kaynak paylaşımını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="bd115-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="bd115-212">CORS kullanan bileşenleri önce.</span><span class="sxs-lookup"><span data-stu-id="bd115-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="bd115-213">Tanılama</span><span class="sxs-lookup"><span data-stu-id="bd115-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="bd115-214">Tanılama yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="bd115-214">Configures diagnostics.</span></span> | <span data-ttu-id="bd115-215">Bileşenlerinden önce bu hataları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bd115-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="bd115-216">İletilen üstbilgileri</span><span class="sxs-lookup"><span data-stu-id="bd115-216">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="bd115-217">Geçerli istek üzerine proxy üstbilgileri iletir.</span><span class="sxs-lookup"><span data-stu-id="bd115-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="bd115-218">Önce güncelleştirilmiş alanları kullanma bileşenleri (örnekler: Düzen, konak, istemci IP'si yöntemi).</span><span class="sxs-lookup"><span data-stu-id="bd115-218">Before components that consume the updated fields (examples: scheme, host, client IP, method).</span></span> |
| [<span data-ttu-id="bd115-219">HTTP yöntemini geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="bd115-219">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="bd115-220">Bu yöntemi geçersiz kılmak gelen bir POST isteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd115-220">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="bd115-221">Bileşenlerinden önce güncelleştirilen yöntemi kullanır.</span><span class="sxs-lookup"><span data-stu-id="bd115-221">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="bd115-222">HTTPS yeniden yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="bd115-222">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="bd115-223">Tüm HTTP isteklerini (ASP.NET Core 2.1 veya üzeri) HTTPS'ye yönlendiriyor.</span><span class="sxs-lookup"><span data-stu-id="bd115-223">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="bd115-224">Bileşenlerinden önce bu URL'yi kullanır.</span><span class="sxs-lookup"><span data-stu-id="bd115-224">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="bd115-225">HTTP katı aktarım güvenliği (HSTS)</span><span class="sxs-lookup"><span data-stu-id="bd115-225">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="bd115-226">Bir özel yanıt üst bilgisi (ASP.NET Core 2.1 veya üzeri) ekleyen güvenlik geliştirmesi ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="bd115-226">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="bd115-227">Yanıtları gönderilmeden önce ve sonra değiştirme isteklerini (örneğin, iletilen üstbilgileri, URL yeniden yazma) bileşenleri.</span><span class="sxs-lookup"><span data-stu-id="bd115-227">Before responses are sent and after components that modify requests (for example, Forwarded Headers, URL Rewriting).</span></span> |
| [<span data-ttu-id="bd115-228">MVC</span><span class="sxs-lookup"><span data-stu-id="bd115-228">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="bd115-229">MVC/Razor sayfaları (ASP.NET Core 2.0 veya sonraki bir sürümü) ile istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="bd115-229">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="bd115-230">İstek bir Terminal varsa bir rotayla eşleşen.</span><span class="sxs-lookup"><span data-stu-id="bd115-230">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="bd115-231">OWIN</span><span class="sxs-lookup"><span data-stu-id="bd115-231">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="bd115-232">OWIN tabanlı uygulamalar, sunucuları ve ara yazılım ile birlikte çalışma.</span><span class="sxs-lookup"><span data-stu-id="bd115-232">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="bd115-233">Terminal OWIN ara yazılımı tam isteği işler.</span><span class="sxs-lookup"><span data-stu-id="bd115-233">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="bd115-234">Yanıtları Önbelleğe Alma</span><span class="sxs-lookup"><span data-stu-id="bd115-234">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="bd115-235">Yanıtları önbelleğe alma işlemi için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd115-235">Provides support for caching responses.</span></span> | <span data-ttu-id="bd115-236">Önbelleğe alma gerektiren bileşenler önce.</span><span class="sxs-lookup"><span data-stu-id="bd115-236">Before components that require caching.</span></span> |
| [<span data-ttu-id="bd115-237">Yanıt sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="bd115-237">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="bd115-238">Destek için yanıtları sıkıştırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd115-238">Provides support for compressing responses.</span></span> | <span data-ttu-id="bd115-239">Sıkıştırma iste bileşenlerinden önce.</span><span class="sxs-lookup"><span data-stu-id="bd115-239">Before components that require compression.</span></span> |
| [<span data-ttu-id="bd115-240">İstek yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="bd115-240">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="bd115-241">Yerelleştirme desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd115-241">Provides localization support.</span></span> | <span data-ttu-id="bd115-242">Yerelleştirme önemli bileşenlerinden önce.</span><span class="sxs-lookup"><span data-stu-id="bd115-242">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="bd115-243">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="bd115-243">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="bd115-244">Tanımlar ve istek yolları kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="bd115-244">Defines and constrains request routes.</span></span> | <span data-ttu-id="bd115-245">Yollar eşleştirmek için terminal.</span><span class="sxs-lookup"><span data-stu-id="bd115-245">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="bd115-246">Oturum</span><span class="sxs-lookup"><span data-stu-id="bd115-246">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="bd115-247">Kullanıcı oturumlarını yönetmek için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd115-247">Provides support for managing user sessions.</span></span> | <span data-ttu-id="bd115-248">Bileşenlerinden önce oturumu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="bd115-248">Before components that require Session.</span></span> |
| [<span data-ttu-id="bd115-249">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="bd115-249">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="bd115-250">Statik dosya ve Dizin tarama hizmet vermek için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd115-250">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="bd115-251">İstek bir Terminal varsa, bir dosya ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="bd115-251">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="bd115-252">URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="bd115-252">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="bd115-253">URL yeniden yazma ve istekleri yönlendirme için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd115-253">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="bd115-254">Bileşenlerinden önce bu URL'yi kullanır.</span><span class="sxs-lookup"><span data-stu-id="bd115-254">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="bd115-255">WebSockets</span><span class="sxs-lookup"><span data-stu-id="bd115-255">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="bd115-256">WebSockets Protokolü sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd115-256">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="bd115-257">WebSocket isteklerini kabul etmek için gerekli bileşenleri önce.</span><span class="sxs-lookup"><span data-stu-id="bd115-257">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="write-middleware"></a><span data-ttu-id="bd115-258">Ara yazılım yazma</span><span class="sxs-lookup"><span data-stu-id="bd115-258">Write middleware</span></span>

<span data-ttu-id="bd115-259">Ara yazılım genellikle bir sınıfta kapsüllenmiş ve bir genişletme yöntemi ile kullanıma sunulan.</span><span class="sxs-lookup"><span data-stu-id="bd115-259">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="bd115-260">Kültür geçerli istek için Sorgu dizesinden ayarlar aşağıdaki Ara göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="bd115-260">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="bd115-261">Yukarıdaki örnek kod, bir ara yazılım bileşeni oluşturma göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bd115-261">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="bd115-262">Bkz: ASP.NET Core'nın yerleşik yerelleştirme desteğini <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="bd115-262">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="bd115-263">Ara yazılım kültür, örneğin geçirerek sınayabilirsiniz `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="bd115-263">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="bd115-264">Aşağıdaki kod, bir sınıf için ara yazılım temsilci taşır:</span><span class="sxs-lookup"><span data-stu-id="bd115-264">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bd115-265">Ara yazılım `Task` yöntemin adı olmalıdır `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="bd115-265">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="bd115-266">ASP.NET Core 2.0 veya sonraki sürümlerde, ad ya da olabilir `Invoke` veya `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="bd115-266">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

<span data-ttu-id="bd115-267">Aşağıdaki uzantı yöntemi ara yazılımı üzerinden kullanıma sunan <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="bd115-267">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="bd115-268">Aşağıdaki kod ara yazılımı gelen çağrıları `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="bd115-268">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="bd115-269">Ara yazılım izlemelidir [açık bağımlılıkları İlkesi](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) tarafından kendi oluşturucusuna bağımlılıkları gösterme.</span><span class="sxs-lookup"><span data-stu-id="bd115-269">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="bd115-270">Ara yazılım yapılandırılmıştır kez başına *uygulama ömrü*.</span><span class="sxs-lookup"><span data-stu-id="bd115-270">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="bd115-271">Bkz: [istek başına bağımlılıkları](#per-request-dependencies) istek içinde ara yazılım ile Hizmetleri paylaşan gerekiyorsa bölümü.</span><span class="sxs-lookup"><span data-stu-id="bd115-271">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="bd115-272">Ara yazılım bileşenleri, bunların bağımlılıklarını giderebilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) Oluşturucu parametresi üzerinden.</span><span class="sxs-lookup"><span data-stu-id="bd115-272">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="bd115-273">[UseMiddleware&lt;T&gt; ](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) ek parametreler doğrudan da kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="bd115-273">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="bd115-274">İstek başına bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="bd115-274">Per-request dependencies</span></span>

<span data-ttu-id="bd115-275">Ara yazılım değil istek, uygulama başlatma sırasında oluşturulur çünkü *kapsamlı* ara yazılım oluşturucular tarafından kullanılan etkin kalma süresi olmayan paylaşılan hizmetler diğer bağımlılık eklenen türleriyle her isteği sırasında.</span><span class="sxs-lookup"><span data-stu-id="bd115-275">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="bd115-276">Paylaşmanız gerekir durumunda bir *kapsamlı* hizmet Ara yazılımınızı ve diğer türleri arasında bu hizmetlere ekleme `Invoke` yöntemin imzası.</span><span class="sxs-lookup"><span data-stu-id="bd115-276">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="bd115-277">`Invoke` Yöntemi tarafından DI doldurulur ek parametreleri kabul edebilir:</span><span class="sxs-lookup"><span data-stu-id="bd115-277">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="bd115-278">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bd115-278">Additional resources</span></span>

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
