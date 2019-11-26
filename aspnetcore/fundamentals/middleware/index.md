---
title: ASP.NET Core ara yazılımı
author: rick-anderson
description: ASP.NET Core ara yazılımı ve istek işlem hattı hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/08/2019
uid: fundamentals/middleware/index
ms.openlocfilehash: d678f3d1f6ca10e486543a2965506236e4e61b82
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239843"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="16cb7-103">ASP.NET Core ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="16cb7-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="16cb7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="16cb7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="16cb7-105">Ara yazılım, istekleri ve yanıtları işlemek için bir uygulama ardışık düzenine çevrilmiş yazılımdır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="16cb7-106">Her bileşen:</span><span class="sxs-lookup"><span data-stu-id="16cb7-106">Each component:</span></span>

* <span data-ttu-id="16cb7-107">İsteğin işlem hattında sonraki bileşene geçirilip geçemeyeceğini seçer.</span><span class="sxs-lookup"><span data-stu-id="16cb7-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="16cb7-108">İşlem hattındaki sonraki bileşenden önce ve sonra iş gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-108">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="16cb7-109">İstek işlem hattını oluşturmak için istek temsilcileri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="16cb7-110">İstek temsilcileri her HTTP isteğini işler.</span><span class="sxs-lookup"><span data-stu-id="16cb7-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="16cb7-111">İstek temsilcileri <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>ve <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> uzantı yöntemleri kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="16cb7-112">Tek bir istek temsilcisi, bir anonim Yöntem (çevrimiçi ara yazılım olarak adlandırılır) olarak satır içinde belirtilebilir veya yeniden kullanılabilir bir sınıfta tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="16cb7-113">Bu yeniden kullanılabilir sınıflar ve satır içi anonim yöntemler, *Ara yazılım bileşenleri*olarak da adlandırılan *ara yazılımlar*.</span><span class="sxs-lookup"><span data-stu-id="16cb7-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="16cb7-114">İstek ardışık düzeninde bulunan her bir ara yazılım bileşeni, işlem hattındaki bir sonraki bileşeni çağırmaktan veya işlem hattının kısa süreli olarak sağlanmasından sorumludur.</span><span class="sxs-lookup"><span data-stu-id="16cb7-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="16cb7-115">Bir ara yazılım kısa devre dışı bırakıldığında, bu, diğer ara yazılımların isteği işlemesini önlediği için *Terminal ara yazılımı* olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-115">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="16cb7-116"><xref:migration/http-modules>, ASP.NET Core ve ASP.NET 4. x içindeki istek işlem hatları arasındaki farkı açıklar ve ek ara yazılım örnekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-116"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides additional middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="16cb7-117">IApplicationBuilder ile bir ara yazılım işlem hattı oluşturma</span><span class="sxs-lookup"><span data-stu-id="16cb7-117">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="16cb7-118">ASP.NET Core isteği ardışık düzeni, bir dizi istekten oluşur ve bunlardan sonra çağırılır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-118">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="16cb7-119">Aşağıdaki diyagramda kavram gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-119">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="16cb7-120">Yürütmenin iş parçacığı siyah okları izler.</span><span class="sxs-lookup"><span data-stu-id="16cb7-120">The thread of execution follows the black arrows.</span></span>

![İsteğin geliş, üç middlewares üzerinden işleme ve uygulamayı bırakma yanıtı gösteren istek işleme deseninin.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="16cb7-124">Her temsilci bir sonraki temsilciden önce ve sonra işlemleri gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="16cb7-125">Özel durum işleme temsilcileri işlem hattında erken çağrılmalıdır, bu sayede işlem hattının sonraki aşamalarında oluşan özel durumları yakalayabilirler.</span><span class="sxs-lookup"><span data-stu-id="16cb7-125">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="16cb7-126">Mümkün olan en basit ASP.NET Core uygulaması, tüm istekleri işleyen tek bir istek temsilcisi kurar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-126">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="16cb7-127">Bu durum gerçek bir istek işlem hattı içermez.</span><span class="sxs-lookup"><span data-stu-id="16cb7-127">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="16cb7-128">Bunun yerine, her HTTP isteğine yanıt olarak tek bir anonim işlev çağırılır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-128">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs)]

<span data-ttu-id="16cb7-129">İlk <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> temsilci, işlem hattını sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-129">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="16cb7-130">Birden çok istek temsilciyi <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>ile birlikte zinciri.</span><span class="sxs-lookup"><span data-stu-id="16cb7-130">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="16cb7-131">`next` parametresi, ardışık düzendeki bir sonraki temsilciyi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="16cb7-131">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="16cb7-132">Ardışık düzen, *sonraki* *parametreyi çağırarak işlem* hattı için kısa devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16cb7-132">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="16cb7-133">Aşağıdaki örnekte gösterildiği gibi genellikle sonraki temsilciden önce ve sonra eylemler gerçekleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="16cb7-133">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs)]

<span data-ttu-id="16cb7-134">Bir temsilci bir sonraki temsilciye bir istek iletmezse, *istek ardışık düzenini, kısa*devre olarak gerçekleştirmektir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-134">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="16cb7-135">Gereksiz çalışmayı önlediği için kısa devre, genellikle tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-135">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="16cb7-136">Örneğin, [statik dosya ara yazılımı](xref:fundamentals/static-files) , bir statik dosya için bir isteği işleyerek ve işlem hattının geri kalanını gerçekleştirerek bir *Terminal ara yazılımı* görevi görebilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-136">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="16cb7-137">Daha fazla işlemeyi sonlandıran ara yazılım, `next.Invoke` deyimlerinden sonra kodu işlerken işlem hattına eklenen ara yazılımlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-137">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="16cb7-138">Ancak, zaten gönderilmiş bir yanıta yazma girişimi hakkında aşağıdaki uyarıya bakın.</span><span class="sxs-lookup"><span data-stu-id="16cb7-138">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="16cb7-139">İstemciye yanıt gönderildikten sonra `next.Invoke` çağırmayın.</span><span class="sxs-lookup"><span data-stu-id="16cb7-139">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="16cb7-140">Yanıt başladıktan sonra <xref:Microsoft.AspNetCore.Http.HttpResponse> değişiklikler özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="16cb7-140">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="16cb7-141">Örneğin, üstbilgileri ayarlama ve durum kodu gibi değişiklikler özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="16cb7-141">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="16cb7-142">`next`çağrıldıktan sonra yanıt gövdesine yazma:</span><span class="sxs-lookup"><span data-stu-id="16cb7-142">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="16cb7-143">Protokol ihlaline neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-143">May cause a protocol violation.</span></span> <span data-ttu-id="16cb7-144">Örneğin, belirtilen `Content-Length`daha fazla yazma.</span><span class="sxs-lookup"><span data-stu-id="16cb7-144">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="16cb7-145">Gövde biçimi bozulabilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-145">May corrupt the body format.</span></span> <span data-ttu-id="16cb7-146">Örneğin, bir CSS dosyasına bir HTML altbilgisi yazma.</span><span class="sxs-lookup"><span data-stu-id="16cb7-146">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="16cb7-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*>, üstbilgilerin gönderilip gönderilmediğini veya gövdenin yazıldığını göstermek için faydalı bir ipucu.</span><span class="sxs-lookup"><span data-stu-id="16cb7-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

<a name="order"></a>

## <a name="middleware-order"></a><span data-ttu-id="16cb7-148">Ara yazılım sırası</span><span class="sxs-lookup"><span data-stu-id="16cb7-148">Middleware order</span></span>

<span data-ttu-id="16cb7-149">Ara yazılım bileşenlerinin `Startup.Configure` yöntemi içinde eklendiği sıra, ara yazılım bileşenlerinin isteklerde çağrıldığı sırayı ve yanıtın ters sırasını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-149">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="16cb7-150">Sıra, güvenlik, performans ve işlevsellik açısından **önemlidir** .</span><span class="sxs-lookup"><span data-stu-id="16cb7-150">The order is **critical** for security, performance, and functionality.</span></span>

<span data-ttu-id="16cb7-151">Aşağıdaki `Startup.Configure` yöntemi, güvenlikle ilgili ara yazılım bileşenlerini önerilen sırayla ekler:</span><span class="sxs-lookup"><span data-stu-id="16cb7-151">The following `Startup.Configure` method adds security related middleware components in the recommended order:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/snapshot/StartupAll3.cs?name=snippet)]

<span data-ttu-id="16cb7-152">Önceki kodda:</span><span class="sxs-lookup"><span data-stu-id="16cb7-152">In the preceding code:</span></span>

* <span data-ttu-id="16cb7-153">[Bireysel kullanıcılar hesaplarıyla](xref:security/authentication/identity) yeni bir Web uygulaması oluştururken eklenmemiş olan ara yazılım, yorum yapılır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-153">Middleware that is not added when creating a new web app with [individual users accounts](xref:security/authentication/identity) is commented out.</span></span>
* <span data-ttu-id="16cb7-154">Her ara yazılımın bu tam sıra, ancak birçok do olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="16cb7-154">Not every middleware needs to go in this exact order, but many do.</span></span> <span data-ttu-id="16cb7-155">Örneğin, `UseCors`, `UseAuthentication`ve `UseAuthorization` gösterilen sırayla gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-155">For example, `UseCors`, `UseAuthentication`, and `UseAuthorization` must go in the order shown.</span></span>

<span data-ttu-id="16cb7-156">Aşağıdaki `Startup.Configure` yöntemi, genel uygulama senaryoları için ara yazılım bileşenleri ekler:</span><span class="sxs-lookup"><span data-stu-id="16cb7-156">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

1. <span data-ttu-id="16cb7-157">Özel durum/hata işleme</span><span class="sxs-lookup"><span data-stu-id="16cb7-157">Exception/error handling</span></span>
   * <span data-ttu-id="16cb7-158">Uygulama geliştirme ortamında çalıştığında:</span><span class="sxs-lookup"><span data-stu-id="16cb7-158">When the app runs in the Development environment:</span></span>
     * <span data-ttu-id="16cb7-159">Geliştirici özel durum sayfası ara yazılımı (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) uygulama çalışma zamanı hatalarını raporlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-159">Developer Exception Page Middleware (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) reports app runtime errors.</span></span>
     * <span data-ttu-id="16cb7-160">Veritabanı hata sayfası ara yazılımı veritabanı çalışma zamanı hatalarını raporlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-160">Database Error Page Middleware reports database runtime errors.</span></span>
   * <span data-ttu-id="16cb7-161">Uygulama, üretim ortamında çalıştığında:</span><span class="sxs-lookup"><span data-stu-id="16cb7-161">When the app runs in the Production environment:</span></span>
     * <span data-ttu-id="16cb7-162">Özel durum Işleyici ara yazılımı (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) aşağıdaki middlewares oluşturulan özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-162">Exception Handler Middleware (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) catches exceptions thrown in the following middlewares.</span></span>
     * <span data-ttu-id="16cb7-163">HTTP katı aktarım güvenliği Protokolü (HSTS) ara yazılımı (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) `Strict-Transport-Security` üstbilgisini ekler.</span><span class="sxs-lookup"><span data-stu-id="16cb7-163">HTTP Strict Transport Security Protocol (HSTS) Middleware (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) adds the `Strict-Transport-Security` header.</span></span>
1. <span data-ttu-id="16cb7-164">HTTPS yeniden yönlendirme ara yazılımı (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) HTTP isteklerini HTTPS 'ye yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-164">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) redirects HTTP requests to HTTPS.</span></span>
1. <span data-ttu-id="16cb7-165">Statik dosya ara yazılımı (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) statik dosyaları ve kısa devre dışı istek işlemeyi döndürür.</span><span class="sxs-lookup"><span data-stu-id="16cb7-165">Static File Middleware (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) returns static files and short-circuits further request processing.</span></span>
1. <span data-ttu-id="16cb7-166">Tanımlama bilgisi Ilkesi ara yazılımı (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) uygulamayı AB Genel Veri Koruma Yönetmeliği (GDPR) düzenlemelerine uyar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-166">Cookie Policy Middleware (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) conforms the app to the EU General Data Protection Regulation (GDPR) regulations.</span></span>
1. <span data-ttu-id="16cb7-167">İstekleri yönlendirmek için ara yazılım (`UseRouting`).</span><span class="sxs-lookup"><span data-stu-id="16cb7-167">Routing Middleware (`UseRouting`) to route requests.</span></span>
1. <span data-ttu-id="16cb7-168">Kimlik doğrulama ara yazılımı (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), güvenli kaynaklara erişim izni vermeden önce kullanıcının kimliğini doğrulamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-168">Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) attempts to authenticate the user before they're allowed access to secure resources.</span></span>
1. <span data-ttu-id="16cb7-169">Yetkilendirme ara yazılımı (`UseAuthorization`), bir kullanıcıya güvenli kaynaklara erişim yetkisi verir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-169">Authorization Middleware (`UseAuthorization`) authorizes a user to access secure resources.</span></span>
1. <span data-ttu-id="16cb7-170">Oturum ara yazılımı (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) oturum durumunu oluşturur ve korur.</span><span class="sxs-lookup"><span data-stu-id="16cb7-170">Session Middleware (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) establishes and maintains session state.</span></span> <span data-ttu-id="16cb7-171">Uygulama oturum durumunu kullanıyorsa, tanımlama bilgisi Ilkesi ara yazılımı ve MVC ara yazılımı öncesinde oturum ara yazılımını çağırın.</span><span class="sxs-lookup"><span data-stu-id="16cb7-171">If the app uses session state, call Session Middleware after Cookie Policy Middleware and before MVC Middleware.</span></span>
1. <span data-ttu-id="16cb7-172">İstek ardışık düzenine Razor Pages uç noktaları eklemek için uç nokta yönlendirme ara yazılımı (`MapRazorPages``UseEndpoints`).</span><span class="sxs-lookup"><span data-stu-id="16cb7-172">Endpoint Routing Middleware (`UseEndpoints` with `MapRazorPages`) to add Razor Pages endpoints to the request pipeline.</span></span>

<!--

FUTURE UPDATE

On the next topic overhaul/release update, add API crosslink to "Database Error Page Middleware" in Item 1 of the list ...

Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*

... when available via the API docs.

-->

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();
    app.UseRouting();
    app.UseAuthentication();
    app.UseAuthorization();
    app.UseSession();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapRazorPages();
    });
}
```

<span data-ttu-id="16cb7-173">Yukarıdaki örnek kodda, her bir ara yazılım uzantısı yöntemi <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> ad alanı aracılığıyla <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> gösterilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-173">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="16cb7-174"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>, işlem hattına eklenen ilk ara yazılım bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-174"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="16cb7-175">Bu nedenle, özel durum Işleyicisi ara yazılımı sonraki çağrılarında oluşan tüm özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-175">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="16cb7-176">Statik dosya ara yazılımı, geri kalan bileşenlere geçmeden istekleri ve kısa devre dışı bırakabilirsiniz. bu sayede işlem hattının başlarında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-176">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="16cb7-177">Statik dosya ara **yazılımı yetkilendirme denetimleri sağlamaz.**</span><span class="sxs-lookup"><span data-stu-id="16cb7-177">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="16cb7-178">*Wwwroot*altındakiler de dahil olmak üzere statik dosya ara yazılımı tarafından sunulan tüm dosyalar herkese açık bir şekilde sunulur.</span><span class="sxs-lookup"><span data-stu-id="16cb7-178">Any files served by Static File Middleware, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="16cb7-179">Statik dosyaların güvenliğini sağlamaya yönelik bir yaklaşım için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="16cb7-179">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

<span data-ttu-id="16cb7-180">İstek statik dosya ara yazılımı tarafından işlenmemişse, kimlik doğrulaması yapan kimlik doğrulama ara yazılım (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) üzerinden geçirilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-180">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="16cb7-181">Kimlik doğrulaması kısa devre dışı kimliği doğrulanmamış istekler değildir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-181">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="16cb7-182">Kimlik doğrulama ara yazılımı isteklerin kimliğini doğrulayabilse de, yetkilendirme (ve reddetme) yalnızca MVC, belirli bir Razor sayfası veya MVC denetleyicisi ve eylemi seçerse oluşur.</span><span class="sxs-lookup"><span data-stu-id="16cb7-182">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

<span data-ttu-id="16cb7-183">Aşağıdaki örnek, yanıt sıkıştırma ara yazılımı ile önce statik dosya isteklerinin statik dosya ara yazılımı tarafından işlendiği bir ara yazılım sırasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-183">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="16cb7-184">Statik dosyalar bu ara yazılım sırasıyla sıkıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="16cb7-184">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="16cb7-185">Razor Pages yanıtları sıkıştırılabilirler.</span><span class="sxs-lookup"><span data-stu-id="16cb7-185">The Razor Pages responses can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files aren't compressed by Static File Middleware.
    app.UseStaticFiles();

    app.UseResponseCompression();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapRazorPages();
    });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/snapshot/Startup22.cs?name=snippet)]

<span data-ttu-id="16cb7-186">Önceki kodda:</span><span class="sxs-lookup"><span data-stu-id="16cb7-186">In the preceding code:</span></span>

* <span data-ttu-id="16cb7-187">[Bireysel kullanıcılar hesaplarıyla](xref:security/authentication/identity) yeni bir Web uygulaması oluştururken eklenmemiş olan ara yazılım, yorum yapılır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-187">Middleware that is not added when creating a new web app with [individual users accounts](xref:security/authentication/identity) is commented out.</span></span>
* <span data-ttu-id="16cb7-188">Her ara yazılımın bu tam sıra, ancak birçok do olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="16cb7-188">Not every middleware needs to go in this exact order, but many do.</span></span> <span data-ttu-id="16cb7-189">Örneğin, `UseCors` ve `UseAuthentication` gösterilen sırada olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-189">For example, `UseCors` and `UseAuthentication` must go in the order shown.</span></span>

<span data-ttu-id="16cb7-190">Aşağıdaki `Startup.Configure` yöntemi, genel uygulama senaryoları için ara yazılım bileşenleri ekler:</span><span class="sxs-lookup"><span data-stu-id="16cb7-190">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

1. <span data-ttu-id="16cb7-191">Özel durum/hata işleme</span><span class="sxs-lookup"><span data-stu-id="16cb7-191">Exception/error handling</span></span>
   * <span data-ttu-id="16cb7-192">Uygulama geliştirme ortamında çalıştığında:</span><span class="sxs-lookup"><span data-stu-id="16cb7-192">When the app runs in the Development environment:</span></span>
     * <span data-ttu-id="16cb7-193">Geliştirici özel durum sayfası ara yazılımı (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) uygulama çalışma zamanı hatalarını raporlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-193">Developer Exception Page Middleware (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) reports app runtime errors.</span></span>
     * <span data-ttu-id="16cb7-194">Veritabanı hata sayfası ara yazılımı (`Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`) veritabanı çalışma zamanı hatalarını raporlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-194">Database Error Page Middleware (`Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`) reports database runtime errors.</span></span>
   * <span data-ttu-id="16cb7-195">Uygulama, üretim ortamında çalıştığında:</span><span class="sxs-lookup"><span data-stu-id="16cb7-195">When the app runs in the Production environment:</span></span>
     * <span data-ttu-id="16cb7-196">Özel durum Işleyici ara yazılımı (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) aşağıdaki middlewares oluşturulan özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-196">Exception Handler Middleware (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) catches exceptions thrown in the following middlewares.</span></span>
     * <span data-ttu-id="16cb7-197">HTTP katı aktarım güvenliği Protokolü (HSTS) ara yazılımı (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) `Strict-Transport-Security` üstbilgisini ekler.</span><span class="sxs-lookup"><span data-stu-id="16cb7-197">HTTP Strict Transport Security Protocol (HSTS) Middleware (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) adds the `Strict-Transport-Security` header.</span></span>
1. <span data-ttu-id="16cb7-198">HTTPS yeniden yönlendirme ara yazılımı (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) HTTP isteklerini HTTPS 'ye yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-198">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) redirects HTTP requests to HTTPS.</span></span>
1. <span data-ttu-id="16cb7-199">Statik dosya ara yazılımı (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) statik dosyaları ve kısa devre dışı istek işlemeyi döndürür.</span><span class="sxs-lookup"><span data-stu-id="16cb7-199">Static File Middleware (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) returns static files and short-circuits further request processing.</span></span>
1. <span data-ttu-id="16cb7-200">Tanımlama bilgisi Ilkesi ara yazılımı (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) uygulamayı AB Genel Veri Koruma Yönetmeliği (GDPR) düzenlemelerine uyar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-200">Cookie Policy Middleware (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) conforms the app to the EU General Data Protection Regulation (GDPR) regulations.</span></span>
1. <span data-ttu-id="16cb7-201">Kimlik doğrulama ara yazılımı (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), güvenli kaynaklara erişim izni vermeden önce kullanıcının kimliğini doğrulamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-201">Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) attempts to authenticate the user before they're allowed access to secure resources.</span></span>
1. <span data-ttu-id="16cb7-202">Oturum ara yazılımı (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) oturum durumunu oluşturur ve korur.</span><span class="sxs-lookup"><span data-stu-id="16cb7-202">Session Middleware (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) establishes and maintains session state.</span></span> <span data-ttu-id="16cb7-203">Uygulama oturum durumunu kullanıyorsa, tanımlama bilgisi Ilkesi ara yazılımı ve MVC ara yazılımı öncesinde oturum ara yazılımını çağırın.</span><span class="sxs-lookup"><span data-stu-id="16cb7-203">If the app uses session state, call Session Middleware after Cookie Policy Middleware and before MVC Middleware.</span></span>
1. <span data-ttu-id="16cb7-204">MVC (<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>) istek ardışık düzenine MVC eklemek için.</span><span class="sxs-lookup"><span data-stu-id="16cb7-204">MVC (<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>) to add MVC to the request pipeline.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();
    app.UseAuthentication();
    app.UseSession();
    app.UseMvc();
}
```

<span data-ttu-id="16cb7-205">Yukarıdaki örnek kodda, her bir ara yazılım uzantısı yöntemi <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> ad alanı aracılığıyla <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> gösterilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-205">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="16cb7-206"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>, işlem hattına eklenen ilk ara yazılım bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-206"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="16cb7-207">Bu nedenle, özel durum Işleyicisi ara yazılımı sonraki çağrılarında oluşan tüm özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-207">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="16cb7-208">Statik dosya ara yazılımı, geri kalan bileşenlere geçmeden istekleri ve kısa devre dışı bırakabilirsiniz. bu sayede işlem hattının başlarında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-208">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="16cb7-209">Statik dosya ara **yazılımı yetkilendirme denetimleri sağlamaz.**</span><span class="sxs-lookup"><span data-stu-id="16cb7-209">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="16cb7-210">*Wwwroot*altındakiler de dahil olmak üzere statik dosya ara yazılımı tarafından sunulan tüm dosyalar herkese açık bir şekilde sunulur.</span><span class="sxs-lookup"><span data-stu-id="16cb7-210">Any files served by Static File Middleware, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="16cb7-211">Statik dosyaların güvenliğini sağlamaya yönelik bir yaklaşım için bkz. <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="16cb7-211">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

<span data-ttu-id="16cb7-212">İstek statik dosya ara yazılımı tarafından işlenmemişse, kimlik doğrulaması yapan kimlik doğrulama ara yazılım (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) üzerinden geçirilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-212">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="16cb7-213">Kimlik doğrulaması kısa devre dışı kimliği doğrulanmamış istekler değildir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-213">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="16cb7-214">Kimlik doğrulama ara yazılımı isteklerin kimliğini doğrulayabilse de, yetkilendirme (ve reddetme) yalnızca MVC, belirli bir Razor sayfası veya MVC denetleyicisi ve eylemi seçerse oluşur.</span><span class="sxs-lookup"><span data-stu-id="16cb7-214">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

<span data-ttu-id="16cb7-215">Aşağıdaki örnek, yanıt sıkıştırma ara yazılımı ile önce statik dosya isteklerinin statik dosya ara yazılımı tarafından işlendiği bir ara yazılım sırasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-215">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="16cb7-216">Statik dosyalar bu ara yazılım sırasıyla sıkıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="16cb7-216">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="16cb7-217"><xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> MVC yanıtları sıkıştırılabilirler.</span><span class="sxs-lookup"><span data-stu-id="16cb7-217">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files aren't compressed by Static File Middleware.
    app.UseStaticFiles();

    app.UseResponseCompression();

    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

## <a name="use-run-and-map"></a><span data-ttu-id="16cb7-218">Kullanın, çalıştırın ve eşleyin</span><span class="sxs-lookup"><span data-stu-id="16cb7-218">Use, Run, and Map</span></span>

<span data-ttu-id="16cb7-219"><xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>, <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>ve <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>kullanarak HTTP işlem hattını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="16cb7-219">Configure the HTTP pipeline using <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>, <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, and <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>.</span></span> <span data-ttu-id="16cb7-220">`Use` yöntemi, işlem hattı kısa devre dışı olabilir (yani, bir `next` isteği temsilcisi çağırmazsa).</span><span class="sxs-lookup"><span data-stu-id="16cb7-220">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="16cb7-221">`Run` bir kuraldır ve bazı ara yazılım bileşenleri, işlem hattının sonunda çalışan `Run[Middleware]` Yöntemler sunabilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-221">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="16cb7-222"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> uzantıları, işlem hattının dallanması için bir kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-222"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="16cb7-223">`Map`, istek ardışık düzenini, verilen istek yolunun eşleşenleri temelinde dallandırır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-223">`Map` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="16cb7-224">İstek yolu verilen yol ile başlıyorsa, dal yürütülür.</span><span class="sxs-lookup"><span data-stu-id="16cb7-224">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs)]

<span data-ttu-id="16cb7-225">Aşağıdaki tabloda, önceki kodu kullanarak `http://localhost:1234` gelen istekler ve yanıtlar gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-225">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="16cb7-226">İstek</span><span class="sxs-lookup"><span data-stu-id="16cb7-226">Request</span></span>             | <span data-ttu-id="16cb7-227">Yanıt</span><span class="sxs-lookup"><span data-stu-id="16cb7-227">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="16cb7-228">localhost: 1234</span><span class="sxs-lookup"><span data-stu-id="16cb7-228">localhost:1234</span></span>      | <span data-ttu-id="16cb7-229">Eşleme olmayan temsilciden Merhaba.</span><span class="sxs-lookup"><span data-stu-id="16cb7-229">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="16cb7-230">localhost: 1234/Map1</span><span class="sxs-lookup"><span data-stu-id="16cb7-230">localhost:1234/map1</span></span> | <span data-ttu-id="16cb7-231">Eşleme testi 1</span><span class="sxs-lookup"><span data-stu-id="16cb7-231">Map Test 1</span></span>                   |
| <span data-ttu-id="16cb7-232">localhost: 1234/MAP2</span><span class="sxs-lookup"><span data-stu-id="16cb7-232">localhost:1234/map2</span></span> | <span data-ttu-id="16cb7-233">Eşleme testi 2</span><span class="sxs-lookup"><span data-stu-id="16cb7-233">Map Test 2</span></span>                   |
| <span data-ttu-id="16cb7-234">localhost: 1234/map3</span><span class="sxs-lookup"><span data-stu-id="16cb7-234">localhost:1234/map3</span></span> | <span data-ttu-id="16cb7-235">Eşleme olmayan temsilciden Merhaba.</span><span class="sxs-lookup"><span data-stu-id="16cb7-235">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="16cb7-236">`Map` kullanıldığında, eşleşen yol kesimleri `HttpRequest.Path` kaldırılır ve her istek için `HttpRequest.PathBase` eklenir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-236">When `Map` is used, the matched path segments are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="16cb7-237"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*>, belirtilen koşulun sonucuna göre istek ardışık düzenini dallandırır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-237"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="16cb7-238">`Func<HttpContext, bool>` türündeki herhangi bir koşul, istekleri işlem hattının yeni bir dalına eşlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-238">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="16cb7-239">Aşağıdaki örnekte, bir sorgu dizesi değişkeninin varlığını algılamak için bir koşul kullanılır `branch`:</span><span class="sxs-lookup"><span data-stu-id="16cb7-239">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs)]

<span data-ttu-id="16cb7-240">Aşağıdaki tabloda, önceki kodu kullanarak `http://localhost:1234` gelen istekler ve yanıtlar gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-240">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="16cb7-241">İstek</span><span class="sxs-lookup"><span data-stu-id="16cb7-241">Request</span></span>                       | <span data-ttu-id="16cb7-242">Yanıt</span><span class="sxs-lookup"><span data-stu-id="16cb7-242">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="16cb7-243">localhost: 1234</span><span class="sxs-lookup"><span data-stu-id="16cb7-243">localhost:1234</span></span>                | <span data-ttu-id="16cb7-244">Eşleme olmayan temsilciden Merhaba.</span><span class="sxs-lookup"><span data-stu-id="16cb7-244">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="16cb7-245">localhost: 1234/? dalı = ana</span><span class="sxs-lookup"><span data-stu-id="16cb7-245">localhost:1234/?branch=master</span></span> | <span data-ttu-id="16cb7-246">Kullanılan dal = ana</span><span class="sxs-lookup"><span data-stu-id="16cb7-246">Branch used = master</span></span>         |

<span data-ttu-id="16cb7-247">`Map` iç içe geçirmeyi destekler, örneğin:</span><span class="sxs-lookup"><span data-stu-id="16cb7-247">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="16cb7-248">`Map` aynı anda birden çok kesimde eşleşir:</span><span class="sxs-lookup"><span data-stu-id="16cb7-248">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="16cb7-249">Yerleşik ara yazılım</span><span class="sxs-lookup"><span data-stu-id="16cb7-249">Built-in middleware</span></span>

<span data-ttu-id="16cb7-250">ASP.NET Core aşağıdaki ara yazılım bileşenleriyle birlikte gönderilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-250">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="16cb7-251">*Order* sütunu, istek işleme ardışık düzeninde ara yazılım yerleştirme ve ara yazılımın istek işlemeyi sonlandırabilecekleri koşullar bölümünde notlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-251">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="16cb7-252">Bir ara yazılım, istek işlem hattının ne kadar kısa süreli olduğunu ve daha fazla aşağı akış ara yazılımı bir isteği işlemesini engelliyorsa, bu, *Terminal ara yazılımı*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-252">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="16cb7-253">Kısa devre oluşturma hakkında daha fazla bilgi için, [IApplicationBuilder ile bir ara yazılım işlem hattı oluşturma](#create-a-middleware-pipeline-with-iapplicationbuilder) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="16cb7-253">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="16cb7-254">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="16cb7-254">Middleware</span></span> | <span data-ttu-id="16cb7-255">Açıklama</span><span class="sxs-lookup"><span data-stu-id="16cb7-255">Description</span></span> | <span data-ttu-id="16cb7-256">Sipariş verme</span><span class="sxs-lookup"><span data-stu-id="16cb7-256">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="16cb7-257">Kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="16cb7-257">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="16cb7-258">Kimlik doğrulama desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-258">Provides authentication support.</span></span> | <span data-ttu-id="16cb7-259">`HttpContext.User` önce.</span><span class="sxs-lookup"><span data-stu-id="16cb7-259">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="16cb7-260">OAuth geri çağırmaları için Terminal.</span><span class="sxs-lookup"><span data-stu-id="16cb7-260">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="16cb7-261">Tanımlama bilgisi Ilkesi</span><span class="sxs-lookup"><span data-stu-id="16cb7-261">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="16cb7-262">Kişisel bilgileri depolamak için kullanıcılardan onay izler ve `secure` ve `SameSite`gibi tanımlama bilgisi alanları için en düşük standartları uygular.</span><span class="sxs-lookup"><span data-stu-id="16cb7-262">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="16cb7-263">Tanımlama bilgilerini veren ara yazılım öncesi.</span><span class="sxs-lookup"><span data-stu-id="16cb7-263">Before middleware that issues cookies.</span></span> <span data-ttu-id="16cb7-264">Örnekler: Authentication, Session, MVC (TempData).</span><span class="sxs-lookup"><span data-stu-id="16cb7-264">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="16cb7-265">CORS</span><span class="sxs-lookup"><span data-stu-id="16cb7-265">CORS</span></span>](xref:security/cors) | <span data-ttu-id="16cb7-266">Çıkış noktaları arası kaynak paylaşımını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="16cb7-266">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="16cb7-267">CORS kullanan bileşenlerden önce.</span><span class="sxs-lookup"><span data-stu-id="16cb7-267">Before components that use CORS.</span></span> |
| [<span data-ttu-id="16cb7-268">Tanılama</span><span class="sxs-lookup"><span data-stu-id="16cb7-268">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="16cb7-269">Geliştirici özel durum sayfası, özel durum işleme, durum kodu sayfaları ve yeni uygulamalar için varsayılan Web sayfası sağlayan çeşitli ayrı middlewares.</span><span class="sxs-lookup"><span data-stu-id="16cb7-269">Several separate middlewares that provide a developer exception page, exception handling, status code pages, and the default web page for new apps.</span></span> | <span data-ttu-id="16cb7-270">Hata oluşturan bileşenlerden önce.</span><span class="sxs-lookup"><span data-stu-id="16cb7-270">Before components that generate errors.</span></span> <span data-ttu-id="16cb7-271">Özel durumlar için Terminal veya yeni uygulamalar için varsayılan Web sayfasına hizmet sunma.</span><span class="sxs-lookup"><span data-stu-id="16cb7-271">Terminal for exceptions or serving the default web page for new apps.</span></span> |
| [<span data-ttu-id="16cb7-272">İletilen üstbilgiler</span><span class="sxs-lookup"><span data-stu-id="16cb7-272">Forwarded Headers</span></span>](xref:host-and-deploy/proxy-load-balancer) | <span data-ttu-id="16cb7-273">Proxy üst bilgilerini geçerli istek üzerine iletir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-273">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="16cb7-274">Güncelleştirilmiş alanları kullanan bileşenlerden önce.</span><span class="sxs-lookup"><span data-stu-id="16cb7-274">Before components that consume the updated fields.</span></span> <span data-ttu-id="16cb7-275">Örnekler: Scheme, Host, istemci IP, yöntem.</span><span class="sxs-lookup"><span data-stu-id="16cb7-275">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="16cb7-276">Sistem durumu denetimi</span><span class="sxs-lookup"><span data-stu-id="16cb7-276">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="16cb7-277">ASP.NET Core uygulamasının sistem durumunu ve bağımlılıklarını denetler (örneğin, veritabanı kullanılabilirliğini denetleme).</span><span class="sxs-lookup"><span data-stu-id="16cb7-277">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="16cb7-278">Bir istek bir sistem durumu denetimi uç noktasıyla eşleşiyorsa Terminal.</span><span class="sxs-lookup"><span data-stu-id="16cb7-278">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="16cb7-279">HTTP yöntemini geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="16cb7-279">HTTP Method Override</span></span>](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | <span data-ttu-id="16cb7-280">Gelen POST isteğinin yöntemi geçersiz kılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-280">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="16cb7-281">Güncelleştirilmiş yöntemini kullanan bileşenlerden önce.</span><span class="sxs-lookup"><span data-stu-id="16cb7-281">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="16cb7-282">HTTPS yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="16cb7-282">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="16cb7-283">Tüm HTTP isteklerini HTTPS 'ye yeniden yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="16cb7-283">Redirect all HTTP requests to HTTPS.</span></span> | <span data-ttu-id="16cb7-284">URL 'YI kullanan bileşenlerden önce.</span><span class="sxs-lookup"><span data-stu-id="16cb7-284">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="16cb7-285">HTTP katı taşıma güvenliği (HSTS)</span><span class="sxs-lookup"><span data-stu-id="16cb7-285">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="16cb7-286">Özel bir yanıt üst bilgisi ekleyen güvenlik geliştirme ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="16cb7-286">Security enhancement middleware that adds a special response header.</span></span> | <span data-ttu-id="16cb7-287">Yanıtlar gönderilmeden önce ve istekleri değiştiren bileşenler.</span><span class="sxs-lookup"><span data-stu-id="16cb7-287">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="16cb7-288">Örnekler: Iletilen üstbilgiler, URL yeniden yazma.</span><span class="sxs-lookup"><span data-stu-id="16cb7-288">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="16cb7-289">MVC</span><span class="sxs-lookup"><span data-stu-id="16cb7-289">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="16cb7-290">MVC/Razor Pages ile istekleri işler.</span><span class="sxs-lookup"><span data-stu-id="16cb7-290">Processes requests with MVC/Razor Pages.</span></span> | <span data-ttu-id="16cb7-291">Bir istek bir rota ile eşleşiyorsa Terminal.</span><span class="sxs-lookup"><span data-stu-id="16cb7-291">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="16cb7-292">OWıN</span><span class="sxs-lookup"><span data-stu-id="16cb7-292">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="16cb7-293">OWIN tabanlı uygulamalar, sunucular ve ara yazılım ile birlikte çalışma.</span><span class="sxs-lookup"><span data-stu-id="16cb7-293">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="16cb7-294">OWıN ara yazılımı isteği tam olarak işliyorsa Terminal.</span><span class="sxs-lookup"><span data-stu-id="16cb7-294">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="16cb7-295">Yanıtları Önbelleğe Alma</span><span class="sxs-lookup"><span data-stu-id="16cb7-295">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="16cb7-296">Yanıtları önbelleğe almak için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-296">Provides support for caching responses.</span></span> | <span data-ttu-id="16cb7-297">Önbelleğe alma gerektiren bileşenlerden önce.</span><span class="sxs-lookup"><span data-stu-id="16cb7-297">Before components that require caching.</span></span> |
| [<span data-ttu-id="16cb7-298">Yanıt sıkıştırması</span><span class="sxs-lookup"><span data-stu-id="16cb7-298">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="16cb7-299">Yanıtları sıkıştırmak için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-299">Provides support for compressing responses.</span></span> | <span data-ttu-id="16cb7-300">Sıkıştırma gerektiren bileşenlerden önce.</span><span class="sxs-lookup"><span data-stu-id="16cb7-300">Before components that require compression.</span></span> |
| [<span data-ttu-id="16cb7-301">Yerelleştirme iste</span><span class="sxs-lookup"><span data-stu-id="16cb7-301">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="16cb7-302">Yerelleştirme desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-302">Provides localization support.</span></span> | <span data-ttu-id="16cb7-303">Yerelleştirmenin önemli bileşenlerinden önce.</span><span class="sxs-lookup"><span data-stu-id="16cb7-303">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="16cb7-304">Uç nokta yönlendirme</span><span class="sxs-lookup"><span data-stu-id="16cb7-304">Endpoint Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="16cb7-305">İstek yollarını tanımlar ve kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-305">Defines and constrains request routes.</span></span> | <span data-ttu-id="16cb7-306">Eşleşen yolların terminali.</span><span class="sxs-lookup"><span data-stu-id="16cb7-306">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="16cb7-307">Oturum</span><span class="sxs-lookup"><span data-stu-id="16cb7-307">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="16cb7-308">Kullanıcı oturumlarını yönetmek için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-308">Provides support for managing user sessions.</span></span> | <span data-ttu-id="16cb7-309">Oturum gerektiren bileşenlerden önce.</span><span class="sxs-lookup"><span data-stu-id="16cb7-309">Before components that require Session.</span></span> |
| [<span data-ttu-id="16cb7-310">Statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="16cb7-310">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="16cb7-311">Statik dosyaları ve dizin taramayı sunma desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-311">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="16cb7-312">Bir istek bir dosyayla eşleşiyorsa Terminal.</span><span class="sxs-lookup"><span data-stu-id="16cb7-312">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="16cb7-313">URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="16cb7-313">URL Rewrite</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="16cb7-314">URL 'Leri yeniden yazma ve istekleri yeniden yönlendirme desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="16cb7-314">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="16cb7-315">URL 'YI kullanan bileşenlerden önce.</span><span class="sxs-lookup"><span data-stu-id="16cb7-315">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="16cb7-316">WebSockets</span><span class="sxs-lookup"><span data-stu-id="16cb7-316">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="16cb7-317">WebSockets protokolünü etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="16cb7-317">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="16cb7-318">WebSocket isteklerini kabul etmek için gereken bileşenlerden önce.</span><span class="sxs-lookup"><span data-stu-id="16cb7-318">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="16cb7-319">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="16cb7-319">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
