---
title: ASP.NET Core ara yazılımı
author: rick-anderson
description: ASP.NET Core ara yazılım ve istek ardışık düzenini hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 02/17/2019
uid: fundamentals/middleware/index
ms.openlocfilehash: bac121441d6856ca79affe1a3130e5cbc76debd9
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901241"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="bd634-103">ASP.NET Core ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="bd634-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="bd634-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="bd634-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="bd634-105">Ara yazılım isteklerini ve yanıtlarını işlemek için bir uygulama ardışık birleştirilmiş bir yazılımdır.</span><span class="sxs-lookup"><span data-stu-id="bd634-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="bd634-106">Her bileşen için:</span><span class="sxs-lookup"><span data-stu-id="bd634-106">Each component:</span></span>

* <span data-ttu-id="bd634-107">İstek ardışık düzende sonraki bileşene geçmek bu seçeneği seçer.</span><span class="sxs-lookup"><span data-stu-id="bd634-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="bd634-108">İşlem hattı, iş önce ve sonraki bileşen sonra gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bd634-108">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="bd634-109">İstek Temsilciler, istek ardışık düzenini oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bd634-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="bd634-110">İstek temsilcileri her HTTP isteği işler.</span><span class="sxs-lookup"><span data-stu-id="bd634-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="bd634-111">Temsilcileri kullanarak yapılandırılmış olan istek <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, ve <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> genişletme yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="bd634-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="bd634-112">Tek tek istekler temsilci (satır içi ara yazılımı olarak adlandırılır) bir anonim yöntem belirtilen satır içi olabilir veya yeniden kullanılabilir bir sınıf içinde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="bd634-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="bd634-113">Bu yeniden kullanılabilir sınıfları ve satır içi anonim yöntemler *ara yazılım*ayrıca adlı *ara yazılımı bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="bd634-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="bd634-114">Her ara yazılım bileşeni istek ardışık düzende, ardışık düzende sonraki bileşene çağırma veya işlem hattı kısa devre sorumludur.</span><span class="sxs-lookup"><span data-stu-id="bd634-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="bd634-115">Olduğunda bir ara yazılım short-circuits, çağrıldığı bir *terminal ara yazılım* isteği işlemesini daha fazla ara yazılımı önlediği için.</span><span class="sxs-lookup"><span data-stu-id="bd634-115">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="bd634-116"><xref:migration/http-modules> İstek hatlarında ASP.NET Core ve ASP.NET arasındaki farkı açıklar 4.x ve daha fazla ara yazılım örnekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd634-116"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="bd634-117">Bir ara yazılım ardışık düzenini IApplicationBuilder ile oluşturma</span><span class="sxs-lookup"><span data-stu-id="bd634-117">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="bd634-118">İstek Temsilciler, birbiri ardına adlı bir dizi ASP.NET Core istek ardışık düzenini oluşur.</span><span class="sxs-lookup"><span data-stu-id="bd634-118">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="bd634-119">Kavram Aşağıdaki diyagramda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bd634-119">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="bd634-120">Yürütme iş parçacığını siyah okları izler.</span><span class="sxs-lookup"><span data-stu-id="bd634-120">The thread of execution follows the black arrows.</span></span>

![İstek işleme düzeni ulaşan, işlem üç middlewares ve uygulamadan ayrılmasını yanıt bir istek gösteriliyor.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="bd634-124">Her temsilci önce ve sonra İleri temsilci işlemleri gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="bd634-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="bd634-125">Bunlar işlem hattının sonraki aşamasında oluşan özel durumları yakalayabilirsiniz özel durum işleme temsilciler kanal içinde çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bd634-125">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="bd634-126">Tüm istekleri işleyen bir tek istek temsilci basit olası ASP.NET Core uygulaması ayarlar.</span><span class="sxs-lookup"><span data-stu-id="bd634-126">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="bd634-127">Bu durumda, bir gerçek istek ardışık düzeni dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="bd634-127">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="bd634-128">Bunun yerine, tek bir anonim işlev, her bir HTTP isteğine yanıt olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="bd634-128">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="bd634-129">İlk <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> temsilci işlem hattı sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="bd634-129">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="bd634-130">Birden çok istek temsilciler birlikte zincirleme <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="bd634-130">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="bd634-131">`next` Parametresi ardışık düzende sonraki temsilciyi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="bd634-131">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="bd634-132">İşlem hattı tarafından kısa devre oluşturur *değil* çağırma *sonraki* parametresi.</span><span class="sxs-lookup"><span data-stu-id="bd634-132">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="bd634-133">Aşağıdaki örnekte de gösterildiği gibi öncesinde ve sonrasında sonraki temsilci, genellikle eylemleri gerçekleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bd634-133">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

<span data-ttu-id="bd634-134">Bir temsilci sonraki temsilci atamak için bir istek başarısız olduğunda çağrılır *istek ardışık düzenini kısa devre*.</span><span class="sxs-lookup"><span data-stu-id="bd634-134">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="bd634-135">Gereksiz iş önlediği için kısa devre genellikle tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="bd634-135">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="bd634-136">Örneğin, [statik dosya ara yazılımlarını](xref:fundamentals/static-files) olarak davranıp bir *terminal ara yazılım* statik bir dosya için bir istek işleme ve kalan ardışık düzenini kısa devre.</span><span class="sxs-lookup"><span data-stu-id="bd634-136">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="bd634-137">Ara yazılım ardışık düzene daha fazla işleme sonlandıran bir ara yazılım koddan sonra hala işlemeden önce eklenen kendi `next.Invoke` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="bd634-137">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="bd634-138">Bununla birlikte, zaten gönderilen yanıt yazma girişiminde bulunulurken hakkında aşağıdaki uyarıyı görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="bd634-138">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="bd634-139">Remove() çağırmayın `next.Invoke` istemciye yanıt gönderildikten sonra.</span><span class="sxs-lookup"><span data-stu-id="bd634-139">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="bd634-140">Değişikliklerini <xref:Microsoft.AspNetCore.Http.HttpResponse> yanıt başlatıldıktan sonra bir özel durum.</span><span class="sxs-lookup"><span data-stu-id="bd634-140">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="bd634-141">Örneğin, üst bilgileri ve durum kodu ayarlama gibi değişiklikler, bir özel durum.</span><span class="sxs-lookup"><span data-stu-id="bd634-141">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="bd634-142">Yanıt gövdesi için çağırdıktan sonra Yazma `next`:</span><span class="sxs-lookup"><span data-stu-id="bd634-142">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="bd634-143">Protokol ihlali neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="bd634-143">May cause a protocol violation.</span></span> <span data-ttu-id="bd634-144">Örneğin, belirtilen birden fazla yazma `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="bd634-144">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="bd634-145">Gövde biçimi bozuk.</span><span class="sxs-lookup"><span data-stu-id="bd634-145">May corrupt the body format.</span></span> <span data-ttu-id="bd634-146">Örneğin, bir CSS dosyası için bir HTML altbilgi yazma.</span><span class="sxs-lookup"><span data-stu-id="bd634-146">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="bd634-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> üstbilgileri gönderildikten veya için gövde yazılmadan belirtmek için kullanışlı bir ipucudur.</span><span class="sxs-lookup"><span data-stu-id="bd634-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="bd634-148">Sipariş verme</span><span class="sxs-lookup"><span data-stu-id="bd634-148">Order</span></span>

<span data-ttu-id="bd634-149">Ara yazılım bileşenleri içinde eklenen sırasını `Startup.Configure` yöntemi, istekler ve yanıt için ters sırada ara yazılımı bileşenleri çağrılır sırasını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="bd634-149">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="bd634-150">Güvenlik, performans ve işlev için sırasını kritiktir.</span><span class="sxs-lookup"><span data-stu-id="bd634-150">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="bd634-151">Aşağıdaki `Startup.Configure` yöntemi yaygın uygulama senaryoları için ara yazılım bileşenlerini ekler:</span><span class="sxs-lookup"><span data-stu-id="bd634-151">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="bd634-152">Özel durum/hata işleme</span><span class="sxs-lookup"><span data-stu-id="bd634-152">Exception/error handling</span></span>
1. <span data-ttu-id="bd634-153">HTTP taşıma katı güvenlik protokolü</span><span class="sxs-lookup"><span data-stu-id="bd634-153">HTTP Strict Transport Security Protocol</span></span>
1. <span data-ttu-id="bd634-154">HTTPS yeniden yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="bd634-154">HTTPS redirection</span></span>
1. <span data-ttu-id="bd634-155">Statický souborový server</span><span class="sxs-lookup"><span data-stu-id="bd634-155">Static file server</span></span>
1. <span data-ttu-id="bd634-156">Tanımlama bilgisi ilkesi zorlama</span><span class="sxs-lookup"><span data-stu-id="bd634-156">Cookie policy enforcement</span></span>
1. <span data-ttu-id="bd634-157">Kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="bd634-157">Authentication</span></span>
1. <span data-ttu-id="bd634-158">Oturum</span><span class="sxs-lookup"><span data-stu-id="bd634-158">Session</span></span>
1. <span data-ttu-id="bd634-159">MVC</span><span class="sxs-lookup"><span data-stu-id="bd634-159">MVC</span></span>

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
    // Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // If the app uses session state, call Session Middleware after Cookie 
    // Policy Middleware and before MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. <span data-ttu-id="bd634-160">Özel durum/hata işleme</span><span class="sxs-lookup"><span data-stu-id="bd634-160">Exception/error handling</span></span>
1. <span data-ttu-id="bd634-161">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="bd634-161">Static files</span></span>
1. <span data-ttu-id="bd634-162">Kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="bd634-162">Authentication</span></span>
1. <span data-ttu-id="bd634-163">Oturum</span><span class="sxs-lookup"><span data-stu-id="bd634-163">Session</span></span>
1. <span data-ttu-id="bd634-164">MVC</span><span class="sxs-lookup"><span data-stu-id="bd634-164">MVC</span></span>

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

    // If the app uses session state, call UseSession before 
    // MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="bd634-165">Yukarıdaki örnek kodda, her bir ara yazılım genişletme yöntemi üzerinde kullanıma sunulan <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> aracılığıyla <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> ad alanı.</span><span class="sxs-lookup"><span data-stu-id="bd634-165">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="bd634-166"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ardışık düzene ilk ara yazılım bileşeni eklenir.</span><span class="sxs-lookup"><span data-stu-id="bd634-166"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="bd634-167">Bu nedenle, özel durum işleyicisi Ara sonraki çağrılarında oluşan özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="bd634-167">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="bd634-168">Böylece isteklerini işlemek ve kalan bileşenleri olmadan iki statik dosya ara yazılımlarını erken işlem hattında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="bd634-168">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="bd634-169">Statik dosya ara yazılımlarını sağlar **hiçbir** yetkilendirme denetimleri.</span><span class="sxs-lookup"><span data-stu-id="bd634-169">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="bd634-170">Tüm dosyaları sunulan işlem tarafından altında dahil olmak üzere *wwwroot*, genel olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bd634-170">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="bd634-171">Statik dosyaların güvenliğini sağlamak bir yaklaşım için bkz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="bd634-171">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bd634-172">Statik dosya ara yazılımı tarafından istek işlenmez, bu kimlik doğrulaması Ara yazılımıyla aktarılır (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), kimlik doğrulaması gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="bd634-172">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="bd634-173">Kimlik doğrulaması, kimliği doğrulanmamış istekler kısa devre oluşturur değil.</span><span class="sxs-lookup"><span data-stu-id="bd634-173">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="bd634-174">Kimlik doğrulaması ara yazılım kimlik doğrulaması istekleri olsa da, yalnızca belirli bir Razor sayfası veya MVC denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="bd634-174">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bd634-175">Statik dosya ara yazılımı tarafından istek işlenmez, bu kimlik Ara yazılımıyla aktarılır (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), kimlik doğrulaması gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="bd634-175">If the request isn't handled by Static File Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="bd634-176">Kimlik, kimliği doğrulanmamış istekler kısa devre oluşturur değil.</span><span class="sxs-lookup"><span data-stu-id="bd634-176">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="bd634-177">İstek kimlik doğrular olsa da, yalnızca belirli bir denetleyici ve eylem MVC seçtikten sonra yetkilendirme (ve reddetme) gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="bd634-177">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="bd634-178">Aşağıdaki örnek, statik dosyaların nerede statik dosya ara yazılımlarını önce yanıt sıkıştırma ara yazılımı tarafından işlenen bir ara yazılım sırasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="bd634-178">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="bd634-179">Bu ara yazılım siparişle sıkıştırılmış statik dosyaları değildir.</span><span class="sxs-lookup"><span data-stu-id="bd634-179">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="bd634-180">MVC yanıtlarından <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="bd634-180">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static File Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="bd634-181">Harita kullanın ve çalıştırma</span><span class="sxs-lookup"><span data-stu-id="bd634-181">Use, Run, and Map</span></span>

<span data-ttu-id="bd634-182">HTTP kullanarak işlem hattını yapılandırmak `Use`, `Run`, ve `Map`.</span><span class="sxs-lookup"><span data-stu-id="bd634-182">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="bd634-183">`Use` Yöntemi iki işlem hattı (diğer bir deyişle, çağırma değil, bir `next` istek temsilci).</span><span class="sxs-lookup"><span data-stu-id="bd634-183">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="bd634-184">`Run` bir kuralı ve bazı ara yazılımı bileşenleri getirebilir `Run[Middleware]` ardışık düzen sonunda çalışan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="bd634-184">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="bd634-185"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> Uzantılar, işlem hattı dallanma için bir kural kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bd634-185"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="bd634-186">`Map*` dalları istek ardışık düzenini belirtilen istek yolu eşleşmeleri üzerinde temel.</span><span class="sxs-lookup"><span data-stu-id="bd634-186">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="bd634-187">İstek yolu belirtilen yol ile başlarsa, dalı çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="bd634-187">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="bd634-188">Aşağıdaki tablo istekleri ve gelen yanıtları gösterir `http://localhost:1234` önceki kod kullanarak.</span><span class="sxs-lookup"><span data-stu-id="bd634-188">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="bd634-189">İstek</span><span class="sxs-lookup"><span data-stu-id="bd634-189">Request</span></span>             | <span data-ttu-id="bd634-190">Yanıt</span><span class="sxs-lookup"><span data-stu-id="bd634-190">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="bd634-191">1234</span><span class="sxs-lookup"><span data-stu-id="bd634-191">localhost:1234</span></span>      | <span data-ttu-id="bd634-192">Harita olmayan temsilci gelen Merhaba.</span><span class="sxs-lookup"><span data-stu-id="bd634-192">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="bd634-193">1234 / map1</span><span class="sxs-lookup"><span data-stu-id="bd634-193">localhost:1234/map1</span></span> | <span data-ttu-id="bd634-194">Test 1 eşleme</span><span class="sxs-lookup"><span data-stu-id="bd634-194">Map Test 1</span></span>                   |
| <span data-ttu-id="bd634-195">1234 / map2</span><span class="sxs-lookup"><span data-stu-id="bd634-195">localhost:1234/map2</span></span> | <span data-ttu-id="bd634-196">Harita Test 2</span><span class="sxs-lookup"><span data-stu-id="bd634-196">Map Test 2</span></span>                   |
| <span data-ttu-id="bd634-197">1234 / map3</span><span class="sxs-lookup"><span data-stu-id="bd634-197">localhost:1234/map3</span></span> | <span data-ttu-id="bd634-198">Harita olmayan temsilci gelen Merhaba.</span><span class="sxs-lookup"><span data-stu-id="bd634-198">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="bd634-199">Zaman `Map` olan kullanıldığında, eşleşen yolu segment(s) çıkarılır `HttpRequest.Path` ve için eklenen `HttpRequest.PathBase` her istek için.</span><span class="sxs-lookup"><span data-stu-id="bd634-199">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="bd634-200">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) dalları istek ardışık düzenini belirli bir koşul sonucuna göre.</span><span class="sxs-lookup"><span data-stu-id="bd634-200">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="bd634-201">Herhangi bir koşul türü `Func<HttpContext, bool>` istekleri işlem hattının yeni bir dala eşlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bd634-201">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="bd634-202">Aşağıdaki örnekte, bir koşul bir sorgu dizesi değişkeni varolup olmadığını algılamak için kullanılan `branch`:</span><span class="sxs-lookup"><span data-stu-id="bd634-202">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="bd634-203">Aşağıdaki tablo istekleri ve gelen yanıtları gösterir `http://localhost:1234` önceki kod kullanarak.</span><span class="sxs-lookup"><span data-stu-id="bd634-203">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="bd634-204">İstek</span><span class="sxs-lookup"><span data-stu-id="bd634-204">Request</span></span>                       | <span data-ttu-id="bd634-205">Yanıt</span><span class="sxs-lookup"><span data-stu-id="bd634-205">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="bd634-206">1234</span><span class="sxs-lookup"><span data-stu-id="bd634-206">localhost:1234</span></span>                | <span data-ttu-id="bd634-207">Harita olmayan temsilci gelen Merhaba.</span><span class="sxs-lookup"><span data-stu-id="bd634-207">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="bd634-208">1234 /? dal ana =</span><span class="sxs-lookup"><span data-stu-id="bd634-208">localhost:1234/?branch=master</span></span> | <span data-ttu-id="bd634-209">Dal kullanılan ana =</span><span class="sxs-lookup"><span data-stu-id="bd634-209">Branch used = master</span></span>         |

<span data-ttu-id="bd634-210">`Map` Örneğin, içe destekler:</span><span class="sxs-lookup"><span data-stu-id="bd634-210">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="bd634-211">`Map` Ayrıca birden fazla bölüm aynı anda eşleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bd634-211">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="bd634-212">Yerleşik ara yazılım</span><span class="sxs-lookup"><span data-stu-id="bd634-212">Built-in middleware</span></span>

<span data-ttu-id="bd634-213">ASP.NET Core aşağıdaki ara yazılımı bileşenleri ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="bd634-213">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="bd634-214">*Sipariş* sütun sağlar ara yazılım yerleştirme istek işleme ardışık düzeninde ve ara yazılım hangi koşullar altında ilgili notlar isteği işlemeyi sonlandır.</span><span class="sxs-lookup"><span data-stu-id="bd634-214">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="bd634-215">Bir ara yazılım ardışık düzen işleme isteği short-circuits ve istek önlediği daha da aşağı akış ara yazılım adlı bir *terminal ara yazılım*.</span><span class="sxs-lookup"><span data-stu-id="bd634-215">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="bd634-216">Kısa devre daha fazla bilgi için bkz: [IApplicationBuilder ile bir ara yazılım ardışık düzenini oluşturma](#create-a-middleware-pipeline-with-iapplicationbuilder) bölümü.</span><span class="sxs-lookup"><span data-stu-id="bd634-216">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="bd634-217">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="bd634-217">Middleware</span></span> | <span data-ttu-id="bd634-218">Açıklama</span><span class="sxs-lookup"><span data-stu-id="bd634-218">Description</span></span> | <span data-ttu-id="bd634-219">Sipariş verme</span><span class="sxs-lookup"><span data-stu-id="bd634-219">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="bd634-220">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="bd634-220">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="bd634-221">Kimlik doğrulama desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd634-221">Provides authentication support.</span></span> | <span data-ttu-id="bd634-222">Önce `HttpContext.User` gereklidir.</span><span class="sxs-lookup"><span data-stu-id="bd634-222">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="bd634-223">Terminal OAuth geri çağırmalar için.</span><span class="sxs-lookup"><span data-stu-id="bd634-223">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="bd634-224">Tanımlama bilgisi ilkesi</span><span class="sxs-lookup"><span data-stu-id="bd634-224">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="bd634-225">Onay kullanıcıların kişisel bilgilerini depolamak için ve izler, tanımlama bilgisi alanları için en düşük standartlara zorlayan `secure` ve `SameSite`.</span><span class="sxs-lookup"><span data-stu-id="bd634-225">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="bd634-226">Önce bir ara yazılım, tanımlama bilgileri verir.</span><span class="sxs-lookup"><span data-stu-id="bd634-226">Before middleware that issues cookies.</span></span> <span data-ttu-id="bd634-227">Örnekler: Kimlik doğrulaması, oturum, MVC (TempData).</span><span class="sxs-lookup"><span data-stu-id="bd634-227">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="bd634-228">CORS</span><span class="sxs-lookup"><span data-stu-id="bd634-228">CORS</span></span>](xref:security/cors) | <span data-ttu-id="bd634-229">Çıkış noktaları arası kaynak paylaşımını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="bd634-229">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="bd634-230">CORS kullanan bileşenleri önce.</span><span class="sxs-lookup"><span data-stu-id="bd634-230">Before components that use CORS.</span></span> |
| [<span data-ttu-id="bd634-231">Özel Durum İşleme</span><span class="sxs-lookup"><span data-stu-id="bd634-231">Exception Handling</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="bd634-232">Özel durumları işler.</span><span class="sxs-lookup"><span data-stu-id="bd634-232">Handles exceptions.</span></span> | <span data-ttu-id="bd634-233">Bileşenlerinden önce bu hataları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bd634-233">Before components that generate errors.</span></span> |
| [<span data-ttu-id="bd634-234">İletilen üstbilgileri</span><span class="sxs-lookup"><span data-stu-id="bd634-234">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="bd634-235">Geçerli istek üzerine proxy üstbilgileri iletir.</span><span class="sxs-lookup"><span data-stu-id="bd634-235">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="bd634-236">Bileşenlerinden önce güncelleştirilmiş alanları kullanır.</span><span class="sxs-lookup"><span data-stu-id="bd634-236">Before components that consume the updated fields.</span></span> <span data-ttu-id="bd634-237">Örnekler: düzeni, ana bilgisayar, istemci IP'si yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bd634-237">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="bd634-238">Sistem durumu denetimi</span><span class="sxs-lookup"><span data-stu-id="bd634-238">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="bd634-239">ASP.NET Core uygulaması ve veritabanı kullanılabilirlik denetimi gibi bağımlılıklarını durumunu denetler.</span><span class="sxs-lookup"><span data-stu-id="bd634-239">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="bd634-240">İstek bir sistem durumu onay uç noktası eşleşiyorsa terminal.</span><span class="sxs-lookup"><span data-stu-id="bd634-240">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="bd634-241">HTTP yöntemini geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="bd634-241">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="bd634-242">Bu yöntemi geçersiz kılmak gelen bir POST isteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd634-242">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="bd634-243">Bileşenlerinden önce güncelleştirilen yöntemi kullanır.</span><span class="sxs-lookup"><span data-stu-id="bd634-243">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="bd634-244">HTTPS yeniden yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="bd634-244">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="bd634-245">Tüm HTTP isteklerini (ASP.NET Core 2.1 veya üzeri) HTTPS'ye yönlendiriyor.</span><span class="sxs-lookup"><span data-stu-id="bd634-245">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="bd634-246">Bileşenlerinden önce bu URL'yi kullanır.</span><span class="sxs-lookup"><span data-stu-id="bd634-246">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="bd634-247">HTTP katı aktarım güvenliği (HSTS)</span><span class="sxs-lookup"><span data-stu-id="bd634-247">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="bd634-248">Bir özel yanıt üst bilgisi (ASP.NET Core 2.1 veya üzeri) ekleyen güvenlik geliştirmesi ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="bd634-248">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="bd634-249">Yanıtları gönderilmeden önce ve sonra değiştirme isteklerini bileşenleri.</span><span class="sxs-lookup"><span data-stu-id="bd634-249">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="bd634-250">Örnekler: Üst bilgiler, iletilen URL yeniden yazma.</span><span class="sxs-lookup"><span data-stu-id="bd634-250">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="bd634-251">MVC</span><span class="sxs-lookup"><span data-stu-id="bd634-251">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="bd634-252">MVC/Razor sayfaları (ASP.NET Core 2.0 veya sonraki bir sürümü) ile istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="bd634-252">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="bd634-253">İstek bir Terminal varsa bir rotayla eşleşen.</span><span class="sxs-lookup"><span data-stu-id="bd634-253">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="bd634-254">OWIN</span><span class="sxs-lookup"><span data-stu-id="bd634-254">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="bd634-255">OWIN tabanlı uygulamalar, sunucuları ve ara yazılım ile birlikte çalışma.</span><span class="sxs-lookup"><span data-stu-id="bd634-255">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="bd634-256">Terminal OWIN ara yazılımı tam isteği işler.</span><span class="sxs-lookup"><span data-stu-id="bd634-256">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="bd634-257">Yanıtları Önbelleğe Alma</span><span class="sxs-lookup"><span data-stu-id="bd634-257">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="bd634-258">Yanıtları önbelleğe alma işlemi için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd634-258">Provides support for caching responses.</span></span> | <span data-ttu-id="bd634-259">Önbelleğe alma gerektiren bileşenler önce.</span><span class="sxs-lookup"><span data-stu-id="bd634-259">Before components that require caching.</span></span> |
| [<span data-ttu-id="bd634-260">Yanıt sıkıştırma</span><span class="sxs-lookup"><span data-stu-id="bd634-260">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="bd634-261">Destek için yanıtları sıkıştırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd634-261">Provides support for compressing responses.</span></span> | <span data-ttu-id="bd634-262">Sıkıştırma iste bileşenlerinden önce.</span><span class="sxs-lookup"><span data-stu-id="bd634-262">Before components that require compression.</span></span> |
| [<span data-ttu-id="bd634-263">İstek yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="bd634-263">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="bd634-264">Yerelleştirme desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd634-264">Provides localization support.</span></span> | <span data-ttu-id="bd634-265">Yerelleştirme önemli bileşenlerinden önce.</span><span class="sxs-lookup"><span data-stu-id="bd634-265">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="bd634-266">Yönlendirme</span><span class="sxs-lookup"><span data-stu-id="bd634-266">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="bd634-267">Tanımlar ve istek yolları kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="bd634-267">Defines and constrains request routes.</span></span> | <span data-ttu-id="bd634-268">Yollar eşleştirmek için terminal.</span><span class="sxs-lookup"><span data-stu-id="bd634-268">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="bd634-269">Oturum</span><span class="sxs-lookup"><span data-stu-id="bd634-269">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="bd634-270">Kullanıcı oturumlarını yönetmek için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd634-270">Provides support for managing user sessions.</span></span> | <span data-ttu-id="bd634-271">Bileşenlerinden önce oturumu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="bd634-271">Before components that require Session.</span></span> |
| [<span data-ttu-id="bd634-272">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="bd634-272">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="bd634-273">Statik dosya ve Dizin tarama hizmet vermek için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd634-273">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="bd634-274">İstek bir Terminal varsa, bir dosya ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="bd634-274">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="bd634-275">URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="bd634-275">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="bd634-276">URL yeniden yazma ve istekleri yönlendirme için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd634-276">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="bd634-277">Bileşenlerinden önce bu URL'yi kullanır.</span><span class="sxs-lookup"><span data-stu-id="bd634-277">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="bd634-278">WebSockets</span><span class="sxs-lookup"><span data-stu-id="bd634-278">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="bd634-279">WebSockets Protokolü sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd634-279">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="bd634-280">WebSocket isteklerini kabul etmek için gerekli bileşenleri önce.</span><span class="sxs-lookup"><span data-stu-id="bd634-280">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="bd634-281">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bd634-281">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
