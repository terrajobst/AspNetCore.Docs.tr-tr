---
title: URL yeniden yazma ara yazılımı ASP.NET core'da
author: guardrex
description: URL yeniden yazma ve URL yeniden yazma ara yazılımı ile ASP.NET Core uygulamalarında yeniden yönlendirme hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 11/19/2018
uid: fundamentals/url-rewriting
ms.openlocfilehash: 84052789717738a48c346d35d1a2642017a9ab93
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861920"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="6b35f-103">URL yeniden yazma ara yazılımı ASP.NET core'da</span><span class="sxs-lookup"><span data-stu-id="6b35f-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="6b35f-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="6b35f-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="6b35f-105">Bu konuda 1.1 sürümü için indirme [URL yeniden yazma ara yazılımı ASP.NET core'da (sürüm 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/URL_Rewriting_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="6b35f-105">For the 1.1 version of this topic, download [URL Rewriting Middleware in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/URL_Rewriting_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="6b35f-106">Bu belge, URL yeniden yazma URL yeniden yazma ara yazılımı ASP.NET Core uygulamalarında kullanma hakkında yönergeler sunar.</span><span class="sxs-lookup"><span data-stu-id="6b35f-106">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

<span data-ttu-id="6b35f-107">URL yeniden yazma URL bir veya daha fazla önceden tanımlanmış kurallara göre istek değiştirme işlemidir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-107">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="6b35f-108">Böylece konumları ve adresleri sıkı şekilde bağlı olmayan URL yeniden yazma kaynak konumları ve adresleri arasında bir Özet oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6b35f-108">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses aren't tightly linked.</span></span> <span data-ttu-id="6b35f-109">URL yeniden yazma için çeşitli senaryolarda kullanışlıdır:</span><span class="sxs-lookup"><span data-stu-id="6b35f-109">URL rewriting is valuable in several scenarios to:</span></span>

* <span data-ttu-id="6b35f-110">Taşıyın veya sunucu kaynaklarına geçici veya kalıcı olarak değiştirin ve korumak için bu kaynakları kararlı bulucular.</span><span class="sxs-lookup"><span data-stu-id="6b35f-110">Move or replace server resources temporarily or permanently and maintain stable locators for those resources.</span></span>
* <span data-ttu-id="6b35f-111">İstek işleme bir uygulamanın alanları arasında veya farklı uygulamalar arasında bölün.</span><span class="sxs-lookup"><span data-stu-id="6b35f-111">Split request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="6b35f-112">Kaldır, ekleme veya URL kesimleri gelen isteklerden yeniden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="6b35f-112">Remove, add, or reorganize URL segments on incoming requests.</span></span>
* <span data-ttu-id="6b35f-113">Genel URL, arama motoru iyileştirmesi (SEO) iyileştirin.</span><span class="sxs-lookup"><span data-stu-id="6b35f-113">Optimize public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="6b35f-114">Bir kaynak isteyerek döndürülen içerik tahmin edebilmesi için ortak kolay URL'leri kullanımına izin verir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-114">Permit the use of friendly public URLs to help visitors predict the content returned by requesting a resource.</span></span>
* <span data-ttu-id="6b35f-115">Uç noktalarını güvenli hale getirmek için güvenli istekleri yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="6b35f-115">Redirect insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="6b35f-116">Burada dış bir siteye barındırılan bir statik varlık başka bir siteye kendi içeriğinize varlık bağlayarak kullanır, hotlinking engelleyin.</span><span class="sxs-lookup"><span data-stu-id="6b35f-116">Prevent hotlinking, where an external site uses a hosted static asset on another site by linking the asset into its own content.</span></span>

> [!NOTE]
> <span data-ttu-id="6b35f-117">URL yeniden yazma uygulama performansını düşürebilir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-117">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="6b35f-118">Uygun olduğunda, sayısı ve karmaşıklığı kurallar sınırlayın.</span><span class="sxs-lookup"><span data-stu-id="6b35f-118">Where feasible, limit the number and complexity of rules.</span></span>

<span data-ttu-id="6b35f-119">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6b35f-119">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="6b35f-120">URL yeniden yönlendirme ve URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="6b35f-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="6b35f-121">İfade arasındaki farkı *URL yeniden yönlendirme* ve *URL yeniden yazma* inceliklidir ancak istemcilere kaynakları sağlamak için önemli etkilere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-121">The difference in wording between *URL redirect* and *URL rewrite* is subtle but has important implications for providing resources to clients.</span></span> <span data-ttu-id="6b35f-122">ASP.NET Core'nın URL yeniden yazma ara yazılımı her ikisi de gereksinimini toplantı yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="6b35f-123">A *URL yeniden yönlendirme* istemci burada belirtildiği başlangıçta istenen istemci farklı bir adresten bir kaynağa erişmek için bir istemci tarafı işlemi içerir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-123">A *URL redirect* involves a client-side operation, where the client is instructed to access a resource at a different address than the client originally requested.</span></span> <span data-ttu-id="6b35f-124">Bu sunucuya gidiş dönüş gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-124">This requires a round trip to the server.</span></span> <span data-ttu-id="6b35f-125">İstemci kaynak için yeni bir istekte bulunduğunda istemciye döndürülen yeniden yönlendirme URL'sini tarayıcınızın adres çubuğunda görünür.</span><span class="sxs-lookup"><span data-stu-id="6b35f-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span>

<span data-ttu-id="6b35f-126">Varsa `/resource` olduğu *yeniden yönlendirilen* için `/different-resource`, sunucu istemcinin kaynakta edinmelidir yanıt verir. `/different-resource` yeniden yönlendirme geçici veya kalıcı olduğunu gösteren bir durum kodu ile.</span><span class="sxs-lookup"><span data-stu-id="6b35f-126">If `/resource` is *redirected* to `/different-resource`, the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span>

![Webapı hizmet uç noktası sunucusundaki sürüm 2 (v2) 1 (v1) sürümünden geçici olarak değiştirildi.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="6b35f-132">İstekleri için farklı bir URL yeniden yönlendirme, yeniden yönlendirme ile yanıt durum kodunu belirterek kalıcı veya geçici olup olmadığını gösterir:</span><span class="sxs-lookup"><span data-stu-id="6b35f-132">When redirecting requests to a different URL, indicate whether the redirect is permanent or temporary by specifying the status code with the response:</span></span>

* <span data-ttu-id="6b35f-133">*301 - kalıcı olarak taşındı* durum kodu kullanılan burada yeni, kalıcı bir URL kaynağı vardır ve istediğiniz istemci gelecekteki tüm istekler kaynak için yeni URL kullanması gerektiğini bildirin.</span><span class="sxs-lookup"><span data-stu-id="6b35f-133">The *301 - Moved Permanently* status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="6b35f-134">*İstemci önbellek ve 301 durum kodu alındığında yanıt yeniden.*</span><span class="sxs-lookup"><span data-stu-id="6b35f-134">*The client may cache and reuse the response when a 301 status code is received.*</span></span>

* <span data-ttu-id="6b35f-135">*302 bulundu -* durum kodu yeniden yönlendirme geçici olduğu veya değiştirilebilir genel olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6b35f-135">The *302 - Found* status code is used where the redirection is temporary or generally subject to change.</span></span> <span data-ttu-id="6b35f-136">302 durum kodu olmayan mağaza URL'sini ve gelecekte kullanmak üzere istemciye gösterir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-136">The 302 status code indicates to the client not to store the URL and use it in the future.</span></span>

<span data-ttu-id="6b35f-137">Durum kodları hakkında daha fazla bilgi için bkz. [RFC 2616: durum kodu tanımları](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="6b35f-137">For more information on status codes, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="6b35f-138">A *URL yeniden yazma* istenen istemci farklı bir kaynak adresten bir kaynak sağlayan bir sunucu tarafı işlemdir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-138">A *URL rewrite* is a server-side operation that provides a resource from a different resource address than the client requested.</span></span> <span data-ttu-id="6b35f-139">Bir URL yeniden yazma sunucuya gidiş dönüş gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="6b35f-139">Rewriting a URL doesn't require a round trip to the server.</span></span> <span data-ttu-id="6b35f-140">Yeniden URL istemciye döndürülen değildir ve tarayıcınızın adres çubuğuna görünmez.</span><span class="sxs-lookup"><span data-stu-id="6b35f-140">The rewritten URL isn't returned to the client and doesn't appear in the browser's address bar.</span></span>

<span data-ttu-id="6b35f-141">Varsa `/resource` olduğu *yazılan* için `/different-resource`, sunucunun *dahili olarak* getirir ve kaynağı döndürür `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-141">If `/resource` is *rewritten* to `/different-resource`, the server *internally* fetches and returns the resource at `/different-resource`.</span></span>

<span data-ttu-id="6b35f-142">İstemci yeniden URL'deki kaynak alınamadı olabilir, ancak istemci, isteği yapar ve yanıtı alır, kaynağın yeniden URL'de mevcut haberdar değildir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-142">Although the client might be able to retrieve the resource at the rewritten URL, the client isn't informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Webapı hizmet uç noktası, sunucu üzerindeki sürüm 2 (v2) için 1 (v1) sürümünden değiştirildi.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="6b35f-147">URL yeniden yazma örnek uygulaması</span><span class="sxs-lookup"><span data-stu-id="6b35f-147">URL rewriting sample app</span></span>

<span data-ttu-id="6b35f-148">URL yeniden yazma ara yazılımı ile özelliklerini inceleyebilirsiniz [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span><span class="sxs-lookup"><span data-stu-id="6b35f-148">You can explore the features of the URL Rewriting Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="6b35f-149">Uygulama yeniden yönlendirme uygulanır ve yeniden yazma kuralları ve çeşitli senaryolar için yeniden yönlendirilen veya yeniden URL gösterir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-149">The app applies redirect and rewrite rules and shows the redirected or rewritten URL for several scenarios.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="6b35f-150">URL yeniden yazma ara yazılımı kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="6b35f-150">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="6b35f-151">URL yeniden yazma ara yazılımı, aşağıdaki yaklaşımlardan kullanamaz olduğunuzda kullanın:</span><span class="sxs-lookup"><span data-stu-id="6b35f-151">Use URL Rewriting Middleware when you're unable to use the following approaches:</span></span>

* [<span data-ttu-id="6b35f-152">URL yeniden yazma modülü ile Windows Server'da IIS</span><span class="sxs-lookup"><span data-stu-id="6b35f-152">URL Rewrite module with IIS on Windows Server</span></span>](https://www.iis.net/downloads/microsoft/url-rewrite)
* [<span data-ttu-id="6b35f-153">Apache sunucuda Apache mod_rewrite Modülü</span><span class="sxs-lookup"><span data-stu-id="6b35f-153">Apache mod_rewrite module on Apache Server</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="6b35f-154">Ngınx üzerinde yeniden yazma URL'si</span><span class="sxs-lookup"><span data-stu-id="6b35f-154">URL rewriting on Nginx</span></span>](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

<span data-ttu-id="6b35f-155">Uygulama üzerinde barındırılıyorsa, ayrıca, kullanılacak Ara yazılımları kullanmayı [HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıyla [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="6b35f-155">Also, use the middleware when the app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

<span data-ttu-id="6b35f-156">IIS, Apache, Nginx teknolojileri yeniden sunucu tabanlı URL'sini kullanmak üzere temel neden şunlardır:</span><span class="sxs-lookup"><span data-stu-id="6b35f-156">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, and Nginx are:</span></span>

* <span data-ttu-id="6b35f-157">Ara yazılım, bu modüllerin tam özelliklerini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="6b35f-157">The middleware doesn't support the full features of these modules.</span></span>

  <span data-ttu-id="6b35f-158">Sunucu modüllerinin özelliklerinden bazıları gibi ASP.NET Core projeleriyle çalışmıyor `IsFile` ve `IsDirectory` IIS yeniden yazma modülü kısıtlamaları.</span><span class="sxs-lookup"><span data-stu-id="6b35f-158">Some of the features of the server modules don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="6b35f-159">Bu senaryolarda, ara yazılım kullanın.</span><span class="sxs-lookup"><span data-stu-id="6b35f-159">In these scenarios, use the middleware instead.</span></span>
* <span data-ttu-id="6b35f-160">Ara yazılım performansını büyük olasılıkla, modül ile eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="6b35f-160">The performance of the middleware probably doesn't match that of the modules.</span></span>

  <span data-ttu-id="6b35f-161">Değerlendirmesi emin olmak için hangi yaklaşımın en iyi performansı düşürür veya performans düzeyi düşürülmüş göz ardı edilebilir ise bilmek tek yoludur.</span><span class="sxs-lookup"><span data-stu-id="6b35f-161">Benchmarking is the only way to know for sure which approach degrades performance the most or if degraded performance is negligible.</span></span>

## <a name="package"></a><span data-ttu-id="6b35f-162">Paket</span><span class="sxs-lookup"><span data-stu-id="6b35f-162">Package</span></span>

<span data-ttu-id="6b35f-163">Ara yazılım projenize eklemek için paket başvurusu ekleme [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) proje dosyasında içeren [Microsoft.AspNetCore.Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) paket.</span><span class="sxs-lookup"><span data-stu-id="6b35f-163">To include the middleware in your project, add a package reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the project file, which contains the [Microsoft.AspNetCore.Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) package.</span></span>

<span data-ttu-id="6b35f-164">Değil kullanırken `Microsoft.AspNetCore.App` metapackage, bir proje başvurusu Ekle `Microsoft.AspNetCore.Rewrite` paket.</span><span class="sxs-lookup"><span data-stu-id="6b35f-164">When not using the `Microsoft.AspNetCore.App` metapackage, add a project reference to the `Microsoft.AspNetCore.Rewrite` package.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="6b35f-165">Uzantı ve seçenekleri</span><span class="sxs-lookup"><span data-stu-id="6b35f-165">Extension and options</span></span>

<span data-ttu-id="6b35f-166">URL yeniden yazma oluşturmak ve bir örneğini oluşturarak kuralları yeniden yönlendirme [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) sınıfı her biri, yeniden yazma kuralları için genişletme yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="6b35f-166">Establish URL rewrite and redirect rules by creating an instance of the [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) class with extension methods for each of your rewrite rules.</span></span> <span data-ttu-id="6b35f-167">İşlenen bunları istediğiniz sırayla birden çok kural zincir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-167">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="6b35f-168">`RewriteOptions` İle istek ardışık düzenine eklenen URL yeniden yazma ara yazılımı geçirilen <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>:</span><span class="sxs-lookup"><span data-stu-id="6b35f-168">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

### <a name="redirect-non-www-to-www"></a><span data-ttu-id="6b35f-169">Www www olmayan yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="6b35f-169">Redirect non-www to www</span></span>

<span data-ttu-id="6b35f-170">Üç seçenek olmayan yönlendirmek için uygulama izin`www` ister `www`:</span><span class="sxs-lookup"><span data-stu-id="6b35f-170">Three options permit the app to redirect non-`www` requests to `www`:</span></span>

* <span data-ttu-id="6b35f-171"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*> &ndash; İstek için kalıcı yeniden yönlendirme `www` istek olmayan ise alt etki alanı`www`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-171"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*> &ndash; Permanently redirect the request to the `www` subdomain if the request is non-`www`.</span></span> <span data-ttu-id="6b35f-172">İle yeniden yönlendiren bir [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) durum kodu.</span><span class="sxs-lookup"><span data-stu-id="6b35f-172">Redirects with a [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) status code.</span></span>

* <span data-ttu-id="6b35f-173"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*> &ndash; Yeniden yönlendirme isteği `www` gelen istek olmayan ise alt etki alanı`www`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-173"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*> &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="6b35f-174">İle yeniden yönlendiren bir [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) durum kodu.</span><span class="sxs-lookup"><span data-stu-id="6b35f-174">Redirects with a [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) status code.</span></span> <span data-ttu-id="6b35f-175">Aşırı yanıtı durum kodu sağlamanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-175">An overload permits you to provide the status code for the response.</span></span> <span data-ttu-id="6b35f-176">Bir alanı kullanan <xref:Microsoft.AspNetCore.Http.StatusCodes> bir durum kodu ataması için sınıf.</span><span class="sxs-lookup"><span data-stu-id="6b35f-176">Use a field of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for a status code assignment.</span></span>

### <a name="url-redirect"></a><span data-ttu-id="6b35f-177">URL yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="6b35f-177">URL redirect</span></span>

<span data-ttu-id="6b35f-178">Kullanım <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> isteklerini yeniden yönlendirmek için.</span><span class="sxs-lookup"><span data-stu-id="6b35f-178">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> to redirect requests.</span></span> <span data-ttu-id="6b35f-179">İlk parametre, normal ifade gelen URL yolunda eşleşen içerir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-179">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="6b35f-180">İkinci değiştirme dizesi parametresidir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-180">The second parameter is the replacement string.</span></span> <span data-ttu-id="6b35f-181">Üçüncü parametre varsa, durum kodu belirtir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-181">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="6b35f-182">Durum kodu durum kodu belirtmezseniz varsayılan *302 bulundu -*, kaynak geçici olarak taşındı veya değiştirilinceye olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-182">If you don't specify the status code, the status code defaults to *302 - Found*, which indicates that the resource is temporarily moved or replaced.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=9)]

<span data-ttu-id="6b35f-183">Geliştirici Araçları etkin bir tarayıcıda yolu kullanarak örnek uygulamaya istekte bulunmak `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-183">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="6b35f-184">Normal ifade üzerinde istek yolunun eşleşmesi `redirect-rule/(.*)`, ve yolu ile değiştirilir `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-184">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="6b35f-185">Yeniden yönlendirme URL'si ile istemciye gönderilen bir *302 bulundu -* durum kodu.</span><span class="sxs-lookup"><span data-stu-id="6b35f-185">The redirect URL is sent back to the client with a *302 - Found* status code.</span></span> <span data-ttu-id="6b35f-186">Tarayıcı, tarayıcının adres çubuğunda görüntülenen yeniden yönlendirme URL'sindeki yeni bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-186">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="6b35f-187">Yeniden yönlendirme URL'sini örnek uygulama eşleşmeye hiçbir kural itibaren:</span><span class="sxs-lookup"><span data-stu-id="6b35f-187">Since no rules in the sample app match on the redirect URL:</span></span>

* <span data-ttu-id="6b35f-188">İkinci isteği aldığında bir *200 - OK* uygulama yanıtı.</span><span class="sxs-lookup"><span data-stu-id="6b35f-188">The second request receives a *200 - OK* response from the app.</span></span>
* <span data-ttu-id="6b35f-189">Yanıt gövdesinin yeniden yönlendirme URL'sini gösterir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-189">The body of the response shows the redirect URL.</span></span>

<span data-ttu-id="6b35f-190">Bir gidiş dönüş yapılan sunucuya bir URL olduğunda *yeniden yönlendirilen*.</span><span class="sxs-lookup"><span data-stu-id="6b35f-190">A round trip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="6b35f-191">Yeniden yönlendirme kuralları oluştururken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="6b35f-191">Be cautious when establishing redirect rules.</span></span> <span data-ttu-id="6b35f-192">Yeniden yönlendirme kuralları, sonra bir yeniden yönlendirme de dahil olmak üzere uygulama her istekte değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-192">Redirect rules are evaluated on every request to the app, including after a redirect.</span></span> <span data-ttu-id="6b35f-193">Yanlışlıkla oluşturmak kolaydır bir *döngü sonsuz yeniden yönlendirmeleri,*.</span><span class="sxs-lookup"><span data-stu-id="6b35f-193">It's easy to accidentally create a *loop of infinite redirects*.</span></span>

<span data-ttu-id="6b35f-194">Özgün istek: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="6b35f-194">Original Request: `/redirect-rule/1234/5678`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="6b35f-196">Parantez içinde yer alan bir ifade parçası olarak adlandırılan bir *yakalama grubu*.</span><span class="sxs-lookup"><span data-stu-id="6b35f-196">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="6b35f-197">Nokta (`.`) ifade anlamına gelir *herhangi bir karakterle eşleştir*.</span><span class="sxs-lookup"><span data-stu-id="6b35f-197">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="6b35f-198">Yıldız işareti (`*`) gösteren *önceki karakteri sıfır veya daha fazla kez eşleştir*.</span><span class="sxs-lookup"><span data-stu-id="6b35f-198">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="6b35f-199">Bu nedenle, son iki yol kesimleri URL'sinin `1234/5678`, yakalama grubu tarafından yakalanan `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-199">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="6b35f-200">Sonra istek URL'sindeki sağladığınız herhangi bir değer `redirect-rule/` bu tek bir yakalama grubu tarafından yakalanır.</span><span class="sxs-lookup"><span data-stu-id="6b35f-200">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="6b35f-201">Değiştirme dizesinde yakalanan gruplar dolar işareti ile dizesine eklenen (`$`) yakalama dizisi sayı takip eder.</span><span class="sxs-lookup"><span data-stu-id="6b35f-201">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="6b35f-202">İlk yakalama grubu değer ile elde edilen `$1`ikinci ile `$2`, ve bunların sırası, normal ifade yakalama grupları için devam edin.</span><span class="sxs-lookup"><span data-stu-id="6b35f-202">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="6b35f-203">Var. yalnızca bir yakalanan gruba içinde yeniden yönlendirme kuralı regex örnek uygulamada, olan değiştirme dizesinde tek eklenen grubu için `$1`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-203">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="6b35f-204">Kural uygulandığında, URL olur `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-204">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="6b35f-205">Güvenli bir uç noktası için URL yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="6b35f-205">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="6b35f-206">Kullanım <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> aynı konak ve yol HTTPS protokolünü kullanarak HTTP isteklerini yeniden yönlendirmek için.</span><span class="sxs-lookup"><span data-stu-id="6b35f-206">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> to redirect HTTP requests to the same host and path using the HTTPS protocol.</span></span> <span data-ttu-id="6b35f-207">Durum kodu verilmiyorsa, ara yazılım için varsayılan olarak *302 bulundu -*.</span><span class="sxs-lookup"><span data-stu-id="6b35f-207">If the status code isn't supplied, the middleware defaults to *302 - Found*.</span></span> <span data-ttu-id="6b35f-208">Bağlantı noktası verilmiyorsa varsa:</span><span class="sxs-lookup"><span data-stu-id="6b35f-208">If the port isn't supplied:</span></span>

* <span data-ttu-id="6b35f-209">Varsayılan olarak ara yazılım `null`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-209">The middleware defaults to `null`.</span></span>
* <span data-ttu-id="6b35f-210">Şema değişikliklerini `https` (HTTPS protokolü) ve istemci kaynak bağlantı noktası 443 üzerinden erişir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-210">The scheme changes to `https` (HTTPS protocol), and the client accesses the resource on port 443.</span></span>

<span data-ttu-id="6b35f-211">Aşağıdaki örnek, durum kodu ayarlamak için gösterilmektedir *301 - kalıcı olarak taşındı* ve 5001 için bağlantı noktasını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6b35f-211">The following example shows how to set the status code to *301 - Moved Permanently* and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="6b35f-212">Kullanım <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> aynı konak ve yol bağlantı noktası 443 üzerinden güvenli HTTPS protokolü ile güvenli isteklerini yeniden yönlendirmek için.</span><span class="sxs-lookup"><span data-stu-id="6b35f-212">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> to redirect insecure requests to the same host and path with secure HTTPS protocol on port 443.</span></span> <span data-ttu-id="6b35f-213">Ara yazılım durum kodunu ayarlar *301 - kalıcı olarak taşındı*.</span><span class="sxs-lookup"><span data-stu-id="6b35f-213">The middleware sets the status code to *301 - Moved Permanently*.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="6b35f-214">Ek yeniden yönlendirme kuralları gereksinimi olmadan güvenli bir uç noktasına yeniden yönlendirirken, HTTPS yeniden yönlendirmesi ara yazılım kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="6b35f-214">When redirecting to a secure endpoint without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="6b35f-215">Daha fazla bilgi için [HTTPS zorlama](xref:security/enforcing-ssl#require-https) konu.</span><span class="sxs-lookup"><span data-stu-id="6b35f-215">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="6b35f-216">Örnek uygulamayı nasıl kullanılacağını gösteren özellikli `AddRedirectToHttps` veya `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-216">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="6b35f-217">Uzantı yöntemine ekleyin `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-217">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="6b35f-218">Güvenli olmayan bir istek, herhangi bir URL konumundaki uygulamaya olun.</span><span class="sxs-lookup"><span data-stu-id="6b35f-218">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="6b35f-219">Tarayıcı güvenlik otomatik olarak imzalanan sertifika güvenilmeyen uyarısını kapatmak veya sertifikaya güvenmek için bir özel durum oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6b35f-219">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="6b35f-220">Özgün istek kullanarak `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="6b35f-220">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="6b35f-222">Özgün istek kullanarak `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="6b35f-222">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="6b35f-224">URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="6b35f-224">URL rewrite</span></span>

<span data-ttu-id="6b35f-225">Kullanım <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> URL yeniden yazma için bir kural oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="6b35f-225">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> to create a rule for rewriting URLs.</span></span> <span data-ttu-id="6b35f-226">İlk parametre gelen URL yolu temel eşleştirme için normal ifade içeriyor.</span><span class="sxs-lookup"><span data-stu-id="6b35f-226">The first parameter contains the regex for matching on the incoming URL path.</span></span> <span data-ttu-id="6b35f-227">İkinci değiştirme dizesi parametresidir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-227">The second parameter is the replacement string.</span></span> <span data-ttu-id="6b35f-228">Üçüncü parametreyi `skipRemainingRules: {true|false}`, ara yazılımı için geçerli kural uyguladıysanız, ek bir yeniden yazma kuralları atlamak gerekip gerekmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-228">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=10-11)]

<span data-ttu-id="6b35f-229">Özgün istek: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="6b35f-229">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıt izleme](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="6b35f-231">Simgeyi seçtiğinizde (`^`) URL yolunun başında, eşleşen ifade anlamına gelir. başlangıçtan başlar.</span><span class="sxs-lookup"><span data-stu-id="6b35f-231">The carat (`^`) at the beginning of the expression means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="6b35f-232">Yeniden yönlendirme kuralı ile bir önceki örnekte `redirect-rule/(.*)`, hiçbir ayar yoktur (`^`) normal ifade başlangıcı.</span><span class="sxs-lookup"><span data-stu-id="6b35f-232">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat (`^`) at the start of the regex.</span></span> <span data-ttu-id="6b35f-233">Bu nedenle, herhangi bir karakter önünde `redirect-rule/` yolunda başarılı bir eşleşme.</span><span class="sxs-lookup"><span data-stu-id="6b35f-233">Therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="6b35f-234">Yol</span><span class="sxs-lookup"><span data-stu-id="6b35f-234">Path</span></span>                               | <span data-ttu-id="6b35f-235">Eşleştirme</span><span class="sxs-lookup"><span data-stu-id="6b35f-235">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="6b35f-236">Evet</span><span class="sxs-lookup"><span data-stu-id="6b35f-236">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="6b35f-237">Evet</span><span class="sxs-lookup"><span data-stu-id="6b35f-237">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="6b35f-238">Evet</span><span class="sxs-lookup"><span data-stu-id="6b35f-238">Yes</span></span>   |

<span data-ttu-id="6b35f-239">Yeniden üretme kuralı `^rewrite-rule/(\d+)/(\d+)`, ile başlatırsanız, yalnızca yollarıyla eşleşen `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-239">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="6b35f-240">Aşağıdaki tabloda, eşleşen içinde farka dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6b35f-240">In the following table, note the difference in matching.</span></span>

| <span data-ttu-id="6b35f-241">Yol</span><span class="sxs-lookup"><span data-stu-id="6b35f-241">Path</span></span>                              | <span data-ttu-id="6b35f-242">Eşleştirme</span><span class="sxs-lookup"><span data-stu-id="6b35f-242">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="6b35f-243">Evet</span><span class="sxs-lookup"><span data-stu-id="6b35f-243">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="6b35f-244">Hayır</span><span class="sxs-lookup"><span data-stu-id="6b35f-244">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="6b35f-245">Hayır</span><span class="sxs-lookup"><span data-stu-id="6b35f-245">No</span></span>    |

<span data-ttu-id="6b35f-246">Aşağıdaki `^rewrite-rule/` bölümü ifadesi, iki yakalama grupları vardır `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-246">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="6b35f-247">`\d` Gösterir *bir basamağı (sayı) eşleşen*.</span><span class="sxs-lookup"><span data-stu-id="6b35f-247">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="6b35f-248">Artı işaretini (`+`) anlamına gelir *bir veya daha önceki karakter eşleşen*.</span><span class="sxs-lookup"><span data-stu-id="6b35f-248">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="6b35f-249">Bu nedenle, URL, başka bir sayının İleri-eğik çizgi ardından bir sayı içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-249">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="6b35f-250">Bu yakalama grupları yeniden URL eklenmiş `$1` ve `$2`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-250">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="6b35f-251">Yeniden yazma kuralı değiştirme dizesi, sorgu dizesinde yakalanan gruplar yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-251">The rewrite rule replacement string places the captured groups into the query string.</span></span> <span data-ttu-id="6b35f-252">İstenen yolunu `/rewrite-rule/1234/5678` kaynağı almak için yazılan `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-252">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="6b35f-253">Özgün istek üzerinde bir sorgu dizesi varsa, URL'yi yeniden zaman korunur.</span><span class="sxs-lookup"><span data-stu-id="6b35f-253">If a query string is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="6b35f-254">Hiçbir kaynak almak için sunucuya gidiş dönüş yoktur.</span><span class="sxs-lookup"><span data-stu-id="6b35f-254">There's no round trip to the server to obtain the resource.</span></span> <span data-ttu-id="6b35f-255">Kaynağın mevcut durumunda getirilen ve istemciye döndürülen bir *200 - OK* durum kodu.</span><span class="sxs-lookup"><span data-stu-id="6b35f-255">If the resource exists, it's fetched and returned to the client with a *200 - OK* status code.</span></span> <span data-ttu-id="6b35f-256">Tarayıcının adres çubuğuna URL'yi, istemci yeniden yönlendirilen değildir çünkü değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="6b35f-256">Because the client isn't redirected, the URL in the browser's address bar doesn't change.</span></span> <span data-ttu-id="6b35f-257">İstemciler, bir URL yeniden yazma işlemi sunucuda oluştu algılayamaz.</span><span class="sxs-lookup"><span data-stu-id="6b35f-257">Clients can't detect that a URL rewrite operation occurred on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="6b35f-258">Kullanım `skipRemainingRules: true` her olası kurallarına çünkü hesaplama açısından pahalıdır ve uygulama yanıt süresini artırır.</span><span class="sxs-lookup"><span data-stu-id="6b35f-258">Use `skipRemainingRules: true` whenever possible because matching rules is computationally expensive and increases app response time.</span></span> <span data-ttu-id="6b35f-259">Hızlı Uygulama yanıtı:</span><span class="sxs-lookup"><span data-stu-id="6b35f-259">For the fastest app response:</span></span>
>
> * <span data-ttu-id="6b35f-260">Sipariş en sık eşleşen kural kurallardan az sık eşleşen kural yeniden yazın.</span><span class="sxs-lookup"><span data-stu-id="6b35f-260">Order rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="6b35f-261">Bir eşleşme olursa ve hiçbir ek kural işleme gerekli olduğunda kalan kurallarının işlenmesini atlayın.</span><span class="sxs-lookup"><span data-stu-id="6b35f-261">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="6b35f-262">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="6b35f-262">Apache mod_rewrite</span></span>

<span data-ttu-id="6b35f-263">Apache mod_rewrite kurallarıyla uygulamak <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*>.</span><span class="sxs-lookup"><span data-stu-id="6b35f-263">Apply Apache mod_rewrite rules with <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*>.</span></span> <span data-ttu-id="6b35f-264">Kurallar dosyası uygulamayla dağıtıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="6b35f-264">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="6b35f-265">Daha fazla bilgi ve mod_rewrite kuralları örnekleri için bkz. [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="6b35f-265">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

<span data-ttu-id="6b35f-266">A <xref:System.IO.StreamReader> kurallardan okumak için kullanılan *ApacheModRewrite.txt* kurallar dosyası:</span><span class="sxs-lookup"><span data-stu-id="6b35f-266">A <xref:System.IO.StreamReader> is used to read the rules from the *ApacheModRewrite.txt* rules file:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=3-4,12)]

<span data-ttu-id="6b35f-267">Örnek uygulama isteklerinden yönlendiren `/apache-mod-rules-redirect/(.\*)` için `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-267">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="6b35f-268">Yanıt durum kodu *302 bulundu -*.</span><span class="sxs-lookup"><span data-stu-id="6b35f-268">The response status code is *302 - Found*.</span></span>

[!code[](url-rewriting/samples/2.x/SampleApp/ApacheModRewrite.txt)]

<span data-ttu-id="6b35f-269">Özgün istek: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="6b35f-269">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_apache_mod_redirect.png)

<span data-ttu-id="6b35f-271">Ara yazılım, aşağıdaki Apache mod_rewrite sunucu değişkenlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="6b35f-271">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="6b35f-272">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="6b35f-272">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="6b35f-273">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="6b35f-273">HTTP_ACCEPT</span></span>
* <span data-ttu-id="6b35f-274">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="6b35f-274">HTTP_CONNECTION</span></span>
* <span data-ttu-id="6b35f-275">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="6b35f-275">HTTP_COOKIE</span></span>
* <span data-ttu-id="6b35f-276">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="6b35f-276">HTTP_FORWARDED</span></span>
* <span data-ttu-id="6b35f-277">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="6b35f-277">HTTP_HOST</span></span>
* <span data-ttu-id="6b35f-278">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="6b35f-278">HTTP_REFERER</span></span>
* <span data-ttu-id="6b35f-279">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="6b35f-279">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="6b35f-280">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6b35f-280">HTTPS</span></span>
* <span data-ttu-id="6b35f-281">IPV6</span><span class="sxs-lookup"><span data-stu-id="6b35f-281">IPV6</span></span>
* <span data-ttu-id="6b35f-282">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="6b35f-282">QUERY_STRING</span></span>
* <span data-ttu-id="6b35f-283">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="6b35f-283">REMOTE_ADDR</span></span>
* <span data-ttu-id="6b35f-284">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="6b35f-284">REMOTE_PORT</span></span>
* <span data-ttu-id="6b35f-285">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="6b35f-285">REQUEST_FILENAME</span></span>
* <span data-ttu-id="6b35f-286">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="6b35f-286">REQUEST_METHOD</span></span>
* <span data-ttu-id="6b35f-287">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="6b35f-287">REQUEST_SCHEME</span></span>
* <span data-ttu-id="6b35f-288">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="6b35f-288">REQUEST_URI</span></span>
* <span data-ttu-id="6b35f-289">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="6b35f-289">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="6b35f-290">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="6b35f-290">SERVER_ADDR</span></span>
* <span data-ttu-id="6b35f-291">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="6b35f-291">SERVER_PORT</span></span>
* <span data-ttu-id="6b35f-292">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="6b35f-292">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="6b35f-293">SAAT</span><span class="sxs-lookup"><span data-stu-id="6b35f-293">TIME</span></span>
* <span data-ttu-id="6b35f-294">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="6b35f-294">TIME_DAY</span></span>
* <span data-ttu-id="6b35f-295">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="6b35f-295">TIME_HOUR</span></span>
* <span data-ttu-id="6b35f-296">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="6b35f-296">TIME_MIN</span></span>
* <span data-ttu-id="6b35f-297">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="6b35f-297">TIME_MON</span></span>
* <span data-ttu-id="6b35f-298">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="6b35f-298">TIME_SEC</span></span>
* <span data-ttu-id="6b35f-299">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="6b35f-299">TIME_WDAY</span></span>
* <span data-ttu-id="6b35f-300">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="6b35f-300">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="6b35f-301">IIS URL yeniden yazma modülü kuralları</span><span class="sxs-lookup"><span data-stu-id="6b35f-301">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="6b35f-302">IIS URL yeniden yazma modülü uygulanan aynı kural kümesi kullanmak için <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>.</span><span class="sxs-lookup"><span data-stu-id="6b35f-302">To use the same rule set that applies to the IIS URL Rewrite Module, use <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>.</span></span> <span data-ttu-id="6b35f-303">Kurallar dosyası uygulamayla dağıtıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="6b35f-303">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="6b35f-304">Uygulamanın kullanılacak Ara yazılımının doğrudan olmayan *web.config* dosya Windows Server IIS üzerinde çalışırken.</span><span class="sxs-lookup"><span data-stu-id="6b35f-304">Don't direct the middleware to use the app's *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="6b35f-305">Uygulamanın dışında IIS ile bu kuralları depolanması gereken *web.config* IIS yeniden yazma modülü ile çakışmaları önlemek için dosya.</span><span class="sxs-lookup"><span data-stu-id="6b35f-305">With IIS, these rules should be stored outside of the app's *web.config* file in order to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="6b35f-306">Daha fazla bilgi ve IIS URL yeniden yazma modülü kuralları örnekleri için bkz. [Url yeniden yazma modülü 2.0 kullanarak](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) ve [URL yeniden yazma Module yapılandırma başvurusu](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="6b35f-306">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

<span data-ttu-id="6b35f-307">A <xref:System.IO.StreamReader> kurallardan okumak için kullanılan *IISUrlRewrite.xml* kurallar dosyası:</span><span class="sxs-lookup"><span data-stu-id="6b35f-307">A <xref:System.IO.StreamReader> is used to read the rules from the *IISUrlRewrite.xml* rules file:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5-6,13)]

<span data-ttu-id="6b35f-308">Örnek uygulama, gelen istekleri yeniden yazar `/iis-rules-rewrite/(.*)` için `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-308">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="6b35f-309">Yanıtı istemciye gönderilen bir *200 - OK* durum kodu.</span><span class="sxs-lookup"><span data-stu-id="6b35f-309">The response is sent to the client with a *200 - OK* status code.</span></span>

[!code-xml[](url-rewriting/samples/2.x/SampleApp/IISUrlRewrite.xml)]

<span data-ttu-id="6b35f-310">Özgün istek: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="6b35f-310">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıt izleme](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="6b35f-312">Etkin bir IIS yeniden yazma modülü ile uygulamanızı istenmeyen yollarla erişememeleri yapılandırılmış sunucu düzeyinde kurallar varsa, IIS yeniden yazma modülü bir uygulama için devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6b35f-312">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="6b35f-313">Daha fazla bilgi için [devre dışı bırakma IIS modüllerini](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="6b35f-313">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="6b35f-314">Desteklenmeyen özellikler</span><span class="sxs-lookup"><span data-stu-id="6b35f-314">Unsupported features</span></span>

<span data-ttu-id="6b35f-315">Yayımlanan bir ara yazılım ile ASP.NET Core 2.x, aşağıdaki IIS URL yeniden yazma modülü özellikleri desteklemez:</span><span class="sxs-lookup"><span data-stu-id="6b35f-315">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="6b35f-316">Giden kuralları</span><span class="sxs-lookup"><span data-stu-id="6b35f-316">Outbound Rules</span></span>
* <span data-ttu-id="6b35f-317">Özel sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6b35f-317">Custom Server Variables</span></span>
* <span data-ttu-id="6b35f-318">Joker karakterler</span><span class="sxs-lookup"><span data-stu-id="6b35f-318">Wildcards</span></span>
* <span data-ttu-id="6b35f-319">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="6b35f-319">LogRewrittenUrl</span></span>

#### <a name="supported-server-variables"></a><span data-ttu-id="6b35f-320">Desteklenen sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6b35f-320">Supported server variables</span></span>

<span data-ttu-id="6b35f-321">Ara yazılım, aşağıdaki IIS URL yeniden yazma modülü sunucu değişkenleri destekler:</span><span class="sxs-lookup"><span data-stu-id="6b35f-321">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="6b35f-322">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="6b35f-322">CONTENT_LENGTH</span></span>
* <span data-ttu-id="6b35f-323">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="6b35f-323">CONTENT_TYPE</span></span>
* <span data-ttu-id="6b35f-324">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="6b35f-324">HTTP_ACCEPT</span></span>
* <span data-ttu-id="6b35f-325">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="6b35f-325">HTTP_CONNECTION</span></span>
* <span data-ttu-id="6b35f-326">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="6b35f-326">HTTP_COOKIE</span></span>
* <span data-ttu-id="6b35f-327">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="6b35f-327">HTTP_HOST</span></span>
* <span data-ttu-id="6b35f-328">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="6b35f-328">HTTP_REFERER</span></span>
* <span data-ttu-id="6b35f-329">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="6b35f-329">HTTP_URL</span></span>
* <span data-ttu-id="6b35f-330">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="6b35f-330">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="6b35f-331">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6b35f-331">HTTPS</span></span>
* <span data-ttu-id="6b35f-332">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="6b35f-332">LOCAL_ADDR</span></span>
* <span data-ttu-id="6b35f-333">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="6b35f-333">QUERY_STRING</span></span>
* <span data-ttu-id="6b35f-334">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="6b35f-334">REMOTE_ADDR</span></span>
* <span data-ttu-id="6b35f-335">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="6b35f-335">REMOTE_PORT</span></span>
* <span data-ttu-id="6b35f-336">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="6b35f-336">REQUEST_FILENAME</span></span>
* <span data-ttu-id="6b35f-337">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="6b35f-337">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="6b35f-338">De edinebilirsiniz bir <xref:Microsoft.Extensions.FileProviders.IFileProvider> aracılığıyla bir <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="6b35f-338">You can also obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> via a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>.</span></span> <span data-ttu-id="6b35f-339">Bu yaklaşım, yeniden yazma konumu için daha fazla esneklik kuralları dosyaları sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-339">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="6b35f-340">Sağladığınız yolun sunucusuna yeniden yazma kuralları dosyalarınızı dağıtıldığından emin emin olun.</span><span class="sxs-lookup"><span data-stu-id="6b35f-340">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="6b35f-341">Metot tabanlı kuralı</span><span class="sxs-lookup"><span data-stu-id="6b35f-341">Method-based rule</span></span>

<span data-ttu-id="6b35f-342">Kullanım <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> bir yöntemde kendi kural mantığı uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="6b35f-342">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> to implement your own rule logic in a method.</span></span> <span data-ttu-id="6b35f-343">`Add` kullanıma sunan <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>, hangi kullanımınıza <xref:Microsoft.AspNetCore.Http.HttpContext> yönteminiz olarak kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="6b35f-343">`Add` exposes the <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>, which makes available the <xref:Microsoft.AspNetCore.Http.HttpContext> for use in your method.</span></span> <span data-ttu-id="6b35f-344">[RewriteContext.Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*) nasıl ek işlem hattı belirler işleme gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-344">The [RewriteContext.Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*) determines how additional pipeline processing is handled.</span></span> <span data-ttu-id="6b35f-345">Değer birine ayarlayın <xref:Microsoft.AspNetCore.Rewrite.RuleResult> aşağıdaki tabloda açıklanan alanları.</span><span class="sxs-lookup"><span data-stu-id="6b35f-345">Set the value to one of the <xref:Microsoft.AspNetCore.Rewrite.RuleResult> fields described in the following table.</span></span>

| `RewriteContext.Result`              | <span data-ttu-id="6b35f-346">Eylem</span><span class="sxs-lookup"><span data-stu-id="6b35f-346">Action</span></span>                                                           |
| ------------------------------------ | ---------------------------------------------------------------- |
| <span data-ttu-id="6b35f-347">`RuleResult.ContinueRules` (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="6b35f-347">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="6b35f-348">Kurallarını uygulama devam edin.</span><span class="sxs-lookup"><span data-stu-id="6b35f-348">Continue applying rules.</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="6b35f-349">Kuralları uygulamak durdurun ve bir yanıt gönderir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-349">Stop applying rules and send the response.</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="6b35f-350">Kuralları uygulamak durdurun ve bağlam için bir sonraki ara yazılım gönderin.</span><span class="sxs-lookup"><span data-stu-id="6b35f-350">Stop applying rules and send the context to the next middleware.</span></span> |

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=14)]

<span data-ttu-id="6b35f-351">Örnek uygulama ile biten istekleri yolları için yönlendiren bir yöntemi gösterir *.xml*.</span><span class="sxs-lookup"><span data-stu-id="6b35f-351">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="6b35f-352">Bir istek yaptıysanız `/file.xml`, istek yönlendireceği `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-352">If a request is made for `/file.xml`, the request is redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="6b35f-353">Durum kodu kümesine *301 - kalıcı olarak taşındı*.</span><span class="sxs-lookup"><span data-stu-id="6b35f-353">The status code is set to *301 - Moved Permanently*.</span></span> <span data-ttu-id="6b35f-354">Ne zaman tarayıcı yapar yeni bir istek */xmlfiles/file.xml*, statik dosya ara yazılımlarını dosya istemciden hizmet *wwwroot/xmlfiles* klasör.</span><span class="sxs-lookup"><span data-stu-id="6b35f-354">When the browser makes a new request for */xmlfiles/file.xml*, Static File Middleware serves the file to the client from the *wwwroot/xmlfiles* folder.</span></span> <span data-ttu-id="6b35f-355">İçin bir yeniden yönlendirme, yanıtın durum kodu açıkça ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="6b35f-355">For a redirect, explicitly set the status code of the response.</span></span> <span data-ttu-id="6b35f-356">Aksi takdirde, bir *200 - OK* durum kodu döndürülür ve istemcide yeniden yönlendirme gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="6b35f-356">Otherwise, a *200 - OK* status code is returned, and the redirect doesn't occur on the client.</span></span>

<span data-ttu-id="6b35f-357">*RewriteRules.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b35f-357">*RewriteRules.cs*:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectXmlFileRequests&highlight=14-18)]

<span data-ttu-id="6b35f-358">Bu yaklaşım, istekleri de yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6b35f-358">This approach can also rewrite requests.</span></span> <span data-ttu-id="6b35f-359">Örnek uygulama sunmak herhangi bir metin dosyası talep için yol yeniden yazma gösterir *dosya.txt* metin dosyasından *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="6b35f-359">The sample app demonstrates rewriting the path for any text file request to serve the *file.txt* text file from the *wwwroot* folder.</span></span> <span data-ttu-id="6b35f-360">Statik dosya ara yazılımlarını dosyanın güncelleştirilmiş isteği yola göre hizmet eder:</span><span class="sxs-lookup"><span data-stu-id="6b35f-360">Static File Middleware serves the file based on the updated request path:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=15,22)]

<span data-ttu-id="6b35f-361">*RewriteRules.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b35f-361">*RewriteRules.cs*:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RewriteTextFileRequests&highlight=7-8)]

### <a name="irule-based-rule"></a><span data-ttu-id="6b35f-362">Kural Irule tabanlı</span><span class="sxs-lookup"><span data-stu-id="6b35f-362">IRule-based rule</span></span>

<span data-ttu-id="6b35f-363">Kullanma <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> kuralı mantığını uygulayan bir sınıf içinde kullanılacak <xref:Microsoft.AspNetCore.Rewrite.IRule> arabirimi.</span><span class="sxs-lookup"><span data-stu-id="6b35f-363">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> to use rule logic in a class that implements the <xref:Microsoft.AspNetCore.Rewrite.IRule> interface.</span></span> <span data-ttu-id="6b35f-364">`IRule` metot tabanlı kural yaklaşımı kullanarak üzerinden daha fazla esneklik sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b35f-364">`IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="6b35f-365">Uygulama sınıfınız için parametreleri geçirebilirsiniz izin veren bir oluşturucu içerebilir <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6b35f-365">Your implementation class may include a constructor that allows you can pass in parameters for the <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> method.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="6b35f-366">Örnek uygulama için parametre değerlerini `extension` ve `newPath` çeşitli koşullara uyması için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="6b35f-366">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="6b35f-367">`extension` Bir değer içermelidir ve değer olmalıdır *.png*, *.jpg*, veya *.gif*.</span><span class="sxs-lookup"><span data-stu-id="6b35f-367">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="6b35f-368">Varsa `newPath` geçerli olmayan bir <xref:System.ArgumentException> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6b35f-368">If the `newPath` isn't valid, an <xref:System.ArgumentException> is thrown.</span></span> <span data-ttu-id="6b35f-369">Bir istek yaptıysanız *image.png*, istek yönlendireceği `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-369">If a request is made for *image.png*, the request is redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="6b35f-370">Bir istek yaptıysanız *image.jpg*, istek yönlendireceği `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="6b35f-370">If a request is made for *image.jpg*, the request is redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="6b35f-371">Durum kodu kümesine *301 - kalıcı olarak taşındı*ve `context.Result` kural işlemeyi durdur ve yanıt göndermek için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="6b35f-371">The status code is set to *301 - Moved Permanently*, and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectImageRequests)]

<span data-ttu-id="6b35f-372">Özgün istek: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="6b35f-372">Original Request: `/image.png`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların image.png için izleme ile](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="6b35f-374">Özgün istek: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="6b35f-374">Original Request: `/image.jpg`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların image.jpg için izleme ile](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="6b35f-376">Normal ifade örnekleri</span><span class="sxs-lookup"><span data-stu-id="6b35f-376">Regex examples</span></span>

| <span data-ttu-id="6b35f-377">Hedef</span><span class="sxs-lookup"><span data-stu-id="6b35f-377">Goal</span></span> | <span data-ttu-id="6b35f-378">Normal ifade dizesini &</span><span class="sxs-lookup"><span data-stu-id="6b35f-378">Regex String &</span></span><br><span data-ttu-id="6b35f-379">Eşleşme örneği</span><span class="sxs-lookup"><span data-stu-id="6b35f-379">Match Example</span></span> | <span data-ttu-id="6b35f-380">Değiştirme dizesi &</span><span class="sxs-lookup"><span data-stu-id="6b35f-380">Replacement String &</span></span><br><span data-ttu-id="6b35f-381">Çıkış örneği</span><span class="sxs-lookup"><span data-stu-id="6b35f-381">Output Example</span></span> |
| ---- | ------------------------------- | -------------------------------------- |
| <span data-ttu-id="6b35f-382">Querystring yol yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="6b35f-382">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="6b35f-383">Şerit sonunda eğik çizgi</span><span class="sxs-lookup"><span data-stu-id="6b35f-383">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="6b35f-384">Eğik zorla</span><span class="sxs-lookup"><span data-stu-id="6b35f-384">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="6b35f-385">Belirli isteklere yeniden yazma kaçının</span><span class="sxs-lookup"><span data-stu-id="6b35f-385">Avoid rewriting specific requests</span></span> | <span data-ttu-id="6b35f-386">`^(.*)(?<!\.axd)$` veya `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="6b35f-386">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="6b35f-387">Evet: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="6b35f-387">Yes: `/resource.htm`</span></span><br><span data-ttu-id="6b35f-388">Hayır: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="6b35f-388">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="6b35f-389">URL kesimleri bunları yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="6b35f-389">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="6b35f-390">URL kesimi değiştirin</span><span class="sxs-lookup"><span data-stu-id="6b35f-390">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="6b35f-391">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6b35f-391">Additional resources</span></span>

* [<span data-ttu-id="6b35f-392">Uygulama Başlatma</span><span class="sxs-lookup"><span data-stu-id="6b35f-392">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="6b35f-393">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="6b35f-393">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="6b35f-394">.NET içinde normal ifadeler</span><span class="sxs-lookup"><span data-stu-id="6b35f-394">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="6b35f-395">Normal ifade dili - hızlı başvuru</span><span class="sxs-lookup"><span data-stu-id="6b35f-395">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="6b35f-396">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="6b35f-396">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="6b35f-397">URL yeniden yazma modülü 2.0 (IIS) kullanma</span><span class="sxs-lookup"><span data-stu-id="6b35f-397">Using Url Rewrite Module 2.0 (for IIS)</span></span>](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="6b35f-398">URL yeniden yazma Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="6b35f-398">URL Rewrite Module Configuration Reference</span></span>](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="6b35f-399">IIS URL yeniden yazma modülü Forumu</span><span class="sxs-lookup"><span data-stu-id="6b35f-399">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="6b35f-400">Basit bir URL yapısı tutun</span><span class="sxs-lookup"><span data-stu-id="6b35f-400">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="6b35f-401">10 URL yeniden yazma, ipuçları ve püf noktaları</span><span class="sxs-lookup"><span data-stu-id="6b35f-401">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="6b35f-402">Eğik çizgi veya değil eğik çizgi</span><span class="sxs-lookup"><span data-stu-id="6b35f-402">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
