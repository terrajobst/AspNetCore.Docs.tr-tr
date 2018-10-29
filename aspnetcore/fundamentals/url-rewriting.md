---
title: URL yeniden yazma ara yazılımı ASP.NET core'da
author: guardrex
description: URL yeniden yazma ve URL yeniden yazma ara yazılımı ile ASP.NET Core uygulamalarında yeniden yönlendirme hakkında bilgi edinin.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: 5a1891c838436467fb49ff6288587fab08201179
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207192"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="66bf5-103">URL yeniden yazma ara yazılımı ASP.NET core'da</span><span class="sxs-lookup"><span data-stu-id="66bf5-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="66bf5-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="66bf5-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="66bf5-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="66bf5-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="66bf5-106">URL yeniden yazma URL bir veya daha fazla önceden tanımlanmış kurallara göre istek değiştirme işlemidir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="66bf5-107">Böylece konumları ve adresleri sıkı şekilde bağlı olmayan URL yeniden yazma kaynak konumları ve adresleri arasında bir Özet oluşturur.</span><span class="sxs-lookup"><span data-stu-id="66bf5-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="66bf5-108">URL yeniden yazma yararlı olduğu bazı senaryolar vardır:</span><span class="sxs-lookup"><span data-stu-id="66bf5-108">There are several scenarios where URL rewriting is valuable:</span></span>

* <span data-ttu-id="66bf5-109">Taşıma veya bu kaynaklar için kararlı bulucular korurken sunucu kaynaklarını geçici veya kalıcı olarak değiştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="66bf5-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources.</span></span>
* <span data-ttu-id="66bf5-110">İstek farklı uygulamalar arasında veya tek bir uygulama alanları genelinde işleme bölme.</span><span class="sxs-lookup"><span data-stu-id="66bf5-110">Splitting request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="66bf5-111">Kaldırma, ekleme veya URL kesimleri gelen isteklerden yeniden düzenleme.</span><span class="sxs-lookup"><span data-stu-id="66bf5-111">Removing, adding, or reorganizing URL segments on incoming requests.</span></span>
* <span data-ttu-id="66bf5-112">En iyi duruma getirme Genel URL'ler arama motoru iyileştirmesi (SEO).</span><span class="sxs-lookup"><span data-stu-id="66bf5-112">Optimizing public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="66bf5-113">Bir bağlantıyı izleyerek bulabilirsiniz içeriği tahmin kolaylaştıracak kolay genel URL kullanımını erişimine izin verme.</span><span class="sxs-lookup"><span data-stu-id="66bf5-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link.</span></span>
* <span data-ttu-id="66bf5-114">Uç noktalarını güvenli hale getirmek için güvenli istekleri yeniden yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="66bf5-114">Redirecting insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="66bf5-115">Görüntü hotlinking engelliyor.</span><span class="sxs-lookup"><span data-stu-id="66bf5-115">Preventing image hotlinking.</span></span>

<span data-ttu-id="66bf5-116">URL çeşitli şekillerde değiştirme, Regex, Apache mod_rewrite modülü kuralları, IIS yeniden yazma modülü kuralları da dahil olmak üzere ve özel kural mantığı kullanarak kurallar tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66bf5-116">You can define rules for changing the URL in several ways, including Regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="66bf5-117">Bu belge, URL yeniden yazma URL yeniden yazma ara yazılımı ASP.NET Core uygulamalarında kullanma hakkında yönergeler sunar.</span><span class="sxs-lookup"><span data-stu-id="66bf5-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="66bf5-118">URL yeniden yazma uygulama performansını düşürebilir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="66bf5-119">Uygun olduğunda, sayısı ve karmaşıklığı kurallar sınırlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="66bf5-120">URL yeniden yönlendirme ve URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="66bf5-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="66bf5-121">İfade arasındaki farkı *URL yeniden yönlendirme* ve *URL yeniden yazma* en küçük görünebilir ancak ilk kaynakları istemcilere sağlamak için önemli etkileri vardır.</span><span class="sxs-lookup"><span data-stu-id="66bf5-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="66bf5-122">ASP.NET Core'nın URL yeniden yazma ara yazılımı her ikisi de gereksinimini toplantı yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="66bf5-123">A *URL yeniden yönlendirme* istemci burada belirtildiği başka bir adresten bir kaynağa erişmek için bir istemci tarafı işlemi olduğundan.</span><span class="sxs-lookup"><span data-stu-id="66bf5-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="66bf5-124">Bu sunucu için bir gidiş dönüş gerektirir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="66bf5-125">İstemci kaynak için yeni bir istekte bulunduğunda istemciye döndürülen yeniden yönlendirme URL'sini tarayıcınızın adres çubuğunda görünür.</span><span class="sxs-lookup"><span data-stu-id="66bf5-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="66bf5-126">Varsa `/resource` olduğu *yeniden yönlendirilen* için `/different-resource`, istemci isteklerini `/resource`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="66bf5-127">İstemci kaynak edinmelidir sunucunun yanıt `/different-resource` yeniden yönlendirme geçici veya kalıcı olduğunu gösteren bir durum kodu ile.</span><span class="sxs-lookup"><span data-stu-id="66bf5-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="66bf5-128">İstemci yeniden yönlendirme URL'sini kaynak için yeni bir isteği yürütür.</span><span class="sxs-lookup"><span data-stu-id="66bf5-128">The client executes a new request for the resource at the redirect URL.</span></span>

![Webapı hizmet uç noktası sunucusundaki sürüm 2 (v2) 1 (v1) sürümünden geçici olarak değiştirildi.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="66bf5-134">İstekleri için farklı bir URL yeniden yönlendirme, yeniden yönlendirme kalıcı veya geçici olup olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="66bf5-135">301 (kalıcı taşındı) durum kodunu burada yeni, kalıcı bir URL kaynağı vardır ve istediğiniz tüm gelecek istekleri kaynak için yeni URL kullanması gerektiğini istemci istemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="66bf5-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="66bf5-136">*301 durum kodu alındığında istemci yanıtı önbelleğe alabilir.*</span><span class="sxs-lookup"><span data-stu-id="66bf5-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="66bf5-137">(Bulunamadı) durum kodunu 302 yeniden yönlendirme geçici veya genel konu olduğu istemci depolamak olmamalıdır ve gelecekte yeniden yönlendirme URL'sini yeniden şekilde değiştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="66bf5-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="66bf5-138">Daha fazla bilgi için [RFC 2616: durum kodu tanımları](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="66bf5-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="66bf5-139">A *URL yeniden yazma* farklı bir kaynak adresten bir kaynak sağlamak için bir sunucu tarafı işlemi.</span><span class="sxs-lookup"><span data-stu-id="66bf5-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="66bf5-140">Bir URL yeniden yazma sunucuya gidiş dönüşlü hale getirmek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="66bf5-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="66bf5-141">Yeniden URL istemciye döndürülen değildir ve tarayıcının adres çubuğunda görüntülenmez.</span><span class="sxs-lookup"><span data-stu-id="66bf5-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="66bf5-142">Zaman `/resource` olduğu *yazılan* için `/different-resource`, istemci isteklerini `/resource`ve sunucu *dahili olarak* kaynakta getirir `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="66bf5-143">İstemci yeniden URL'deki kaynak alınamadı olabilir, ancak istemci, isteği yapar ve yanıtı alır, kaynağın yeniden URL'de mevcut haberdar olmaz.</span><span class="sxs-lookup"><span data-stu-id="66bf5-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Webapı hizmet uç noktası, sunucu üzerindeki sürüm 2 (v2) için 1 (v1) sürümünden değiştirildi.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="66bf5-148">URL yeniden yazma örnek uygulaması</span><span class="sxs-lookup"><span data-stu-id="66bf5-148">URL rewriting sample app</span></span>

<span data-ttu-id="66bf5-149">URL yeniden yazma ara yazılımı ile özelliklerini inceleyebilirsiniz [URL yeniden yazma örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span><span class="sxs-lookup"><span data-stu-id="66bf5-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="66bf5-150">Uygulamaya yeniden uygular ve yeniden yönlendirme kuralları ve yeniden ya da yeniden yönlendirilen URL'sini gösterir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="66bf5-151">URL yeniden yazma ara yazılımı kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="66bf5-151">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="66bf5-152">URL yeniden yazma ara yazılımı kullanamaz olduğunda kullanın [URL yeniden yazma Modülü](https://www.iis.net/downloads/microsoft/url-rewrite) Windows Server, IIS ile [Apache mod_rewrite Modülü](https://httpd.apache.org/docs/2.4/rewrite/) Apache sunucusundaki [NgınxüzerindeURLyenidenyazma](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), veya uygulamanız üzerinde barındırılan [HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıyla [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="66bf5-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="66bf5-153">Sunucu tabanlı URL yeniden yazma teknolojileri IIS, Apache veya Ngınx kullanmak için ana ara yazılım, bu modüllerin tam özelliklerini desteklemiyor ve ara yazılım performansını büyük olasılıkla, modül eşleşmeyecektir nedenleridir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="66bf5-154">Ancak, ASP.NET Core projeleriyle gibi çalışmayan sunucu modülü bazı özellikler mevcuttur `IsFile` ve `IsDirectory` IIS yeniden yazma modülü kısıtlamaları.</span><span class="sxs-lookup"><span data-stu-id="66bf5-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="66bf5-155">Bu senaryolarda, ara yazılım kullanın.</span><span class="sxs-lookup"><span data-stu-id="66bf5-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="66bf5-156">Paket</span><span class="sxs-lookup"><span data-stu-id="66bf5-156">Package</span></span>

<span data-ttu-id="66bf5-157">Ara yazılım projenize eklemek için bir başvuru ekleyin. [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) paket.</span><span class="sxs-lookup"><span data-stu-id="66bf5-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="66bf5-158">Bu özellik, ASP.NET Core 1.1 hedefleyen uygulamalar için veya sonraki kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="66bf5-159">Uzantı ve seçenekleri</span><span class="sxs-lookup"><span data-stu-id="66bf5-159">Extension and options</span></span>

<span data-ttu-id="66bf5-160">URL yeniden yazma oluşturmak ve bir örneğini oluşturarak kuralları yeniden yönlendirme [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) sınıfı, kuralların her biri için genişletme yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="66bf5-160">Establish your URL rewrite and redirect rules by creating an instance of the [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) class with extension methods for each of your rules.</span></span> <span data-ttu-id="66bf5-161">İşlenen bunları istediğiniz sırayla birden çok kural zincir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="66bf5-162">`RewriteOptions` İle istek ardışık düzenine eklenen URL yeniden yazma ara yazılımı geçirilen `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="redirect-non-www-to-www"></a><span data-ttu-id="66bf5-163">Www www olmayan yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="66bf5-163">Redirect non-www to www</span></span>

<span data-ttu-id="66bf5-164">Üç seçenek olmayan yönlendirmek için uygulama izin`www` ister `www`:</span><span class="sxs-lookup"><span data-stu-id="66bf5-164">Three options permit the app to redirect non-`www` requests to `www`:</span></span>

* <span data-ttu-id="66bf5-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; kalıcı olarak isteği yönlendirmek `www` istek olmayan ise alt etki alanı`www`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; Permanently redirect the request to the `www` subdomain if the request is non-`www`.</span></span> <span data-ttu-id="66bf5-166">İle yeniden yönlendiren bir [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) durum kodu.</span><span class="sxs-lookup"><span data-stu-id="66bf5-166">Redirects with a [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) status code.</span></span>
* <span data-ttu-id="66bf5-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; isteği yönlendirmek `www` gelen istek olmayan ise alt etki alanı`www`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="66bf5-168">İle yeniden yönlendiren bir [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) durum kodu.</span><span class="sxs-lookup"><span data-stu-id="66bf5-168">Redirects with a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) status code.</span></span>
* <span data-ttu-id="66bf5-169">[AddRedirectToWww (RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; isteği yönlendirmek `www` gelen istek olmayan ise alt etki alanı`www`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-169">[AddRedirectToWww(RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="66bf5-170">Yanıt durum kodu girmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="66bf5-170">Allows you to provide the status code for the response.</span></span> <span data-ttu-id="66bf5-171">Alanlarını kullanın [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) sınıfı atamaların `AddRedirectToWww`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-171">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `AddRedirectToWww`.</span></span>

::: moniker-end

### <a name="url-redirect"></a><span data-ttu-id="66bf5-172">URL yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="66bf5-172">URL redirect</span></span>

<span data-ttu-id="66bf5-173">Kullanım `AddRedirect` isteklerini yeniden yönlendirmek için.</span><span class="sxs-lookup"><span data-stu-id="66bf5-173">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="66bf5-174">İlk parametre, normal ifade gelen URL yolunda eşleşen içerir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-174">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="66bf5-175">İkinci değiştirme dizesi parametresidir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-175">The second parameter is the replacement string.</span></span> <span data-ttu-id="66bf5-176">Üçüncü parametre varsa, durum kodu belirtir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-176">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="66bf5-177">Durum kodu belirtmezseniz, kaynak geçici olarak taşındı veya değiştirilinceye olduğunu gösteren 302 (bulunamadı) için varsayılanları.</span><span class="sxs-lookup"><span data-stu-id="66bf5-177">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="66bf5-178">Geliştirici Araçları etkin bir tarayıcıda yolu kullanarak örnek uygulamaya istekte bulunmak `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-178">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="66bf5-179">Normal ifade üzerinde istek yolunun eşleşmesi `redirect-rule/(.*)`, ve yolu ile değiştirilir `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-179">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="66bf5-180">Yeniden yönlendirme URL'si istemcisine bir 302 (bulunamadı) durum kodu ile gönderilir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-180">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="66bf5-181">Tarayıcı, tarayıcının adres çubuğunda görüntülenen yeniden yönlendirme URL'sindeki yeni bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-181">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="66bf5-182">Örnek uygulamada hiçbir kural üzerinde yeniden yönlendirme URL'si aynı olduğundan, ikinci isteği uygulamadan 200 (Tamam) yanıtı alır ve yanıt gövdesinin yeniden yönlendirme URL'sini gösterir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-182">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="66bf5-183">Bir URL ise bir gidiş dönüş sunucuya yapılan *yeniden yönlendirilen*.</span><span class="sxs-lookup"><span data-stu-id="66bf5-183">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="66bf5-184">Yeniden yönlendirme kurallarınızı oluştururken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="66bf5-184">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="66bf5-185">Yeniden yönlendirme kuralları, sonra bir yeniden yönlendirme de dahil olmak üzere uygulama, her istekte değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-185">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="66bf5-186">Yanlışlıkla için kolay bir sonsuz yeniden yönlendirmeleri döngüsünü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="66bf5-186">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="66bf5-187">Özgün istek: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="66bf5-187">Original Request: `/redirect-rule/1234/5678`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="66bf5-189">Parantez içinde yer alan bir ifade parçası olarak adlandırılan bir *yakalama grubu*.</span><span class="sxs-lookup"><span data-stu-id="66bf5-189">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="66bf5-190">Nokta (`.`) ifade anlamına gelir *herhangi bir karakterle eşleştir*.</span><span class="sxs-lookup"><span data-stu-id="66bf5-190">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="66bf5-191">Yıldız işareti (`*`) gösteren *önceki karakteri sıfır veya daha fazla kez eşleştir*.</span><span class="sxs-lookup"><span data-stu-id="66bf5-191">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="66bf5-192">Bu nedenle, son iki yol kesimleri URL'sinin `1234/5678`, yakalama grubu tarafından yakalanan `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-192">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="66bf5-193">Sonra istek URL'sindeki sağladığınız herhangi bir değer `redirect-rule/` bu tek bir yakalama grubu tarafından yakalanır.</span><span class="sxs-lookup"><span data-stu-id="66bf5-193">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="66bf5-194">Değiştirme dizesinde yakalanan gruplar dolar işareti ile dizesine eklenen (`$`) yakalama dizisi sayı takip eder.</span><span class="sxs-lookup"><span data-stu-id="66bf5-194">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="66bf5-195">İlk yakalama grubu değer ile elde edilen `$1`ikinci ile `$2`, ve bunların sırası, normal ifade yakalama grupları için devam edin.</span><span class="sxs-lookup"><span data-stu-id="66bf5-195">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="66bf5-196">Var. yalnızca bir yakalanan gruba içinde yeniden yönlendirme kuralı regex örnek uygulamada, olan değiştirme dizesinde tek eklenen grubu için `$1`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-196">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="66bf5-197">Kural uygulandığında, URL olur `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-197">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="66bf5-198">Güvenli bir uç noktası için URL yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="66bf5-198">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="66bf5-199">Kullanım `AddRedirectToHttps` aynı konak ve yol HTTPS kullanarak HTTP isteklerini yeniden yönlendirmek için (`https://`).</span><span class="sxs-lookup"><span data-stu-id="66bf5-199">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="66bf5-200">Durum kodu verilmiyorsa, ara yazılım 302 (bulunamadı) için varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="66bf5-200">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="66bf5-201">Bağlantı noktası verilmiyorsa, ara yazılım için varsayılan olarak `null`, Protokolü yani değişikliklerini `https://` ve istemci kaynak bağlantı noktası 443 üzerinden erişir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-201">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="66bf5-202">Örneğin, durum kodu 301 (kalıcı taşındı) ve bağlantı noktasını değiştirmek için 5001 gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-202">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="66bf5-203">Kullanım `AddRedirectToHttpsPermanent` aynı konak ve yol güvenli HTTPS protokolü ile güvenli isteklerini yeniden yönlendirmek için (`https://` bağlantı noktası 443 üzerinden).</span><span class="sxs-lookup"><span data-stu-id="66bf5-203">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="66bf5-204">Ara yazılım 301 (kalıcı taşındı) durum kodunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="66bf5-204">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="66bf5-205">HTTPS için ek yeniden yönlendirme kuralları gereksinimi olmadan yönlendirirken, HTTPS yeniden yönlendirmesi ara yazılım kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="66bf5-205">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="66bf5-206">Daha fazla bilgi için [HTTPS zorlama](xref:security/enforcing-ssl#require-https) konu.</span><span class="sxs-lookup"><span data-stu-id="66bf5-206">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="66bf5-207">Örnek uygulamayı nasıl kullanılacağını gösteren özellikli `AddRedirectToHttps` veya `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-207">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="66bf5-208">Uzantı yöntemine ekleyin `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-208">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="66bf5-209">Güvenli olmayan bir istek, herhangi bir URL konumundaki uygulamaya olun.</span><span class="sxs-lookup"><span data-stu-id="66bf5-209">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="66bf5-210">Tarayıcı güvenlik otomatik olarak imzalanan sertifika güvenilmeyen uyarısını kapatmak veya sertifikaya güvenmek için bir özel durum oluşturun.</span><span class="sxs-lookup"><span data-stu-id="66bf5-210">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="66bf5-211">Özgün istek kullanarak `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="66bf5-211">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="66bf5-213">Özgün istek kullanarak `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="66bf5-213">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="66bf5-215">URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="66bf5-215">URL rewrite</span></span>

<span data-ttu-id="66bf5-216">Kullanım `AddRewrite` URL yeniden yazma için bir kural oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="66bf5-216">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="66bf5-217">İlk parametre gelen URL yolu temel eşlemek için normal ifade içeriyor.</span><span class="sxs-lookup"><span data-stu-id="66bf5-217">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="66bf5-218">İkinci değiştirme dizesi parametresidir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-218">The second parameter is the replacement string.</span></span> <span data-ttu-id="66bf5-219">Üçüncü parametreyi `skipRemainingRules: {true|false}`, ara yazılımı için geçerli kural uyguladıysanız, ek bir yeniden yazma kuralları atlamak gerekip gerekmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-219">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="66bf5-220">Özgün istek: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="66bf5-220">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıt izleme](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="66bf5-222">Simgeyi seçtiğinizde normal ifade fark ilk şey olduğunu (`^`) ifadenin başında.</span><span class="sxs-lookup"><span data-stu-id="66bf5-222">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="66bf5-223">Başka bir deyişle, eşleşen bir URL yolu başlangıcında başlar.</span><span class="sxs-lookup"><span data-stu-id="66bf5-223">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="66bf5-224">Yeniden yönlendirme kuralı ile bir önceki örnekte `redirect-rule/(.*)`, normal ifade başlangıcında hiçbir ayar yoktur; bu nedenle, herhangi bir karakter önünde `redirect-rule/` yolunda başarılı bir eşleşme.</span><span class="sxs-lookup"><span data-stu-id="66bf5-224">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="66bf5-225">Yol</span><span class="sxs-lookup"><span data-stu-id="66bf5-225">Path</span></span>                               | <span data-ttu-id="66bf5-226">Eşleştirme</span><span class="sxs-lookup"><span data-stu-id="66bf5-226">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="66bf5-227">Evet</span><span class="sxs-lookup"><span data-stu-id="66bf5-227">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="66bf5-228">Evet</span><span class="sxs-lookup"><span data-stu-id="66bf5-228">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="66bf5-229">Evet</span><span class="sxs-lookup"><span data-stu-id="66bf5-229">Yes</span></span>   |

<span data-ttu-id="66bf5-230">Yeniden üretme kuralı `^rewrite-rule/(\d+)/(\d+)`, ile başlatırsanız, yalnızca yollarıyla eşleşen `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-230">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="66bf5-231">İçinde eşleşen aşağıdaki yeniden yazma kuralı ve yukarıdaki yeniden yönlendirme kuralı farka dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="66bf5-231">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="66bf5-232">Yol</span><span class="sxs-lookup"><span data-stu-id="66bf5-232">Path</span></span>                              | <span data-ttu-id="66bf5-233">Eşleştirme</span><span class="sxs-lookup"><span data-stu-id="66bf5-233">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="66bf5-234">Evet</span><span class="sxs-lookup"><span data-stu-id="66bf5-234">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="66bf5-235">Hayır</span><span class="sxs-lookup"><span data-stu-id="66bf5-235">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="66bf5-236">Hayır</span><span class="sxs-lookup"><span data-stu-id="66bf5-236">No</span></span>    |

<span data-ttu-id="66bf5-237">Aşağıdaki `^rewrite-rule/` bölümü ifadesi, iki yakalama grupları vardır `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-237">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="66bf5-238">`\d` Gösterir *bir basamağı (sayı) eşleşen*.</span><span class="sxs-lookup"><span data-stu-id="66bf5-238">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="66bf5-239">Artı işaretini (`+`) anlamına gelir *bir veya daha önceki karakter eşleşen*.</span><span class="sxs-lookup"><span data-stu-id="66bf5-239">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="66bf5-240">Bu nedenle, URL, başka bir sayının İleri-eğik çizgi ardından bir sayı içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-240">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="66bf5-241">Bu yakalama grupları yeniden URL eklenmiş `$1` ve `$2`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-241">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="66bf5-242">Yeniden yazma kuralı değiştirme dizesi yakalanan gruplar querystring yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-242">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="66bf5-243">İstenen yolunu `/rewrite-rule/1234/5678` kaynağı almak için yazılan `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-243">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="66bf5-244">Özgün istek üzerinde bir sorgu dizesi varsa, URL'yi yeniden zaman korunur.</span><span class="sxs-lookup"><span data-stu-id="66bf5-244">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="66bf5-245">Hiçbir geri dönüş kaynağı almak için sunucuya yoktur.</span><span class="sxs-lookup"><span data-stu-id="66bf5-245">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="66bf5-246">Kaynağın varolup olmadığını getirildi ve 200 (Tamam) durum kodu ile istemciye döndürülen.</span><span class="sxs-lookup"><span data-stu-id="66bf5-246">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="66bf5-247">Tarayıcı adres çubuğundaki URL'yi, istemci yeniden yönlendirilen değildir çünkü değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="66bf5-247">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="66bf5-248">İstemci endişelenmiştir kadar hiçbir zaman URL yeniden yazma işlemi oluştu.</span><span class="sxs-lookup"><span data-stu-id="66bf5-248">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="66bf5-249">Kullanım `skipRemainingRules: true` mümkün olduğunda, çünkü kurallarına pahalı bir işlemdir ve uygulama yanıt süresini azaltır.</span><span class="sxs-lookup"><span data-stu-id="66bf5-249">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="66bf5-250">Hızlı Uygulama yanıtı:</span><span class="sxs-lookup"><span data-stu-id="66bf5-250">For the fastest app response:</span></span>
> * <span data-ttu-id="66bf5-251">En sık eşleşen kural, yeniden yazma kuralları az sık eşleşen kural sipariş.</span><span class="sxs-lookup"><span data-stu-id="66bf5-251">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="66bf5-252">Bir eşleşme olursa ve hiçbir ek kural işleme gerekli olduğunda kalan kurallarının işlenmesini atlayın.</span><span class="sxs-lookup"><span data-stu-id="66bf5-252">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="66bf5-253">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="66bf5-253">Apache mod_rewrite</span></span>

<span data-ttu-id="66bf5-254">Apache mod_rewrite kurallarıyla uygulamak `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-254">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="66bf5-255">Kurallar dosyası uygulamayla dağıtıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="66bf5-255">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="66bf5-256">Daha fazla bilgi ve mod_rewrite kuralları örnekleri için bkz. [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="66bf5-256">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="66bf5-257">A `StreamReader` kurallardan okumak için kullanılan *ApacheModRewrite.txt* kurallar dosyası.</span><span class="sxs-lookup"><span data-stu-id="66bf5-257">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="66bf5-258">İlk parametre bir `IFileProvider`, aracılığıyla sağlanan [bağımlılık ekleme](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="66bf5-258">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="66bf5-259">`IHostingEnvironment` Sağlamak için eklenen `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-259">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="66bf5-260">İkinci parametre olan kurallar dosyası yolu olan *ApacheModRewrite.txt* örnek uygulamada.</span><span class="sxs-lookup"><span data-stu-id="66bf5-260">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="66bf5-261">Örnek uygulama isteklerinden yönlendiren `/apache-mod-rules-redirect/(.\*)` için `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-261">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="66bf5-262">Yanıt durum kodu 302 (bulunamadı) ' dir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-262">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="66bf5-263">Özgün istek: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="66bf5-263">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_apache_mod_redirect.png)

<span data-ttu-id="66bf5-265">Ara yazılım, aşağıdaki Apache mod_rewrite sunucu değişkenlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="66bf5-265">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="66bf5-266">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="66bf5-266">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="66bf5-267">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="66bf5-267">HTTP_ACCEPT</span></span>
* <span data-ttu-id="66bf5-268">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="66bf5-268">HTTP_CONNECTION</span></span>
* <span data-ttu-id="66bf5-269">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="66bf5-269">HTTP_COOKIE</span></span>
* <span data-ttu-id="66bf5-270">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="66bf5-270">HTTP_FORWARDED</span></span>
* <span data-ttu-id="66bf5-271">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="66bf5-271">HTTP_HOST</span></span>
* <span data-ttu-id="66bf5-272">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="66bf5-272">HTTP_REFERER</span></span>
* <span data-ttu-id="66bf5-273">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="66bf5-273">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="66bf5-274">HTTPS</span><span class="sxs-lookup"><span data-stu-id="66bf5-274">HTTPS</span></span>
* <span data-ttu-id="66bf5-275">IPV6</span><span class="sxs-lookup"><span data-stu-id="66bf5-275">IPV6</span></span>
* <span data-ttu-id="66bf5-276">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="66bf5-276">QUERY_STRING</span></span>
* <span data-ttu-id="66bf5-277">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="66bf5-277">REMOTE_ADDR</span></span>
* <span data-ttu-id="66bf5-278">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="66bf5-278">REMOTE_PORT</span></span>
* <span data-ttu-id="66bf5-279">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="66bf5-279">REQUEST_FILENAME</span></span>
* <span data-ttu-id="66bf5-280">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="66bf5-280">REQUEST_METHOD</span></span>
* <span data-ttu-id="66bf5-281">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="66bf5-281">REQUEST_SCHEME</span></span>
* <span data-ttu-id="66bf5-282">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="66bf5-282">REQUEST_URI</span></span>
* <span data-ttu-id="66bf5-283">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="66bf5-283">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="66bf5-284">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="66bf5-284">SERVER_ADDR</span></span>
* <span data-ttu-id="66bf5-285">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="66bf5-285">SERVER_PORT</span></span>
* <span data-ttu-id="66bf5-286">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="66bf5-286">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="66bf5-287">SAAT</span><span class="sxs-lookup"><span data-stu-id="66bf5-287">TIME</span></span>
* <span data-ttu-id="66bf5-288">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="66bf5-288">TIME_DAY</span></span>
* <span data-ttu-id="66bf5-289">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="66bf5-289">TIME_HOUR</span></span>
* <span data-ttu-id="66bf5-290">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="66bf5-290">TIME_MIN</span></span>
* <span data-ttu-id="66bf5-291">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="66bf5-291">TIME_MON</span></span>
* <span data-ttu-id="66bf5-292">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="66bf5-292">TIME_SEC</span></span>
* <span data-ttu-id="66bf5-293">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="66bf5-293">TIME_WDAY</span></span>
* <span data-ttu-id="66bf5-294">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="66bf5-294">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="66bf5-295">IIS URL yeniden yazma modülü kuralları</span><span class="sxs-lookup"><span data-stu-id="66bf5-295">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="66bf5-296">IIS URL yeniden yazma modülü uygulanan kurallarını kullanmak için `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-296">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="66bf5-297">Kurallar dosyası uygulamayla dağıtıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="66bf5-297">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="66bf5-298">Kullanılacak Ara yazılımının doğrudan yoksa, *web.config* dosya Windows Server IIS üzerinde çalışırken.</span><span class="sxs-lookup"><span data-stu-id="66bf5-298">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="66bf5-299">IIS ile bu kuralları dışında depolanması gereken, *web.config* IIS yeniden yazma modülü ile çakışmalarını önlemek için.</span><span class="sxs-lookup"><span data-stu-id="66bf5-299">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="66bf5-300">Daha fazla bilgi ve IIS URL yeniden yazma modülü kuralları örnekleri için bkz. [Url yeniden yazma modülü 2.0 kullanarak](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) ve [URL yeniden yazma Module yapılandırma başvurusu](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="66bf5-300">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="66bf5-301">A `StreamReader` kurallardan okumak için kullanılan *IISUrlRewrite.xml* kurallar dosyası.</span><span class="sxs-lookup"><span data-stu-id="66bf5-301">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="66bf5-302">İlk parametre bir `IFileProvider`, ikinci parametre olan, XML kuralları dosyası yolu olsa da *IISUrlRewrite.xml* örnek uygulamada.</span><span class="sxs-lookup"><span data-stu-id="66bf5-302">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="66bf5-303">Örnek uygulama, gelen istekleri yeniden yazar `/iis-rules-rewrite/(.*)` için `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-303">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="66bf5-304">İstemciye bir 200 (Tamam) durum koduyla yanıt gönderilir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-304">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="66bf5-305">Özgün istek: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="66bf5-305">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıt izleme](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="66bf5-307">Etkin bir IIS yeniden yazma modülü ile uygulamanızı istenmeyen yollarla erişememeleri yapılandırılmış sunucu düzeyinde kurallar varsa, IIS yeniden yazma modülü bir uygulama için devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66bf5-307">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="66bf5-308">Daha fazla bilgi için [devre dışı bırakma IIS modüllerini](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="66bf5-308">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="66bf5-309">Desteklenmeyen özellikler</span><span class="sxs-lookup"><span data-stu-id="66bf5-309">Unsupported features</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="66bf5-310">Yayımlanan bir ara yazılım ile ASP.NET Core 2.x, aşağıdaki IIS URL yeniden yazma modülü özellikleri desteklemez:</span><span class="sxs-lookup"><span data-stu-id="66bf5-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="66bf5-311">Giden kuralları</span><span class="sxs-lookup"><span data-stu-id="66bf5-311">Outbound Rules</span></span>
* <span data-ttu-id="66bf5-312">Özel sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="66bf5-312">Custom Server Variables</span></span>
* <span data-ttu-id="66bf5-313">Joker karakterler</span><span class="sxs-lookup"><span data-stu-id="66bf5-313">Wildcards</span></span>
* <span data-ttu-id="66bf5-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="66bf5-314">LogRewrittenUrl</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="66bf5-315">Yayımlanan bir ara yazılım ile ASP.NET Core 1.x aşağıdaki IIS URL yeniden yazma modülü özellikleri desteklemez:</span><span class="sxs-lookup"><span data-stu-id="66bf5-315">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="66bf5-316">Genel kurallar</span><span class="sxs-lookup"><span data-stu-id="66bf5-316">Global Rules</span></span>
* <span data-ttu-id="66bf5-317">Giden kuralları</span><span class="sxs-lookup"><span data-stu-id="66bf5-317">Outbound Rules</span></span>
* <span data-ttu-id="66bf5-318">Yeniden eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="66bf5-318">Rewrite Maps</span></span>
* <span data-ttu-id="66bf5-319">CustomResponse eylemi</span><span class="sxs-lookup"><span data-stu-id="66bf5-319">CustomResponse Action</span></span>
* <span data-ttu-id="66bf5-320">Özel sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="66bf5-320">Custom Server Variables</span></span>
* <span data-ttu-id="66bf5-321">Joker karakterler</span><span class="sxs-lookup"><span data-stu-id="66bf5-321">Wildcards</span></span>
* <span data-ttu-id="66bf5-322">Eylem: CustomResponse</span><span class="sxs-lookup"><span data-stu-id="66bf5-322">Action:CustomResponse</span></span>
* <span data-ttu-id="66bf5-323">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="66bf5-323">LogRewrittenUrl</span></span>

::: moniker-end

#### <a name="supported-server-variables"></a><span data-ttu-id="66bf5-324">Desteklenen sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="66bf5-324">Supported server variables</span></span>

<span data-ttu-id="66bf5-325">Ara yazılım, aşağıdaki IIS URL yeniden yazma modülü sunucu değişkenleri destekler:</span><span class="sxs-lookup"><span data-stu-id="66bf5-325">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="66bf5-326">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="66bf5-326">CONTENT_LENGTH</span></span>
* <span data-ttu-id="66bf5-327">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="66bf5-327">CONTENT_TYPE</span></span>
* <span data-ttu-id="66bf5-328">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="66bf5-328">HTTP_ACCEPT</span></span>
* <span data-ttu-id="66bf5-329">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="66bf5-329">HTTP_CONNECTION</span></span>
* <span data-ttu-id="66bf5-330">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="66bf5-330">HTTP_COOKIE</span></span>
* <span data-ttu-id="66bf5-331">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="66bf5-331">HTTP_HOST</span></span>
* <span data-ttu-id="66bf5-332">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="66bf5-332">HTTP_REFERER</span></span>
* <span data-ttu-id="66bf5-333">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="66bf5-333">HTTP_URL</span></span>
* <span data-ttu-id="66bf5-334">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="66bf5-334">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="66bf5-335">HTTPS</span><span class="sxs-lookup"><span data-stu-id="66bf5-335">HTTPS</span></span>
* <span data-ttu-id="66bf5-336">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="66bf5-336">LOCAL_ADDR</span></span>
* <span data-ttu-id="66bf5-337">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="66bf5-337">QUERY_STRING</span></span>
* <span data-ttu-id="66bf5-338">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="66bf5-338">REMOTE_ADDR</span></span>
* <span data-ttu-id="66bf5-339">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="66bf5-339">REMOTE_PORT</span></span>
* <span data-ttu-id="66bf5-340">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="66bf5-340">REQUEST_FILENAME</span></span>
* <span data-ttu-id="66bf5-341">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="66bf5-341">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="66bf5-342">De edinebilirsiniz bir `IFileProvider` aracılığıyla bir `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-342">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="66bf5-343">Bu yaklaşım, yeniden yazma konumu için daha fazla esneklik kuralları dosyaları sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-343">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="66bf5-344">Sağladığınız yolun sunucusuna yeniden yazma kuralları dosyalarınızı dağıtıldığından emin emin olun.</span><span class="sxs-lookup"><span data-stu-id="66bf5-344">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="66bf5-345">Metot tabanlı kuralı</span><span class="sxs-lookup"><span data-stu-id="66bf5-345">Method-based rule</span></span>

<span data-ttu-id="66bf5-346">Kullanım `Add(Action<RewriteContext> applyRule)` bir yöntemde kendi kural mantığı uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="66bf5-346">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="66bf5-347">`RewriteContext` Sunan `HttpContext` yönteminiz olarak kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="66bf5-347">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="66bf5-348">`RewriteContext.Result` Nasıl ek işlem hattı belirler işleme gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-348">The `RewriteContext.Result` determines how additional pipeline processing is handled.</span></span>

| `RewriteContext.Result`              | <span data-ttu-id="66bf5-349">Eylem</span><span class="sxs-lookup"><span data-stu-id="66bf5-349">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="66bf5-350">`RuleResult.ContinueRules` (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="66bf5-350">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="66bf5-351">Devam kuralları uygulama</span><span class="sxs-lookup"><span data-stu-id="66bf5-351">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="66bf5-352">Kuralları uygulanmasını durdurmak ve yanıtı gönder</span><span class="sxs-lookup"><span data-stu-id="66bf5-352">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="66bf5-353">Kuralları uygulanmasını durdurmak ve sonraki Ara yazılıma bağlamı gönderme</span><span class="sxs-lookup"><span data-stu-id="66bf5-353">Stop applying rules and send the context to the next middleware</span></span> |

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="66bf5-354">Örnek uygulama ile biten istekleri yolları için yönlendiren bir yöntemi gösterir *.xml*.</span><span class="sxs-lookup"><span data-stu-id="66bf5-354">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="66bf5-355">Bir istek yapıp yapmadığını `/file.xml`, onu yönlendireceği `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-355">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="66bf5-356">Durum kodu 301 (kalıcı taşındı) ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="66bf5-356">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="66bf5-357">İçin bir yeniden yönlendirme, yanıtın durum kodu açıkça ayarlamalısınız; Aksi takdirde, bir 200 (Tamam) durum kodu döndürülür ve istemcide yeniden yönlendirme karşılaşılmaz.</span><span class="sxs-lookup"><span data-stu-id="66bf5-357">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="66bf5-358">Özgün istek: `/file.xml`</span><span class="sxs-lookup"><span data-stu-id="66bf5-358">Original Request: `/file.xml`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların file.xml için izleme ile](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="66bf5-360">Kural Irule tabanlı</span><span class="sxs-lookup"><span data-stu-id="66bf5-360">IRule-based rule</span></span>

<span data-ttu-id="66bf5-361">Kullanım `Add(IRule)` uygulayan bir sınıf kendi kural mantığı kapsülleyen `IRule` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="66bf5-361">Use `Add(IRule)` to encapsulate your own rule logic in a class that implements the `IRule` interface.</span></span> <span data-ttu-id="66bf5-362">Kullanarak bir `IRule` yöntemi dayalı kural yaklaşımı kullanarak üzerinde daha fazla esneklik sağlar.</span><span class="sxs-lookup"><span data-stu-id="66bf5-362">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="66bf5-363">Burada, geçirebilirsiniz parametreleri için bir oluşturucu, uygulama sınıfınıza içerebilir `ApplyRule` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="66bf5-363">Your implementaion class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="66bf5-364">Örnek uygulama için parametre değerlerini `extension` ve `newPath` çeşitli koşullara uyması için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="66bf5-364">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="66bf5-365">`extension` Bir değer içermelidir ve değer olmalıdır *.png*, *.jpg*, veya *.gif*.</span><span class="sxs-lookup"><span data-stu-id="66bf5-365">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="66bf5-366">Varsa `newPath` geçerli olmayan bir `ArgumentException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="66bf5-366">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="66bf5-367">Bir istek yapıp yapmadığını *image.png*, onu yönlendireceği `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-367">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="66bf5-368">Bir istek yapıp yapmadığını *image.jpg*, onu yönlendireceği `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="66bf5-368">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="66bf5-369">Durum kodu 301 (kalıcı taşındı) olarak ayarlanır ve `context.Result` kural işlemeyi durdur ve yanıt göndermek için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="66bf5-369">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="66bf5-370">Özgün istek: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="66bf5-370">Original Request: `/image.png`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların image.png için izleme ile](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="66bf5-372">Özgün istek: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="66bf5-372">Original Request: `/image.jpg`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların image.jpg için izleme ile](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="66bf5-374">Normal ifade örnekleri</span><span class="sxs-lookup"><span data-stu-id="66bf5-374">Regex examples</span></span>

| <span data-ttu-id="66bf5-375">Hedef</span><span class="sxs-lookup"><span data-stu-id="66bf5-375">Goal</span></span> | <span data-ttu-id="66bf5-376">Normal ifade dizesini &</span><span class="sxs-lookup"><span data-stu-id="66bf5-376">Regex String &</span></span><br><span data-ttu-id="66bf5-377">Eşleşme örneği</span><span class="sxs-lookup"><span data-stu-id="66bf5-377">Match Example</span></span> | <span data-ttu-id="66bf5-378">Değiştirme dizesi &</span><span class="sxs-lookup"><span data-stu-id="66bf5-378">Replacement String &</span></span><br><span data-ttu-id="66bf5-379">Çıkış örneği</span><span class="sxs-lookup"><span data-stu-id="66bf5-379">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="66bf5-380">Querystring yol yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="66bf5-380">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="66bf5-381">Şerit sonunda eğik çizgi</span><span class="sxs-lookup"><span data-stu-id="66bf5-381">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="66bf5-382">Eğik zorla</span><span class="sxs-lookup"><span data-stu-id="66bf5-382">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="66bf5-383">Belirli isteklere yeniden yazma kaçının</span><span class="sxs-lookup"><span data-stu-id="66bf5-383">Avoid rewriting specific requests</span></span> | <span data-ttu-id="66bf5-384">`^(.*)(?<!\.axd)$` veya `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="66bf5-384">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="66bf5-385">Evet: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="66bf5-385">Yes: `/resource.htm`</span></span><br><span data-ttu-id="66bf5-386">Hayır: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="66bf5-386">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="66bf5-387">URL kesimleri bunları yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="66bf5-387">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="66bf5-388">URL kesimi değiştirin</span><span class="sxs-lookup"><span data-stu-id="66bf5-388">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="66bf5-389">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="66bf5-389">Additional resources</span></span>

* [<span data-ttu-id="66bf5-390">Uygulama Başlatma</span><span class="sxs-lookup"><span data-stu-id="66bf5-390">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="66bf5-391">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="66bf5-391">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="66bf5-392">.NET içinde normal ifadeler</span><span class="sxs-lookup"><span data-stu-id="66bf5-392">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="66bf5-393">Normal ifade dili - hızlı başvuru</span><span class="sxs-lookup"><span data-stu-id="66bf5-393">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="66bf5-394">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="66bf5-394">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="66bf5-395">URL yeniden yazma modülü 2.0 (IIS) kullanma</span><span class="sxs-lookup"><span data-stu-id="66bf5-395">Using Url Rewrite Module 2.0 (for IIS)</span></span>](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="66bf5-396">URL yeniden yazma Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="66bf5-396">URL Rewrite Module Configuration Reference</span></span>](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="66bf5-397">IIS URL yeniden yazma modülü Forumu</span><span class="sxs-lookup"><span data-stu-id="66bf5-397">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="66bf5-398">Basit bir URL yapısı tutun</span><span class="sxs-lookup"><span data-stu-id="66bf5-398">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="66bf5-399">10 URL yeniden yazma, ipuçları ve püf noktaları</span><span class="sxs-lookup"><span data-stu-id="66bf5-399">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="66bf5-400">Eğik çizgi veya değil eğik çizgi</span><span class="sxs-lookup"><span data-stu-id="66bf5-400">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
