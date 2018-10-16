---
title: URL yeniden yazma ara yazılımı ASP.NET core'da
author: guardrex
description: URL yeniden yazma ve URL yeniden yazma ara yazılımı ile ASP.NET Core uygulamalarında yeniden yönlendirme hakkında bilgi edinin.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: d9f33f34f75fe7bf534146c5a426335e74635018
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326075"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="916b3-103">URL yeniden yazma ara yazılımı ASP.NET core'da</span><span class="sxs-lookup"><span data-stu-id="916b3-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="916b3-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="916b3-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="916b3-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="916b3-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="916b3-106">URL yeniden yazma URL bir veya daha fazla önceden tanımlanmış kurallara göre istek değiştirme işlemidir.</span><span class="sxs-lookup"><span data-stu-id="916b3-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="916b3-107">Böylece konumları ve adresleri sıkı şekilde bağlı olmayan URL yeniden yazma kaynak konumları ve adresleri arasında bir Özet oluşturur.</span><span class="sxs-lookup"><span data-stu-id="916b3-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="916b3-108">URL yeniden yazma yararlı olduğu bazı senaryolar vardır:</span><span class="sxs-lookup"><span data-stu-id="916b3-108">There are several scenarios where URL rewriting is valuable:</span></span>

* <span data-ttu-id="916b3-109">Taşıma veya bu kaynaklar için kararlı bulucular korurken sunucu kaynaklarını geçici veya kalıcı olarak değiştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="916b3-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources.</span></span>
* <span data-ttu-id="916b3-110">İstek farklı uygulamalar arasında veya tek bir uygulama alanları genelinde işleme bölme.</span><span class="sxs-lookup"><span data-stu-id="916b3-110">Splitting request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="916b3-111">Kaldırma, ekleme veya URL kesimleri gelen isteklerden yeniden düzenleme.</span><span class="sxs-lookup"><span data-stu-id="916b3-111">Removing, adding, or reorganizing URL segments on incoming requests.</span></span>
* <span data-ttu-id="916b3-112">En iyi duruma getirme Genel URL'ler arama motoru iyileştirmesi (SEO).</span><span class="sxs-lookup"><span data-stu-id="916b3-112">Optimizing public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="916b3-113">Bir bağlantıyı izleyerek bulabilirsiniz içeriği tahmin kolaylaştıracak kolay genel URL kullanımını erişimine izin verme.</span><span class="sxs-lookup"><span data-stu-id="916b3-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link.</span></span>
* <span data-ttu-id="916b3-114">Uç noktalarını güvenli hale getirmek için güvenli istekleri yeniden yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="916b3-114">Redirecting insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="916b3-115">Görüntü hotlinking engelliyor.</span><span class="sxs-lookup"><span data-stu-id="916b3-115">Preventing image hotlinking.</span></span>

<span data-ttu-id="916b3-116">URL çeşitli şekillerde değiştirme, Regex, Apache mod_rewrite modülü kuralları, IIS yeniden yazma modülü kuralları da dahil olmak üzere ve özel kural mantığı kullanarak kurallar tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="916b3-116">You can define rules for changing the URL in several ways, including Regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="916b3-117">Bu belge, URL yeniden yazma URL yeniden yazma ara yazılımı ASP.NET Core uygulamalarında kullanma hakkında yönergeler sunar.</span><span class="sxs-lookup"><span data-stu-id="916b3-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="916b3-118">URL yeniden yazma uygulama performansını düşürebilir.</span><span class="sxs-lookup"><span data-stu-id="916b3-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="916b3-119">Uygun olduğunda, sayısı ve karmaşıklığı kurallar sınırlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="916b3-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="916b3-120">URL yeniden yönlendirme ve URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="916b3-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="916b3-121">İfade arasındaki farkı *URL yeniden yönlendirme* ve *URL yeniden yazma* en küçük görünebilir ancak ilk kaynakları istemcilere sağlamak için önemli etkileri vardır.</span><span class="sxs-lookup"><span data-stu-id="916b3-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="916b3-122">ASP.NET Core'nın URL yeniden yazma ara yazılımı her ikisi de gereksinimini toplantı yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="916b3-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="916b3-123">A *URL yeniden yönlendirme* istemci burada belirtildiği başka bir adresten bir kaynağa erişmek için bir istemci tarafı işlemi olduğundan.</span><span class="sxs-lookup"><span data-stu-id="916b3-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="916b3-124">Bu sunucu için bir gidiş dönüş gerektirir.</span><span class="sxs-lookup"><span data-stu-id="916b3-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="916b3-125">İstemci kaynak için yeni bir istekte bulunduğunda istemciye döndürülen yeniden yönlendirme URL'sini tarayıcınızın adres çubuğunda görünür.</span><span class="sxs-lookup"><span data-stu-id="916b3-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="916b3-126">Varsa `/resource` olduğu *yeniden yönlendirilen* için `/different-resource`, istemci isteklerini `/resource`.</span><span class="sxs-lookup"><span data-stu-id="916b3-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="916b3-127">İstemci kaynak edinmelidir sunucunun yanıt `/different-resource` yeniden yönlendirme geçici veya kalıcı olduğunu gösteren bir durum kodu ile.</span><span class="sxs-lookup"><span data-stu-id="916b3-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="916b3-128">İstemci yeniden yönlendirme URL'sini kaynak için yeni bir isteği yürütür.</span><span class="sxs-lookup"><span data-stu-id="916b3-128">The client executes a new request for the resource at the redirect URL.</span></span>

![Webapı hizmet uç noktası sunucusundaki sürüm 2 (v2) 1 (v1) sürümünden geçici olarak değiştirildi.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="916b3-134">İstekleri için farklı bir URL yeniden yönlendirme, yeniden yönlendirme kalıcı veya geçici olup olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="916b3-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="916b3-135">301 (kalıcı taşındı) durum kodunu burada yeni, kalıcı bir URL kaynağı vardır ve istediğiniz tüm gelecek istekleri kaynak için yeni URL kullanması gerektiğini istemci istemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="916b3-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="916b3-136">*301 durum kodu alındığında istemci yanıtı önbelleğe alabilir.*</span><span class="sxs-lookup"><span data-stu-id="916b3-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="916b3-137">(Bulunamadı) durum kodunu 302 yeniden yönlendirme geçici veya genel konu olduğu istemci depolamak olmamalıdır ve gelecekte yeniden yönlendirme URL'sini yeniden şekilde değiştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="916b3-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="916b3-138">Daha fazla bilgi için [RFC 2616: durum kodu tanımları](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="916b3-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="916b3-139">A *URL yeniden yazma* farklı bir kaynak adresten bir kaynak sağlamak için bir sunucu tarafı işlemi.</span><span class="sxs-lookup"><span data-stu-id="916b3-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="916b3-140">Bir URL yeniden yazma sunucuya gidiş dönüşlü hale getirmek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="916b3-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="916b3-141">Yeniden URL istemciye döndürülen değildir ve tarayıcının adres çubuğunda görüntülenmez.</span><span class="sxs-lookup"><span data-stu-id="916b3-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="916b3-142">Zaman `/resource` olduğu *yazılan* için `/different-resource`, istemci isteklerini `/resource`ve sunucu *dahili olarak* kaynakta getirir `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="916b3-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="916b3-143">İstemci yeniden URL'deki kaynak alınamadı olabilir, ancak istemci, isteği yapar ve yanıtı alır, kaynağın yeniden URL'de mevcut haberdar olmaz.</span><span class="sxs-lookup"><span data-stu-id="916b3-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Webapı hizmet uç noktası, sunucu üzerindeki sürüm 2 (v2) için 1 (v1) sürümünden değiştirildi.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="916b3-148">URL yeniden yazma örnek uygulaması</span><span class="sxs-lookup"><span data-stu-id="916b3-148">URL rewriting sample app</span></span>

<span data-ttu-id="916b3-149">URL yeniden yazma ara yazılımı ile özelliklerini inceleyebilirsiniz [URL yeniden yazma örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span><span class="sxs-lookup"><span data-stu-id="916b3-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="916b3-150">Uygulamaya yeniden uygular ve yeniden yönlendirme kuralları ve yeniden ya da yeniden yönlendirilen URL'sini gösterir.</span><span class="sxs-lookup"><span data-stu-id="916b3-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="916b3-151">URL yeniden yazma ara yazılımı kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="916b3-151">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="916b3-152">URL yeniden yazma ara yazılımı kullanamaz olduğunda kullanın [URL yeniden yazma Modülü](https://www.iis.net/downloads/microsoft/url-rewrite) Windows Server, IIS ile [Apache mod_rewrite Modülü](https://httpd.apache.org/docs/2.4/rewrite/) Apache sunucusundaki [NgınxüzerindeURLyenidenyazma](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), veya uygulamanız üzerinde barındırılan [HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıyla [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="916b3-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="916b3-153">Sunucu tabanlı URL yeniden yazma teknolojileri IIS, Apache veya Ngınx kullanmak için ana ara yazılım, bu modüllerin tam özelliklerini desteklemiyor ve ara yazılım performansını büyük olasılıkla, modül eşleşmeyecektir nedenleridir.</span><span class="sxs-lookup"><span data-stu-id="916b3-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="916b3-154">Ancak, ASP.NET Core projeleriyle gibi çalışmayan sunucu modülü bazı özellikler mevcuttur `IsFile` ve `IsDirectory` IIS yeniden yazma modülü kısıtlamaları.</span><span class="sxs-lookup"><span data-stu-id="916b3-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="916b3-155">Bu senaryolarda, ara yazılım kullanın.</span><span class="sxs-lookup"><span data-stu-id="916b3-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="916b3-156">Paket</span><span class="sxs-lookup"><span data-stu-id="916b3-156">Package</span></span>

<span data-ttu-id="916b3-157">Ara yazılım projenize eklemek için bir başvuru ekleyin. [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) paket.</span><span class="sxs-lookup"><span data-stu-id="916b3-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="916b3-158">Bu özellik, ASP.NET Core 1.1 hedefleyen uygulamalar için veya sonraki kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="916b3-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="916b3-159">Uzantı ve seçenekleri</span><span class="sxs-lookup"><span data-stu-id="916b3-159">Extension and options</span></span>

<span data-ttu-id="916b3-160">URL yeniden yazma oluşturmak ve bir örneğini oluşturarak kuralları yeniden yönlendirme [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) sınıfı, kuralların her biri için genişletme yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="916b3-160">Establish your URL rewrite and redirect rules by creating an instance of the [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) class with extension methods for each of your rules.</span></span> <span data-ttu-id="916b3-161">İşlenen bunları istediğiniz sırayla birden çok kural zincir.</span><span class="sxs-lookup"><span data-stu-id="916b3-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="916b3-162">`RewriteOptions` İle istek ardışık düzenine eklenen URL yeniden yazma ara yazılımı geçirilen `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="916b3-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

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

### <a name="redirect-non-www-to-www"></a><span data-ttu-id="916b3-163">Www www olmayan yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="916b3-163">Redirect non-www to www</span></span>

<span data-ttu-id="916b3-164">Üç seçenek olmayan yönlendirmek için uygulama izin`www` ister `www`:</span><span class="sxs-lookup"><span data-stu-id="916b3-164">Three options permit the app to redirect non-`www` requests to `www`:</span></span>

* <span data-ttu-id="916b3-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; kalıcı olarak isteği yönlendirmek `www` istek olmayan ise alt etki alanı`www`.</span><span class="sxs-lookup"><span data-stu-id="916b3-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; Permanently redirect the request to the `www` subdomain if the request is non-`www`.</span></span> <span data-ttu-id="916b3-166">İle yeniden yönlendiren bir [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) durum kodu.</span><span class="sxs-lookup"><span data-stu-id="916b3-166">Redirects with a [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) status code.</span></span>
* <span data-ttu-id="916b3-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; isteği yönlendirmek `www` gelen istek olmayan ise alt etki alanı`www`.</span><span class="sxs-lookup"><span data-stu-id="916b3-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="916b3-168">İle yeniden yönlendiren bir [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) durum kodu.</span><span class="sxs-lookup"><span data-stu-id="916b3-168">Redirects with a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) status code.</span></span>
* <span data-ttu-id="916b3-169">[AddRedirectToWww (RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; isteği yönlendirmek `www` gelen istek olmayan ise alt etki alanı`www`.</span><span class="sxs-lookup"><span data-stu-id="916b3-169">[AddRedirectToWww(RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="916b3-170">Yanıt durum kodu girmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="916b3-170">Allows you to provide the status code for the response.</span></span> <span data-ttu-id="916b3-171">Alanlarını kullanın [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) sınıfı atamaların `AddRedirectToWww`.</span><span class="sxs-lookup"><span data-stu-id="916b3-171">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `AddRedirectToWww`.</span></span>

::: moniker-end

### <a name="url-redirect"></a><span data-ttu-id="916b3-172">URL yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="916b3-172">URL redirect</span></span>

<span data-ttu-id="916b3-173">Kullanım `AddRedirect` isteklerini yeniden yönlendirmek için.</span><span class="sxs-lookup"><span data-stu-id="916b3-173">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="916b3-174">İlk parametre, normal ifade gelen URL yolunda eşleşen içerir.</span><span class="sxs-lookup"><span data-stu-id="916b3-174">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="916b3-175">İkinci değiştirme dizesi parametresidir.</span><span class="sxs-lookup"><span data-stu-id="916b3-175">The second parameter is the replacement string.</span></span> <span data-ttu-id="916b3-176">Üçüncü parametre varsa, durum kodu belirtir.</span><span class="sxs-lookup"><span data-stu-id="916b3-176">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="916b3-177">Durum kodu belirtmezseniz, kaynak geçici olarak taşındı veya değiştirilinceye olduğunu gösteren 302 (bulunamadı) için varsayılanları.</span><span class="sxs-lookup"><span data-stu-id="916b3-177">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

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

<span data-ttu-id="916b3-178">Geliştirici Araçları etkin bir tarayıcıda yolu kullanarak örnek uygulamaya istekte bulunmak `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="916b3-178">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="916b3-179">Normal ifade üzerinde istek yolunun eşleşmesi `redirect-rule/(.*)`, ve yolu ile değiştirilir `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="916b3-179">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="916b3-180">Yeniden yönlendirme URL'si istemcisine bir 302 (bulunamadı) durum kodu ile gönderilir.</span><span class="sxs-lookup"><span data-stu-id="916b3-180">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="916b3-181">Tarayıcı, tarayıcının adres çubuğunda görüntülenen yeniden yönlendirme URL'sindeki yeni bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="916b3-181">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="916b3-182">Örnek uygulamada hiçbir kural üzerinde yeniden yönlendirme URL'si aynı olduğundan, ikinci isteği uygulamadan 200 (Tamam) yanıtı alır ve yanıt gövdesinin yeniden yönlendirme URL'sini gösterir.</span><span class="sxs-lookup"><span data-stu-id="916b3-182">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="916b3-183">Bir URL ise bir gidiş dönüş sunucuya yapılan *yeniden yönlendirilen*.</span><span class="sxs-lookup"><span data-stu-id="916b3-183">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="916b3-184">Yeniden yönlendirme kurallarınızı oluştururken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="916b3-184">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="916b3-185">Yeniden yönlendirme kuralları, sonra bir yeniden yönlendirme de dahil olmak üzere uygulama, her istekte değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="916b3-185">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="916b3-186">Yanlışlıkla için kolay bir sonsuz yeniden yönlendirmeleri döngüsünü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="916b3-186">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="916b3-187">Özgün istek: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="916b3-187">Original Request: `/redirect-rule/1234/5678`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="916b3-189">Parantez içinde yer alan bir ifade parçası olarak adlandırılan bir *yakalama grubu*.</span><span class="sxs-lookup"><span data-stu-id="916b3-189">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="916b3-190">Nokta (`.`) ifade anlamına gelir *herhangi bir karakterle eşleştir*.</span><span class="sxs-lookup"><span data-stu-id="916b3-190">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="916b3-191">Yıldız işareti (`*`) gösteren *önceki karakteri sıfır veya daha fazla kez eşleştir*.</span><span class="sxs-lookup"><span data-stu-id="916b3-191">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="916b3-192">Bu nedenle, son iki yol kesimleri URL'sinin `1234/5678`, yakalama grubu tarafından yakalanan `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="916b3-192">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="916b3-193">Sonra istek URL'sindeki sağladığınız herhangi bir değer `redirect-rule/` bu tek bir yakalama grubu tarafından yakalanır.</span><span class="sxs-lookup"><span data-stu-id="916b3-193">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="916b3-194">Değiştirme dizesinde yakalanan gruplar dolar işareti ile dizesine eklenen (`$`) yakalama dizisi sayı takip eder.</span><span class="sxs-lookup"><span data-stu-id="916b3-194">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="916b3-195">İlk yakalama grubu değer ile elde edilen `$1`ikinci ile `$2`, ve bunların sırası, normal ifade yakalama grupları için devam edin.</span><span class="sxs-lookup"><span data-stu-id="916b3-195">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="916b3-196">Var. yalnızca bir yakalanan gruba içinde yeniden yönlendirme kuralı regex örnek uygulamada, olan değiştirme dizesinde tek eklenen grubu için `$1`.</span><span class="sxs-lookup"><span data-stu-id="916b3-196">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="916b3-197">Kural uygulandığında, URL olur `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="916b3-197">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="916b3-198">Güvenli bir uç noktası için URL yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="916b3-198">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="916b3-199">Kullanım `AddRedirectToHttps` aynı konak ve yol HTTPS kullanarak HTTP isteklerini yeniden yönlendirmek için (`https://`).</span><span class="sxs-lookup"><span data-stu-id="916b3-199">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="916b3-200">Durum kodu verilmiyorsa, ara yazılım 302 (bulunamadı) için varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="916b3-200">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="916b3-201">Bağlantı noktası verilmiyorsa, ara yazılım için varsayılan olarak `null`, Protokolü yani değişikliklerini `https://` ve istemci kaynak bağlantı noktası 443 üzerinden erişir.</span><span class="sxs-lookup"><span data-stu-id="916b3-201">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="916b3-202">Örneğin, durum kodu 301 (kalıcı taşındı) ve bağlantı noktasını değiştirmek için 5001 gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="916b3-202">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="916b3-203">Kullanım `AddRedirectToHttpsPermanent` aynı konak ve yol güvenli HTTPS protokolü ile güvenli isteklerini yeniden yönlendirmek için (`https://` bağlantı noktası 443 üzerinden).</span><span class="sxs-lookup"><span data-stu-id="916b3-203">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="916b3-204">Ara yazılım 301 (kalıcı taşındı) durum kodunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="916b3-204">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="916b3-205">HTTPS için ek yeniden yönlendirme kuralları gereksinimi olmadan yönlendirirken, HTTPS yeniden yönlendirmesi ara yazılım kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="916b3-205">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="916b3-206">Daha fazla bilgi için [HTTPS zorlama](xref:security/enforcing-ssl#require-https) konu.</span><span class="sxs-lookup"><span data-stu-id="916b3-206">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="916b3-207">Örnek uygulamayı nasıl kullanılacağını gösteren özellikli `AddRedirectToHttps` veya `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="916b3-207">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="916b3-208">Uzantı yöntemine ekleyin `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="916b3-208">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="916b3-209">Güvenli olmayan bir istek, herhangi bir URL konumundaki uygulamaya olun.</span><span class="sxs-lookup"><span data-stu-id="916b3-209">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="916b3-210">Tarayıcı güvenlik otomatik olarak imzalanan sertifika güvenilmeyen uyarısını kapatmak veya sertifikaya güvenmek için bir özel durum oluşturun.</span><span class="sxs-lookup"><span data-stu-id="916b3-210">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="916b3-211">Özgün istek kullanarak `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="916b3-211">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="916b3-213">Özgün istek kullanarak `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="916b3-213">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="916b3-215">URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="916b3-215">URL rewrite</span></span>

<span data-ttu-id="916b3-216">Kullanım `AddRewrite` URL yeniden yazma için bir kural oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="916b3-216">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="916b3-217">İlk parametre gelen URL yolu temel eşlemek için normal ifade içeriyor.</span><span class="sxs-lookup"><span data-stu-id="916b3-217">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="916b3-218">İkinci değiştirme dizesi parametresidir.</span><span class="sxs-lookup"><span data-stu-id="916b3-218">The second parameter is the replacement string.</span></span> <span data-ttu-id="916b3-219">Üçüncü parametreyi `skipRemainingRules: {true|false}`, ara yazılımı için geçerli kural uyguladıysanız, ek bir yeniden yazma kuralları atlamak gerekip gerekmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="916b3-219">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

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

<span data-ttu-id="916b3-220">Özgün istek: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="916b3-220">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıt izleme](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="916b3-222">Simgeyi seçtiğinizde normal ifade fark ilk şey olduğunu (`^`) ifadenin başında.</span><span class="sxs-lookup"><span data-stu-id="916b3-222">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="916b3-223">Başka bir deyişle, eşleşen bir URL yolu başlangıcında başlar.</span><span class="sxs-lookup"><span data-stu-id="916b3-223">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="916b3-224">Yeniden yönlendirme kuralı ile bir önceki örnekte `redirect-rule/(.*)`, normal ifade başlangıcında hiçbir ayar yoktur; bu nedenle, herhangi bir karakter önünde `redirect-rule/` yolunda başarılı bir eşleşme.</span><span class="sxs-lookup"><span data-stu-id="916b3-224">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="916b3-225">Yol</span><span class="sxs-lookup"><span data-stu-id="916b3-225">Path</span></span>                               | <span data-ttu-id="916b3-226">Eşleştirme</span><span class="sxs-lookup"><span data-stu-id="916b3-226">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="916b3-227">Evet</span><span class="sxs-lookup"><span data-stu-id="916b3-227">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="916b3-228">Evet</span><span class="sxs-lookup"><span data-stu-id="916b3-228">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="916b3-229">Evet</span><span class="sxs-lookup"><span data-stu-id="916b3-229">Yes</span></span>   |

<span data-ttu-id="916b3-230">Yeniden üretme kuralı `^rewrite-rule/(\d+)/(\d+)`, ile başlatırsanız, yalnızca yollarıyla eşleşen `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="916b3-230">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="916b3-231">İçinde eşleşen aşağıdaki yeniden yazma kuralı ve yukarıdaki yeniden yönlendirme kuralı farka dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="916b3-231">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="916b3-232">Yol</span><span class="sxs-lookup"><span data-stu-id="916b3-232">Path</span></span>                              | <span data-ttu-id="916b3-233">Eşleştirme</span><span class="sxs-lookup"><span data-stu-id="916b3-233">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="916b3-234">Evet</span><span class="sxs-lookup"><span data-stu-id="916b3-234">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="916b3-235">Hayır</span><span class="sxs-lookup"><span data-stu-id="916b3-235">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="916b3-236">Hayır</span><span class="sxs-lookup"><span data-stu-id="916b3-236">No</span></span>    |

<span data-ttu-id="916b3-237">Aşağıdaki `^rewrite-rule/` bölümü ifadesi, iki yakalama grupları vardır `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="916b3-237">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="916b3-238">`\d` Gösterir *bir basamağı (sayı) eşleşen*.</span><span class="sxs-lookup"><span data-stu-id="916b3-238">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="916b3-239">Artı işaretini (`+`) anlamına gelir *bir veya daha önceki karakter eşleşen*.</span><span class="sxs-lookup"><span data-stu-id="916b3-239">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="916b3-240">Bu nedenle, URL, başka bir sayının İleri-eğik çizgi ardından bir sayı içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="916b3-240">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="916b3-241">Bu yakalama grupları yeniden URL eklenmiş `$1` ve `$2`.</span><span class="sxs-lookup"><span data-stu-id="916b3-241">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="916b3-242">Yeniden yazma kuralı değiştirme dizesi yakalanan gruplar querystring yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="916b3-242">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="916b3-243">İstenen yolunu `/rewrite-rule/1234/5678` kaynağı almak için yazılan `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="916b3-243">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="916b3-244">Özgün istek üzerinde bir sorgu dizesi varsa, URL'yi yeniden zaman korunur.</span><span class="sxs-lookup"><span data-stu-id="916b3-244">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="916b3-245">Hiçbir geri dönüş kaynağı almak için sunucuya yoktur.</span><span class="sxs-lookup"><span data-stu-id="916b3-245">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="916b3-246">Kaynağın varolup olmadığını getirildi ve 200 (Tamam) durum kodu ile istemciye döndürülen.</span><span class="sxs-lookup"><span data-stu-id="916b3-246">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="916b3-247">Tarayıcı adres çubuğundaki URL'yi, istemci yeniden yönlendirilen değildir çünkü değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="916b3-247">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="916b3-248">İstemci endişelenmiştir kadar hiçbir zaman URL yeniden yazma işlemi oluştu.</span><span class="sxs-lookup"><span data-stu-id="916b3-248">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="916b3-249">Kullanım `skipRemainingRules: true` mümkün olduğunda, çünkü kurallarına pahalı bir işlemdir ve uygulama yanıt süresini azaltır.</span><span class="sxs-lookup"><span data-stu-id="916b3-249">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="916b3-250">Hızlı Uygulama yanıtı:</span><span class="sxs-lookup"><span data-stu-id="916b3-250">For the fastest app response:</span></span>
> * <span data-ttu-id="916b3-251">En sık eşleşen kural, yeniden yazma kuralları az sık eşleşen kural sipariş.</span><span class="sxs-lookup"><span data-stu-id="916b3-251">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="916b3-252">Bir eşleşme olursa ve hiçbir ek kural işleme gerekli olduğunda kalan kurallarının işlenmesini atlayın.</span><span class="sxs-lookup"><span data-stu-id="916b3-252">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="916b3-253">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="916b3-253">Apache mod_rewrite</span></span>

<span data-ttu-id="916b3-254">Apache mod_rewrite kurallarıyla uygulamak `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="916b3-254">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="916b3-255">Kurallar dosyası uygulamayla dağıtıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="916b3-255">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="916b3-256">Daha fazla bilgi ve mod_rewrite kuralları örnekleri için bkz. [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="916b3-256">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="916b3-257">A `StreamReader` kurallardan okumak için kullanılan *ApacheModRewrite.txt* kurallar dosyası.</span><span class="sxs-lookup"><span data-stu-id="916b3-257">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="916b3-258">İlk parametre bir `IFileProvider`, aracılığıyla sağlanan [bağımlılık ekleme](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="916b3-258">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="916b3-259">`IHostingEnvironment` Sağlamak için eklenen `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="916b3-259">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="916b3-260">İkinci parametre olan kurallar dosyası yolu olan *ApacheModRewrite.txt* örnek uygulamada.</span><span class="sxs-lookup"><span data-stu-id="916b3-260">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="916b3-261">Örnek uygulama isteklerinden yönlendiren `/apache-mod-rules-redirect/(.\*)` için `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="916b3-261">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="916b3-262">Yanıt durum kodu 302 (bulunamadı) ' dir.</span><span class="sxs-lookup"><span data-stu-id="916b3-262">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="916b3-263">Özgün istek: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="916b3-263">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_apache_mod_redirect.png)

<span data-ttu-id="916b3-265">Ara yazılım, aşağıdaki Apache mod_rewrite sunucu değişkenlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="916b3-265">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="916b3-266">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="916b3-266">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="916b3-267">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="916b3-267">HTTP_ACCEPT</span></span>
* <span data-ttu-id="916b3-268">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="916b3-268">HTTP_CONNECTION</span></span>
* <span data-ttu-id="916b3-269">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="916b3-269">HTTP_COOKIE</span></span>
* <span data-ttu-id="916b3-270">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="916b3-270">HTTP_FORWARDED</span></span>
* <span data-ttu-id="916b3-271">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="916b3-271">HTTP_HOST</span></span>
* <span data-ttu-id="916b3-272">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="916b3-272">HTTP_REFERER</span></span>
* <span data-ttu-id="916b3-273">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="916b3-273">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="916b3-274">HTTPS</span><span class="sxs-lookup"><span data-stu-id="916b3-274">HTTPS</span></span>
* <span data-ttu-id="916b3-275">IPV6</span><span class="sxs-lookup"><span data-stu-id="916b3-275">IPV6</span></span>
* <span data-ttu-id="916b3-276">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="916b3-276">QUERY_STRING</span></span>
* <span data-ttu-id="916b3-277">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="916b3-277">REMOTE_ADDR</span></span>
* <span data-ttu-id="916b3-278">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="916b3-278">REMOTE_PORT</span></span>
* <span data-ttu-id="916b3-279">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="916b3-279">REQUEST_FILENAME</span></span>
* <span data-ttu-id="916b3-280">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="916b3-280">REQUEST_METHOD</span></span>
* <span data-ttu-id="916b3-281">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="916b3-281">REQUEST_SCHEME</span></span>
* <span data-ttu-id="916b3-282">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="916b3-282">REQUEST_URI</span></span>
* <span data-ttu-id="916b3-283">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="916b3-283">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="916b3-284">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="916b3-284">SERVER_ADDR</span></span>
* <span data-ttu-id="916b3-285">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="916b3-285">SERVER_PORT</span></span>
* <span data-ttu-id="916b3-286">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="916b3-286">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="916b3-287">SAAT</span><span class="sxs-lookup"><span data-stu-id="916b3-287">TIME</span></span>
* <span data-ttu-id="916b3-288">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="916b3-288">TIME_DAY</span></span>
* <span data-ttu-id="916b3-289">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="916b3-289">TIME_HOUR</span></span>
* <span data-ttu-id="916b3-290">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="916b3-290">TIME_MIN</span></span>
* <span data-ttu-id="916b3-291">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="916b3-291">TIME_MON</span></span>
* <span data-ttu-id="916b3-292">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="916b3-292">TIME_SEC</span></span>
* <span data-ttu-id="916b3-293">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="916b3-293">TIME_WDAY</span></span>
* <span data-ttu-id="916b3-294">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="916b3-294">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="916b3-295">IIS URL yeniden yazma modülü kuralları</span><span class="sxs-lookup"><span data-stu-id="916b3-295">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="916b3-296">IIS URL yeniden yazma modülü uygulanan kurallarını kullanmak için `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="916b3-296">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="916b3-297">Kurallar dosyası uygulamayla dağıtıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="916b3-297">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="916b3-298">Kullanılacak Ara yazılımının doğrudan yoksa, *web.config* dosya Windows Server IIS üzerinde çalışırken.</span><span class="sxs-lookup"><span data-stu-id="916b3-298">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="916b3-299">IIS ile bu kuralları dışında depolanması gereken, *web.config* IIS yeniden yazma modülü ile çakışmalarını önlemek için.</span><span class="sxs-lookup"><span data-stu-id="916b3-299">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="916b3-300">Daha fazla bilgi ve IIS URL yeniden yazma modülü kuralları örnekleri için bkz. [Url yeniden yazma modülü 2.0 kullanarak](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) ve [URL yeniden yazma Module yapılandırma başvurusu](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="916b3-300">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="916b3-301">A `StreamReader` kurallardan okumak için kullanılan *IISUrlRewrite.xml* kurallar dosyası.</span><span class="sxs-lookup"><span data-stu-id="916b3-301">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="916b3-302">İlk parametre bir `IFileProvider`, ikinci parametre olan, XML kuralları dosyası yolu olsa da *IISUrlRewrite.xml* örnek uygulamada.</span><span class="sxs-lookup"><span data-stu-id="916b3-302">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="916b3-303">Örnek uygulama, gelen istekleri yeniden yazar `/iis-rules-rewrite/(.*)` için `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="916b3-303">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="916b3-304">İstemciye bir 200 (Tamam) durum koduyla yanıt gönderilir.</span><span class="sxs-lookup"><span data-stu-id="916b3-304">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="916b3-305">Özgün istek: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="916b3-305">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıt izleme](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="916b3-307">Etkin bir IIS yeniden yazma modülü ile uygulamanızı istenmeyen yollarla erişememeleri yapılandırılmış sunucu düzeyinde kurallar varsa, IIS yeniden yazma modülü bir uygulama için devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="916b3-307">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="916b3-308">Daha fazla bilgi için [devre dışı bırakma IIS modüllerini](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="916b3-308">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="916b3-309">Desteklenmeyen özellikler</span><span class="sxs-lookup"><span data-stu-id="916b3-309">Unsupported features</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="916b3-310">Yayımlanan bir ara yazılım ile ASP.NET Core 2.x, aşağıdaki IIS URL yeniden yazma modülü özellikleri desteklemez:</span><span class="sxs-lookup"><span data-stu-id="916b3-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="916b3-311">Giden kuralları</span><span class="sxs-lookup"><span data-stu-id="916b3-311">Outbound Rules</span></span>
* <span data-ttu-id="916b3-312">Özel sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="916b3-312">Custom Server Variables</span></span>
* <span data-ttu-id="916b3-313">Joker karakterler</span><span class="sxs-lookup"><span data-stu-id="916b3-313">Wildcards</span></span>
* <span data-ttu-id="916b3-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="916b3-314">LogRewrittenUrl</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="916b3-315">Yayımlanan bir ara yazılım ile ASP.NET Core 1.x aşağıdaki IIS URL yeniden yazma modülü özellikleri desteklemez:</span><span class="sxs-lookup"><span data-stu-id="916b3-315">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="916b3-316">Genel kurallar</span><span class="sxs-lookup"><span data-stu-id="916b3-316">Global Rules</span></span>
* <span data-ttu-id="916b3-317">Giden kuralları</span><span class="sxs-lookup"><span data-stu-id="916b3-317">Outbound Rules</span></span>
* <span data-ttu-id="916b3-318">Yeniden eşlemeleri</span><span class="sxs-lookup"><span data-stu-id="916b3-318">Rewrite Maps</span></span>
* <span data-ttu-id="916b3-319">CustomResponse eylemi</span><span class="sxs-lookup"><span data-stu-id="916b3-319">CustomResponse Action</span></span>
* <span data-ttu-id="916b3-320">Özel sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="916b3-320">Custom Server Variables</span></span>
* <span data-ttu-id="916b3-321">Joker karakterler</span><span class="sxs-lookup"><span data-stu-id="916b3-321">Wildcards</span></span>
* <span data-ttu-id="916b3-322">Eylem: CustomResponse</span><span class="sxs-lookup"><span data-stu-id="916b3-322">Action:CustomResponse</span></span>
* <span data-ttu-id="916b3-323">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="916b3-323">LogRewrittenUrl</span></span>

::: moniker-end

#### <a name="supported-server-variables"></a><span data-ttu-id="916b3-324">Desteklenen sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="916b3-324">Supported server variables</span></span>

<span data-ttu-id="916b3-325">Ara yazılım, aşağıdaki IIS URL yeniden yazma modülü sunucu değişkenleri destekler:</span><span class="sxs-lookup"><span data-stu-id="916b3-325">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="916b3-326">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="916b3-326">CONTENT_LENGTH</span></span>
* <span data-ttu-id="916b3-327">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="916b3-327">CONTENT_TYPE</span></span>
* <span data-ttu-id="916b3-328">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="916b3-328">HTTP_ACCEPT</span></span>
* <span data-ttu-id="916b3-329">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="916b3-329">HTTP_CONNECTION</span></span>
* <span data-ttu-id="916b3-330">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="916b3-330">HTTP_COOKIE</span></span>
* <span data-ttu-id="916b3-331">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="916b3-331">HTTP_HOST</span></span>
* <span data-ttu-id="916b3-332">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="916b3-332">HTTP_REFERER</span></span>
* <span data-ttu-id="916b3-333">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="916b3-333">HTTP_URL</span></span>
* <span data-ttu-id="916b3-334">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="916b3-334">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="916b3-335">HTTPS</span><span class="sxs-lookup"><span data-stu-id="916b3-335">HTTPS</span></span>
* <span data-ttu-id="916b3-336">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="916b3-336">LOCAL_ADDR</span></span>
* <span data-ttu-id="916b3-337">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="916b3-337">QUERY_STRING</span></span>
* <span data-ttu-id="916b3-338">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="916b3-338">REMOTE_ADDR</span></span>
* <span data-ttu-id="916b3-339">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="916b3-339">REMOTE_PORT</span></span>
* <span data-ttu-id="916b3-340">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="916b3-340">REQUEST_FILENAME</span></span>
* <span data-ttu-id="916b3-341">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="916b3-341">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="916b3-342">De edinebilirsiniz bir `IFileProvider` aracılığıyla bir `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="916b3-342">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="916b3-343">Bu yaklaşım, yeniden yazma konumu için daha fazla esneklik kuralları dosyaları sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="916b3-343">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="916b3-344">Sağladığınız yolun sunucusuna yeniden yazma kuralları dosyalarınızı dağıtıldığından emin emin olun.</span><span class="sxs-lookup"><span data-stu-id="916b3-344">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="916b3-345">Metot tabanlı kuralı</span><span class="sxs-lookup"><span data-stu-id="916b3-345">Method-based rule</span></span>

<span data-ttu-id="916b3-346">Kullanım `Add(Action<RewriteContext> applyRule)` bir yöntemde kendi kural mantığı uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="916b3-346">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="916b3-347">`RewriteContext` Sunan `HttpContext` yönteminiz olarak kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="916b3-347">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="916b3-348">`context.Result` Nasıl ek işlem hattı belirler işleme gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="916b3-348">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="916b3-349">bağlamı. Sonuç</span><span class="sxs-lookup"><span data-stu-id="916b3-349">context.Result</span></span>                       | <span data-ttu-id="916b3-350">Eylem</span><span class="sxs-lookup"><span data-stu-id="916b3-350">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="916b3-351">`RuleResult.ContinueRules` (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="916b3-351">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="916b3-352">Devam kuralları uygulama</span><span class="sxs-lookup"><span data-stu-id="916b3-352">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="916b3-353">Kuralları uygulanmasını durdurmak ve yanıtı gönder</span><span class="sxs-lookup"><span data-stu-id="916b3-353">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="916b3-354">Kuralları uygulanmasını durdurmak ve sonraki Ara yazılıma bağlamı gönderme</span><span class="sxs-lookup"><span data-stu-id="916b3-354">Stop applying rules and send the context to the next middleware</span></span> |

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

<span data-ttu-id="916b3-355">Örnek uygulama ile biten istekleri yolları için yönlendiren bir yöntemi gösterir *.xml*.</span><span class="sxs-lookup"><span data-stu-id="916b3-355">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="916b3-356">Bir istek yapıp yapmadığını `/file.xml`, onu yönlendireceği `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="916b3-356">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="916b3-357">Durum kodu 301 (kalıcı taşındı) ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="916b3-357">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="916b3-358">İçin bir yeniden yönlendirme, yanıtın durum kodu açıkça ayarlamalısınız; Aksi takdirde, bir 200 (Tamam) durum kodu döndürülür ve istemcide yeniden yönlendirme karşılaşılmaz.</span><span class="sxs-lookup"><span data-stu-id="916b3-358">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="916b3-359">Özgün istek: `/file.xml`</span><span class="sxs-lookup"><span data-stu-id="916b3-359">Original Request: `/file.xml`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların file.xml için izleme ile](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="916b3-361">Kural Irule tabanlı</span><span class="sxs-lookup"><span data-stu-id="916b3-361">IRule-based rule</span></span>

<span data-ttu-id="916b3-362">Kullanım `Add(IRule)` türetildiği bir sınıf kendi kural mantığı kullanmak `IRule`.</span><span class="sxs-lookup"><span data-stu-id="916b3-362">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="916b3-363">Kullanarak bir `IRule` yöntemi dayalı kural yaklaşımı kullanarak üzerinde daha fazla esneklik sağlar.</span><span class="sxs-lookup"><span data-stu-id="916b3-363">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="916b3-364">Burada, geçirebilirsiniz parametreleri için bir oluşturucu, türetilmiş sınıfınızın içerebilir `ApplyRule` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="916b3-364">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

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

<span data-ttu-id="916b3-365">Örnek uygulama için parametre değerlerini `extension` ve `newPath` çeşitli koşullara uyması için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="916b3-365">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="916b3-366">`extension` Bir değer içermelidir ve değer olmalıdır *.png*, *.jpg*, veya *.gif*.</span><span class="sxs-lookup"><span data-stu-id="916b3-366">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="916b3-367">Varsa `newPath` geçerli olmayan bir `ArgumentException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="916b3-367">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="916b3-368">Bir istek yapıp yapmadığını *image.png*, onu yönlendireceği `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="916b3-368">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="916b3-369">Bir istek yapıp yapmadığını *image.jpg*, onu yönlendireceği `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="916b3-369">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="916b3-370">Durum kodu 301 (kalıcı taşındı) olarak ayarlanır ve `context.Result` kural işlemeyi durdur ve yanıt göndermek için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="916b3-370">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="916b3-371">Özgün istek: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="916b3-371">Original Request: `/image.png`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların image.png için izleme ile](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="916b3-373">Özgün istek: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="916b3-373">Original Request: `/image.jpg`</span></span>

![Tarayıcının geliştirici araçları istek ve yanıtların image.jpg için izleme ile](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="916b3-375">Normal ifade örnekleri</span><span class="sxs-lookup"><span data-stu-id="916b3-375">Regex examples</span></span>

| <span data-ttu-id="916b3-376">Hedef</span><span class="sxs-lookup"><span data-stu-id="916b3-376">Goal</span></span> | <span data-ttu-id="916b3-377">Normal ifade dizesini &</span><span class="sxs-lookup"><span data-stu-id="916b3-377">Regex String &</span></span><br><span data-ttu-id="916b3-378">Eşleşme örneği</span><span class="sxs-lookup"><span data-stu-id="916b3-378">Match Example</span></span> | <span data-ttu-id="916b3-379">Değiştirme dizesi &</span><span class="sxs-lookup"><span data-stu-id="916b3-379">Replacement String &</span></span><br><span data-ttu-id="916b3-380">Çıkış örneği</span><span class="sxs-lookup"><span data-stu-id="916b3-380">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="916b3-381">Querystring yol yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="916b3-381">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="916b3-382">Şerit sonunda eğik çizgi</span><span class="sxs-lookup"><span data-stu-id="916b3-382">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="916b3-383">Eğik zorla</span><span class="sxs-lookup"><span data-stu-id="916b3-383">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="916b3-384">Belirli isteklere yeniden yazma kaçının</span><span class="sxs-lookup"><span data-stu-id="916b3-384">Avoid rewriting specific requests</span></span> | <span data-ttu-id="916b3-385">`^(.*)(?<!\.axd)$` veya `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="916b3-385">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="916b3-386">Evet: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="916b3-386">Yes: `/resource.htm`</span></span><br><span data-ttu-id="916b3-387">Hayır: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="916b3-387">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="916b3-388">URL kesimleri bunları yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="916b3-388">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="916b3-389">URL kesimi değiştirin</span><span class="sxs-lookup"><span data-stu-id="916b3-389">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="916b3-390">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="916b3-390">Additional resources</span></span>

* [<span data-ttu-id="916b3-391">Uygulama Başlatma</span><span class="sxs-lookup"><span data-stu-id="916b3-391">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="916b3-392">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="916b3-392">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="916b3-393">.NET içinde normal ifadeler</span><span class="sxs-lookup"><span data-stu-id="916b3-393">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="916b3-394">Normal ifade dili - hızlı başvuru</span><span class="sxs-lookup"><span data-stu-id="916b3-394">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="916b3-395">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="916b3-395">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="916b3-396">URL yeniden yazma modülü 2.0 (IIS) kullanma</span><span class="sxs-lookup"><span data-stu-id="916b3-396">Using Url Rewrite Module 2.0 (for IIS)</span></span>](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="916b3-397">URL yeniden yazma Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="916b3-397">URL Rewrite Module Configuration Reference</span></span>](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="916b3-398">IIS URL yeniden yazma modülü Forumu</span><span class="sxs-lookup"><span data-stu-id="916b3-398">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="916b3-399">Basit bir URL yapısı tutun</span><span class="sxs-lookup"><span data-stu-id="916b3-399">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="916b3-400">10 URL yeniden yazma, ipuçları ve püf noktaları</span><span class="sxs-lookup"><span data-stu-id="916b3-400">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="916b3-401">Eğik çizgi veya değil eğik çizgi</span><span class="sxs-lookup"><span data-stu-id="916b3-401">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
