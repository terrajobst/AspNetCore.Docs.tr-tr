---
title: URL yeniden yazma ASP.NET Core Ara
author: guardrex
description: URL yeniden yazma işlemi ve ASP.NET Core uygulamaları URL yeniden yazma işlemi Ara yazılımla yeniden yönlendirme hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.date: 08/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/url-rewriting
ms.openlocfilehash: b6465aa7b56450f43be64da19f2e2228a5d68f50
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="780f1-103">URL yeniden yazma ASP.NET Core Ara</span><span class="sxs-lookup"><span data-stu-id="780f1-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="780f1-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="780f1-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="780f1-105">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="780f1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="780f1-106">URL yeniden yazma işlemi bir veya daha fazla önceden tanımlanmış kurallar URL'leri dayalı isteği değiştirme işlemidir.</span><span class="sxs-lookup"><span data-stu-id="780f1-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="780f1-107">URL yeniden yazma işlemi bir Özet kaynak konumları ve adresleri arasında adreslerini ve konumları sıkı şekilde bağlı olmayan şekilde oluşturur.</span><span class="sxs-lookup"><span data-stu-id="780f1-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="780f1-108">URL yeniden yazma işlemi değerli olduğu birkaç senaryo vardır:</span><span class="sxs-lookup"><span data-stu-id="780f1-108">There are several scenarios where URL rewriting is valuable:</span></span>
* <span data-ttu-id="780f1-109">Taşıma veya bu kaynaklar için kararlı bulucular korurken sunucu kaynaklarını geçici veya kalıcı olarak değiştirme</span><span class="sxs-lookup"><span data-stu-id="780f1-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources</span></span>
* <span data-ttu-id="780f1-110">İstek farklı uygulamalar arasında veya bir uygulamanın alanları genelinde işleme bölme</span><span class="sxs-lookup"><span data-stu-id="780f1-110">Splitting request processing across different apps or across areas of one app</span></span>
* <span data-ttu-id="780f1-111">Kaldırma, ekleme ya da URL kesimleri gelen istekleri üzerinde yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="780f1-111">Removing, adding, or reorganizing URL segments on incoming requests</span></span>
* <span data-ttu-id="780f1-112">Ortak URL'ler arama motoru iyileştirme (SEO) için en iyi duruma getirme</span><span class="sxs-lookup"><span data-stu-id="780f1-112">Optimizing public URLs for Search Engine Optimization (SEO)</span></span>
* <span data-ttu-id="780f1-113">Bir bağlantıyı izleyerek bulacaksınız içerik tahmin kişilerin yardımcı olmak üzere ortak kolay URL'leri kullanımını erişimine izin verme</span><span class="sxs-lookup"><span data-stu-id="780f1-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link</span></span>
* <span data-ttu-id="780f1-114">Uç noktalarını güvenli hale getirmek için güvenli olmayan istekleri yönlendirme</span><span class="sxs-lookup"><span data-stu-id="780f1-114">Redirecting insecure requests to secure endpoints</span></span>
* <span data-ttu-id="780f1-115">Görüntü hotlinking önleme</span><span class="sxs-lookup"><span data-stu-id="780f1-115">Preventing image hotlinking</span></span>

<span data-ttu-id="780f1-116">Özel kural mantığı kullanarak birkaç yoldan URL değiştirme ve regex, Apache mod_rewrite modülü kuralları, IIS yeniden yazma modülü kuralları da dahil olmak üzere için kurallar tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="780f1-116">You can define rules for changing the URL in several ways, including regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="780f1-117">Bu belge, ASP.NET Core uygulamaları URL yeniden yazma işlemi Ara kullanma hakkında yönergeler içeren URL yeniden yazma işlemi sunar.</span><span class="sxs-lookup"><span data-stu-id="780f1-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="780f1-118">URL yeniden yazma işlemi bir uygulama performansını azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="780f1-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="780f1-119">Uygun yerlerde sayısı ve karmaşıklığı kuralları sınırlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="780f1-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="780f1-120">URL yeniden yönlendirme ve URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="780f1-120">URL redirect and URL rewrite</span></span>
<span data-ttu-id="780f1-121">İfade arasındaki farkı *URL yeniden yönlendirme* ve *URL yeniden yazma* en ince görünebilir ilk ancak istemciler için kaynaklar sağlamak için önemli etkileri vardır.</span><span class="sxs-lookup"><span data-stu-id="780f1-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="780f1-122">ASP.NET Core'nin URL yeniden yazma işlemi Ara gereken her ikisi için de toplantı yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="780f1-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="780f1-123">A *URL yeniden yönlendirme* istemci burada belirtildiği başka bir adresinde bir kaynağa erişmek için bir istemci tarafı işlemi olduğu.</span><span class="sxs-lookup"><span data-stu-id="780f1-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="780f1-124">Bu sunucuya gidiş gerektirir.</span><span class="sxs-lookup"><span data-stu-id="780f1-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="780f1-125">İstemci kaynak için yeni bir istek yaptığında, istemciye döndürülen yeniden yönlendirme URL'si tarayıcının adres çubuğunda görünür.</span><span class="sxs-lookup"><span data-stu-id="780f1-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="780f1-126">Varsa `/resource` olan *yeniden yönlendirilen* için `/different-resource`, istemci isteklerini `/resource`.</span><span class="sxs-lookup"><span data-stu-id="780f1-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="780f1-127">İstemci kaynak edinmelidir sunucunun yanıt `/different-resource` yeniden yönlendirme geçici veya kalıcı olduğunu belirten bir durum kodu ile.</span><span class="sxs-lookup"><span data-stu-id="780f1-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="780f1-128">İstemci yeniden yönlendirme URL'sini kaynak için yeni bir isteği yürütür.</span><span class="sxs-lookup"><span data-stu-id="780f1-128">The client executes a new request for the resource at the redirect URL.</span></span>

![Webapı hizmet uç noktası sunucusundaki sürüm 2 (v2) için sürüm 1 (v1) geçici olarak değiştirildi.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="780f1-134">İstekleri için farklı bir URL yeniden yönlendirme, yeniden yönlendirme kalıcı veya geçici olup olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="780f1-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="780f1-135">301 (taşınmış kalıcı olarak) durum kodunu burada kaynak yeni, kalıcı bir URL'ye sahip ve istediğiniz tüm gelecekteki isteklerin kaynak için yeni URL kullanmalısınız istemci istemek üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="780f1-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="780f1-136">*301 durum kodu alındığında istemci yanıt önbelleğe alabilir.*</span><span class="sxs-lookup"><span data-stu-id="780f1-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="780f1-137">302 (bulundu) durum kodu, yeniden yönlendirme geçici veya genellikle konu olduğu istemci depolamak ve yeniden yönlendirme URL'sini gelecekte tekrar döndürmemelidir şekilde değiştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="780f1-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="780f1-138">Daha fazla bilgi için bkz: [RFC 2616: durum kodu tanımları](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="780f1-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="780f1-139">A *URL yeniden yazma* farklı bir kaynak adresinden kaynak sağlamak için sunucu tarafı bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="780f1-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="780f1-140">Bir URL yeniden yazma işlemi sunucuya gidiş gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="780f1-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="780f1-141">Yeniden URL istemciye döndürülen değil ve tarayıcının adres çubuğunda görünmez.</span><span class="sxs-lookup"><span data-stu-id="780f1-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="780f1-142">Zaman `/resource` olan *yeniden yazılmıştır* için `/different-resource`, istemci isteklerini `/resource`ve sunucu *dahili olarak* kaynak getirir `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="780f1-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="780f1-143">İstemci yeniden URL'sindeki kaynak alamadı olabilir, ancak istemci, isteği yapar ve yanıtı aldığında kaynağın yeniden URL'de mevcut haberdar olmaz.</span><span class="sxs-lookup"><span data-stu-id="780f1-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Webapı hizmet uç noktası sürüm 1 (v1) sürüm 2 (v2) sunucu üzerinde değiştirildi.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="780f1-148">URL yazmaksızın örnek uygulaması</span><span class="sxs-lookup"><span data-stu-id="780f1-148">URL rewriting sample app</span></span>
<span data-ttu-id="780f1-149">URL yeniden yazma işlemi Ara yazılımla özelliklerini keşfedebilirsiniz [URL yazmaksızın örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span><span class="sxs-lookup"><span data-stu-id="780f1-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="780f1-150">Uygulama yeniden uygular ve yeniden yönlendirme kuralları ve yeniden veya yeniden yönlendirilmiş bir URL gösterir.</span><span class="sxs-lookup"><span data-stu-id="780f1-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="780f1-151">URL yeniden yazma işlemi Ara kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="780f1-151">When to use URL Rewriting Middleware</span></span>
<span data-ttu-id="780f1-152">URL yeniden yazma işlemi Ara kullanamadı olduğunda kullanın [URL yeniden yazma Modülü](https://www.iis.net/downloads/microsoft/url-rewrite) Windows Server'da IIS ile [Apache mod_rewrite Modülü](https://httpd.apache.org/docs/2.4/rewrite/) Apache sunucuda [NginxüzerindeURLyenidenyazmaişlemi](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), veya üzerinde barındırılan bir uygulamanızı [HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="780f1-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="780f1-153">Sunucu tabanlı URL yazmaksızın teknolojileri IIS, Apache veya Nginx kullanmak için ana ara yazılım bu modülleri tüm özelliklerini desteklemeyen ve ara yazılım performansını büyük olasılıkla, modüllerin eşleşmeyecektir nedenleridir.</span><span class="sxs-lookup"><span data-stu-id="780f1-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="780f1-154">Ancak, bazı özellikler ASP.NET Core projelerle gibi çalışmıyor sunucu modüllerinin vardır `IsFile` ve `IsDirectory` IIS yeniden yazma modülü kısıtlamaları.</span><span class="sxs-lookup"><span data-stu-id="780f1-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="780f1-155">Bu senaryolarda, ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="780f1-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="780f1-156">Paket</span><span class="sxs-lookup"><span data-stu-id="780f1-156">Package</span></span>
<span data-ttu-id="780f1-157">Ara yazılım projenize eklemek için bir başvuru ekleyin [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) paket.</span><span class="sxs-lookup"><span data-stu-id="780f1-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="780f1-158">Bu özellik veya üstünü ASP.NET Core 1.1 hedef uygulamaları için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="780f1-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="780f1-159">Uzantı ve seçenekleri</span><span class="sxs-lookup"><span data-stu-id="780f1-159">Extension and options</span></span>
<span data-ttu-id="780f1-160">URL yeniden yazma kurmak ve kuralları bir örneğini oluşturarak yeniden yönlendirme `RewriteOptions` sınıfı, kuralların her biri için genişletme yöntemleri ile.</span><span class="sxs-lookup"><span data-stu-id="780f1-160">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="780f1-161">Birden çok kural bunları işlenen istediğiniz sırayla zincir.</span><span class="sxs-lookup"><span data-stu-id="780f1-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="780f1-162">`RewriteOptions` İle istek ardışık düzenine eklenen URL yeniden yazma işlemi Ara geçirilir `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="780f1-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="780f1-163">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="780f1-163">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="780f1-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="780f1-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
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

* * *
### <a name="url-redirect"></a><span data-ttu-id="780f1-165">URL yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="780f1-165">URL redirect</span></span>
<span data-ttu-id="780f1-166">Kullanım `AddRedirect` isteklerini yeniden yönlendirmek için.</span><span class="sxs-lookup"><span data-stu-id="780f1-166">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="780f1-167">İlk parametre gelen URL yolda eşleştirmek için regex içerir.</span><span class="sxs-lookup"><span data-stu-id="780f1-167">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="780f1-168">İkinci parametre değiştirme dizedir.</span><span class="sxs-lookup"><span data-stu-id="780f1-168">The second parameter is the replacement string.</span></span> <span data-ttu-id="780f1-169">Üçüncü parametresi, varsa, durum kodu belirtir.</span><span class="sxs-lookup"><span data-stu-id="780f1-169">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="780f1-170">Durum kodu belirtmezseniz, kaynak geçici olarak değiştirilen veya taşındığında olduğunu gösteren 302 (bulundu) varsayar.</span><span class="sxs-lookup"><span data-stu-id="780f1-170">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="780f1-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="780f1-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="780f1-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="780f1-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

* * *
<span data-ttu-id="780f1-173">Geliştirici Araçları etkin bir tarayıcı yoluyla örnek uygulama için istekte `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="780f1-173">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="780f1-174">Regex üzerinde istek yolunun eşleşmesi `redirect-rule/(.*)`, ve yolu ile değiştirilir `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="780f1-174">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="780f1-175">Yeniden yönlendirme URL'si 302 (bulundu) durum kodlu istemciye geri gönderilir.</span><span class="sxs-lookup"><span data-stu-id="780f1-175">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="780f1-176">Tarayıcı, tarayıcının adres çubuğunda görünür yeniden yönlendirme URL'sinde yeni isteğinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="780f1-176">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="780f1-177">Yeniden yönlendirme URL'sini eşleşen kural için örnek uygulama olduğundan, ikinci isteği 200 (Tamam) yanıt uygulamadan alır ve yanıtın gövdesini yeniden yönlendirme URL'sini gösterir.</span><span class="sxs-lookup"><span data-stu-id="780f1-177">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="780f1-178">Bir URL olduğunda bir geri dönüş sunucuya yapılan *yeniden yönlendirilen*.</span><span class="sxs-lookup"><span data-stu-id="780f1-178">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="780f1-179">Yeniden yönlendirme kurallarınızı oluştururken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="780f1-179">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="780f1-180">Yeniden yönlendirme kuralları her istekte sonra bir yeniden yönlendirme dahil olmak üzere uygulama değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="780f1-180">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="780f1-181">Yanlışlıkla için kolayca sonsuz yeniden yönlendirmeleri döngüsüne oluşturun.</span><span class="sxs-lookup"><span data-stu-id="780f1-181">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="780f1-182">Özgün istek: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="780f1-182">Original Request: `/redirect-rule/1234/5678`</span></span>

![Geliştirici isteklerini ve yanıtlarını izleme araçları ile bir tarayıcı penceresi](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="780f1-184">Parantez içinde yer alan ifade parçası olarak adlandırılan bir *yakalama grup*.</span><span class="sxs-lookup"><span data-stu-id="780f1-184">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="780f1-185">Nokta (`.`) bir ifade anlamına gelir *herhangi bir karakteri eşleştirmek*.</span><span class="sxs-lookup"><span data-stu-id="780f1-185">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="780f1-186">Yıldız işareti (`*`) gösteren *sıfır veya daha fazla kez önceki karakteri eşleştirmek*.</span><span class="sxs-lookup"><span data-stu-id="780f1-186">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="780f1-187">Bu nedenle, son iki yol kesimleri URL'sinin `1234/5678`, yakalama grubu tarafından yakalanan `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="780f1-187">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="780f1-188">Sağladığınız istek URL'sindeki sonra herhangi bir değer `redirect-rule/` bu tek yakalama grubu tarafından kapsanır.</span><span class="sxs-lookup"><span data-stu-id="780f1-188">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="780f1-189">Dolar işareti ile dizesine eklenen değiştirme dizesini yakalanan gruplar (`$`) yakalama sıra numarası tarafından izlenen.</span><span class="sxs-lookup"><span data-stu-id="780f1-189">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="780f1-190">İlk yakalama grup değeri ile elde edilen `$1`, ikinci ile `$2`, ve, regex yakalama grupları için sırayla devam eder.</span><span class="sxs-lookup"><span data-stu-id="780f1-190">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="780f1-191">Yoktur tek yakalanan grubu içinde yeniden yönlendirme kuralı regex örnek uygulamasında olduğu değiştirme dizesi içinde yalnızca bir eklenen Grup şekilde `$1`.</span><span class="sxs-lookup"><span data-stu-id="780f1-191">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="780f1-192">Kural uygulandığında, URL hale `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="780f1-192">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="780f1-193">Güvenli bir uç noktası için URL yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="780f1-193">URL redirect to a secure endpoint</span></span>
<span data-ttu-id="780f1-194">Kullanım `AddRedirectToHttps` aynı ana bilgisayar ve HTTPS kullanarak yolu HTTP isteklerini yeniden yönlendirmek için (`https://`).</span><span class="sxs-lookup"><span data-stu-id="780f1-194">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="780f1-195">Durum kodu sağlanan değil, ara yazılım 302 (bulundu) varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="780f1-195">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="780f1-196">Bağlantı noktası değil sağlandıysa ara yazılım için varsayılan olarak `null`, protokol başka bir deyişle, değişikliklerini `https://` ve istemci kaynak bağlantı noktası 443 üzerinden erişir.</span><span class="sxs-lookup"><span data-stu-id="780f1-196">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="780f1-197">Örneğin, durum kodu 301 (taşınmış kalıcı olarak) ve bağlantı noktası için 5001 değiştirmek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="780f1-197">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="780f1-198">Kullanım `AddRedirectToHttpsPermanent` aynı konak ve yol güvenli HTTPS protokolü ile güvenli isteklerini yeniden yönlendirmek için (`https://` bağlantı noktası 443'tür).</span><span class="sxs-lookup"><span data-stu-id="780f1-198">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="780f1-199">Ara yazılım durum kodu 301 (taşınmış kalıcı olarak) ayarlar.</span><span class="sxs-lookup"><span data-stu-id="780f1-199">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

<span data-ttu-id="780f1-200">Örnek uygulamayı nasıl kullanılacağını gösteren özellikli `AddRedirectToHttps` veya `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="780f1-200">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="780f1-201">Add genişletme yöntemi `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="780f1-201">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="780f1-202">Tüm URL'deki uygulamaya güvenli olmayan bir isteği oluşturun.</span><span class="sxs-lookup"><span data-stu-id="780f1-202">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="780f1-203">Tarayıcı güvenlik otomatik olarak imzalanan sertifika güvenilmeyen uyarısı yok sayın.</span><span class="sxs-lookup"><span data-stu-id="780f1-203">Dismiss the browser security warning that the self-signed certificate is untrusted.</span></span>

<span data-ttu-id="780f1-204">Özgün istek kullanarak `AddRedirectToHttps(301, 5001)`: `/secure`</span><span class="sxs-lookup"><span data-stu-id="780f1-204">Original Request using `AddRedirectToHttps(301, 5001)`: `/secure`</span></span>

![Geliştirici isteklerini ve yanıtlarını izleme araçları ile bir tarayıcı penceresi](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="780f1-206">Özgün istek kullanarak `AddRedirectToHttpsPermanent`: `/secure`</span><span class="sxs-lookup"><span data-stu-id="780f1-206">Original Request using `AddRedirectToHttpsPermanent`: `/secure`</span></span>

![Geliştirici isteklerini ve yanıtlarını izleme araçları ile bir tarayıcı penceresi](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="780f1-208">URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="780f1-208">URL rewrite</span></span>
<span data-ttu-id="780f1-209">Kullanım `AddRewrite` URL yeniden yazma işlemi için bir kural oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="780f1-209">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="780f1-210">İlk parametre gelen URL yolda eşleştirmek için regex içerir.</span><span class="sxs-lookup"><span data-stu-id="780f1-210">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="780f1-211">İkinci parametre değiştirme dizedir.</span><span class="sxs-lookup"><span data-stu-id="780f1-211">The second parameter is the replacement string.</span></span> <span data-ttu-id="780f1-212">Üçüncü parametre `skipRemainingRules: {true|false}`, ara yazılımı için geçerli kural uyguladıysanız ek yeniden yazma kuralları Atla gerekip gerekmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="780f1-212">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="780f1-213">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="780f1-213">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="780f1-214">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="780f1-214">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

* * *
<span data-ttu-id="780f1-215">Özgün istek: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="780f1-215">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Geliştirici Araçları istek ve yanıt izleme ile bir tarayıcı penceresi](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="780f1-217">Regex fark ilk ayar şeydir (`^`) ifade başındaki.</span><span class="sxs-lookup"><span data-stu-id="780f1-217">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="780f1-218">Başka bir deyişle, eşleşen URL yolunu başında başlar.</span><span class="sxs-lookup"><span data-stu-id="780f1-218">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="780f1-219">Yeniden yönlendirme kuralı ile önceki örnekte `redirect-rule/(.*)`, hiçbir ayar regex başlangıcında; bu nedenle, herhangi bir karakter koyun `redirect-rule/` yolunda başarılı bir eşleşme.</span><span class="sxs-lookup"><span data-stu-id="780f1-219">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="780f1-220">Yol</span><span class="sxs-lookup"><span data-stu-id="780f1-220">Path</span></span>                               | <span data-ttu-id="780f1-221">Eşleştirme</span><span class="sxs-lookup"><span data-stu-id="780f1-221">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="780f1-222">Evet</span><span class="sxs-lookup"><span data-stu-id="780f1-222">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="780f1-223">Evet</span><span class="sxs-lookup"><span data-stu-id="780f1-223">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="780f1-224">Evet</span><span class="sxs-lookup"><span data-stu-id="780f1-224">Yes</span></span>   |

<span data-ttu-id="780f1-225">Yeniden yazma kuralı `^rewrite-rule/(\d+)/(\d+)`, ile başlatırsanız yollar yalnızca eşleşen `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="780f1-225">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="780f1-226">Aşağıdaki yeniden yazma kuralı ve yukarıdaki yeniden yönlendirme kuralı arasında eşleştirme içinde fark.</span><span class="sxs-lookup"><span data-stu-id="780f1-226">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="780f1-227">Yol</span><span class="sxs-lookup"><span data-stu-id="780f1-227">Path</span></span>                              | <span data-ttu-id="780f1-228">Eşleştirme</span><span class="sxs-lookup"><span data-stu-id="780f1-228">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="780f1-229">Evet</span><span class="sxs-lookup"><span data-stu-id="780f1-229">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="780f1-230">Hayır</span><span class="sxs-lookup"><span data-stu-id="780f1-230">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="780f1-231">Hayır</span><span class="sxs-lookup"><span data-stu-id="780f1-231">No</span></span>    |

<span data-ttu-id="780f1-232">Aşağıdaki `^rewrite-rule/` bölümü bir ifade, iki yakalama grubu vardır `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="780f1-232">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="780f1-233">`\d` Güveninin *bir rakam (sayı) eşleşen*.</span><span class="sxs-lookup"><span data-stu-id="780f1-233">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="780f1-234">Artı işareti (`+`) anlamına gelir *bir veya daha önceki karakteri eşleştirmek*.</span><span class="sxs-lookup"><span data-stu-id="780f1-234">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="780f1-235">Bu nedenle, URL bir-başka bir sayının eğik çizgi ve ardından bir sayı içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="780f1-235">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="780f1-236">Gruplar halinde yeniden URL'si olarak eklenen bu yakalama `$1` ve `$2`.</span><span class="sxs-lookup"><span data-stu-id="780f1-236">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="780f1-237">Yeniden yazma kuralı değiştirme dizesini yakalanan gruplar querystring yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="780f1-237">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="780f1-238">İstenen yolunu `/rewrite-rule/1234/5678` kaynak elde etmek için yeniden yazılmıştır `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="780f1-238">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="780f1-239">Bir sorgu dizesi özgün istekte mevcut ise, URL yeniden yazılmıştır, korunur.</span><span class="sxs-lookup"><span data-stu-id="780f1-239">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="780f1-240">Kaynak elde etmek için sunucuya hiçbir gidiş dönüş yoktur.</span><span class="sxs-lookup"><span data-stu-id="780f1-240">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="780f1-241">Kaynağın mevcut değilse, getirilen ve 200 (Tamam) durum kodlu istemciye döndürülen.</span><span class="sxs-lookup"><span data-stu-id="780f1-241">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="780f1-242">İstemci yeniden yönlendirilen değil çünkü tarayıcı adres çubuğundaki URL'yi değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="780f1-242">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="780f1-243">İstemci ilgili oldukça hiçbir zaman URL yeniden yazma işlemi oluştu.</span><span class="sxs-lookup"><span data-stu-id="780f1-243">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="780f1-244">Kullanım `skipRemainingRules: true` mümkün olduğunda, çünkü eşleştirme kuralları pahalı bir işlemdir ve uygulama yanıt süresini azaltır.</span><span class="sxs-lookup"><span data-stu-id="780f1-244">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="780f1-245">Hızlı uygulama yanıt için:</span><span class="sxs-lookup"><span data-stu-id="780f1-245">For the fastest app response:</span></span>
> * <span data-ttu-id="780f1-246">En sık eşleşen kural, yeniden yazma kuralları az sık eşleşen kural sipariş.</span><span class="sxs-lookup"><span data-stu-id="780f1-246">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="780f1-247">Bir eşleşme olursa ve hiçbir ek kural işlenirken gerekli olduğunda kalan kurallarının işlenmesini atlayın.</span><span class="sxs-lookup"><span data-stu-id="780f1-247">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="780f1-248">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="780f1-248">Apache mod_rewrite</span></span>
<span data-ttu-id="780f1-249">Apache mod_rewrite kurallarıyla uygulamak `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="780f1-249">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="780f1-250">Kural dosyasının uygulamayla dağıtıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="780f1-250">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="780f1-251">Daha fazla bilgi ve mod_rewrite kuralları örnekleri için bkz: [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="780f1-251">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="780f1-252">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="780f1-252">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="780f1-253">A `StreamReader` kurallardan okumak için kullanılan *ApacheModRewrite.txt* kurallar dosyası.</span><span class="sxs-lookup"><span data-stu-id="780f1-253">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="780f1-254">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="780f1-254">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="780f1-255">İlk parametre alan bir `IFileProvider`, aracılığıyla sağlanan [bağımlılık ekleme](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="780f1-255">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="780f1-256">`IHostingEnvironment` Sağlamak için eklenen `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="780f1-256">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="780f1-257">İkinci parametre olan kurallar dosyanızı yoludur *ApacheModRewrite.txt* örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="780f1-257">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

* * *
<span data-ttu-id="780f1-258">Örnek uygulama isteklerinden yönlendiren `/apache-mod-rules-redirect/(.\*)` için `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="780f1-258">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="780f1-259">Yanıt durum kodu 302 (bulundu) ' dir.</span><span class="sxs-lookup"><span data-stu-id="780f1-259">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="780f1-260">Özgün istek: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="780f1-260">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Geliştirici isteklerini ve yanıtlarını izleme araçları ile bir tarayıcı penceresi](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="780f1-262">Desteklenen sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="780f1-262">Supported server variables</span></span>
<span data-ttu-id="780f1-263">Ara yazılım aşağıdaki Apache mod_rewrite sunucu değişkenlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="780f1-263">The middleware supports the following Apache mod_rewrite server variables:</span></span>
* <span data-ttu-id="780f1-264">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="780f1-264">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="780f1-265">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="780f1-265">HTTP_ACCEPT</span></span>
* <span data-ttu-id="780f1-266">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="780f1-266">HTTP_CONNECTION</span></span>
* <span data-ttu-id="780f1-267">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="780f1-267">HTTP_COOKIE</span></span>
* <span data-ttu-id="780f1-268">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="780f1-268">HTTP_FORWARDED</span></span>
* <span data-ttu-id="780f1-269">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="780f1-269">HTTP_HOST</span></span>
* <span data-ttu-id="780f1-270">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="780f1-270">HTTP_REFERER</span></span>
* <span data-ttu-id="780f1-271">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="780f1-271">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="780f1-272">HTTPS</span><span class="sxs-lookup"><span data-stu-id="780f1-272">HTTPS</span></span>
* <span data-ttu-id="780f1-273">IPV6</span><span class="sxs-lookup"><span data-stu-id="780f1-273">IPV6</span></span>
* <span data-ttu-id="780f1-274">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="780f1-274">QUERY_STRING</span></span>
* <span data-ttu-id="780f1-275">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="780f1-275">REMOTE_ADDR</span></span>
* <span data-ttu-id="780f1-276">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="780f1-276">REMOTE_PORT</span></span>
* <span data-ttu-id="780f1-277">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="780f1-277">REQUEST_FILENAME</span></span>
* <span data-ttu-id="780f1-278">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="780f1-278">REQUEST_METHOD</span></span>
* <span data-ttu-id="780f1-279">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="780f1-279">REQUEST_SCHEME</span></span>
* <span data-ttu-id="780f1-280">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="780f1-280">REQUEST_URI</span></span>
* <span data-ttu-id="780f1-281">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="780f1-281">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="780f1-282">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="780f1-282">SERVER_ADDR</span></span>
* <span data-ttu-id="780f1-283">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="780f1-283">SERVER_PORT</span></span>
* <span data-ttu-id="780f1-284">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="780f1-284">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="780f1-285">SAAT</span><span class="sxs-lookup"><span data-stu-id="780f1-285">TIME</span></span>
* <span data-ttu-id="780f1-286">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="780f1-286">TIME_DAY</span></span>
* <span data-ttu-id="780f1-287">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="780f1-287">TIME_HOUR</span></span>
* <span data-ttu-id="780f1-288">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="780f1-288">TIME_MIN</span></span>
* <span data-ttu-id="780f1-289">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="780f1-289">TIME_MON</span></span>
* <span data-ttu-id="780f1-290">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="780f1-290">TIME_SEC</span></span>
* <span data-ttu-id="780f1-291">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="780f1-291">TIME_WDAY</span></span>
* <span data-ttu-id="780f1-292">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="780f1-292">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="780f1-293">IIS URL yeniden yazma modülü kuralları</span><span class="sxs-lookup"><span data-stu-id="780f1-293">IIS URL Rewrite Module rules</span></span>
<span data-ttu-id="780f1-294">IIS URL yeniden yazma modülü için geçerli bir kurallar kullanmak için `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="780f1-294">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="780f1-295">Kural dosyasının uygulamayla dağıtıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="780f1-295">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="780f1-296">Kullanılacak Ara yazılımının doğrudan yok, *web.config* Windows Server IIS üzerinde çalışırken dosya.</span><span class="sxs-lookup"><span data-stu-id="780f1-296">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="780f1-297">IIS ile bu kuralları dışında depolanması gereken, *web.config* IIS yeniden yazma modülü ile çakışmaları önlemek için.</span><span class="sxs-lookup"><span data-stu-id="780f1-297">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="780f1-298">Daha fazla bilgi ve IIS URL yeniden yazma modülü kuralları örnekleri için bkz: [Url yeniden yazma modülü 2.0 kullanarak](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) ve [URL yeniden yazma modülü yapılandırma başvurusu](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="780f1-298">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="780f1-299">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="780f1-299">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="780f1-300">A `StreamReader` kurallardan okumak için kullanılan *IISUrlRewrite.xml* kurallar dosyası.</span><span class="sxs-lookup"><span data-stu-id="780f1-300">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="780f1-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="780f1-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="780f1-302">İlk parametre alan bir `IFileProvider`, ikinci parametre olan XML kuralları dosyanızın yolu, *IISUrlRewrite.xml* örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="780f1-302">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

* * *
<span data-ttu-id="780f1-303">Örnek uygulaması isteklerden yeniden yazar `/iis-rules-rewrite/(.*)` için `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="780f1-303">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="780f1-304">200 (Tamam) durum kodlu istemciye gönderilen yanıtı.</span><span class="sxs-lookup"><span data-stu-id="780f1-304">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="780f1-305">Özgün istek: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="780f1-305">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Geliştirici Araçları istek ve yanıt izleme ile bir tarayıcı penceresi](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="780f1-307">Uygulamanızı istenmeyen şekilde etkileyebilecek yapılandırılmış sunucu düzeyi kurallarıyla etkin bir IIS yeniden yazma modülü varsa, IIS yeniden yazma modülü bir uygulama için devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="780f1-307">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="780f1-308">Daha fazla bilgi için bkz: [devre dışı bırakma IIS modüllerini](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="780f1-308">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="780f1-309">Desteklenmeyen özellikler</span><span class="sxs-lookup"><span data-stu-id="780f1-309">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="780f1-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="780f1-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="780f1-311">Yayımlanan ara yazılımı ile ASP.NET Core 2.x aşağıdaki IIS URL yeniden yazma modülü özellikleri desteklemez:</span><span class="sxs-lookup"><span data-stu-id="780f1-311">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="780f1-312">Giden kuralları</span><span class="sxs-lookup"><span data-stu-id="780f1-312">Outbound Rules</span></span>
* <span data-ttu-id="780f1-313">Özel sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="780f1-313">Custom Server Variables</span></span>
* <span data-ttu-id="780f1-314">Joker karakterler</span><span class="sxs-lookup"><span data-stu-id="780f1-314">Wildcards</span></span>
* <span data-ttu-id="780f1-315">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="780f1-315">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="780f1-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="780f1-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="780f1-317">Yayımlanan ara yazılımı ile ASP.NET Core 1.x aşağıdaki IIS URL yeniden yazma modülü özellikleri desteklemez:</span><span class="sxs-lookup"><span data-stu-id="780f1-317">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="780f1-318">Genel kurallar</span><span class="sxs-lookup"><span data-stu-id="780f1-318">Global Rules</span></span>
* <span data-ttu-id="780f1-319">Giden kuralları</span><span class="sxs-lookup"><span data-stu-id="780f1-319">Outbound Rules</span></span>
* <span data-ttu-id="780f1-320">MAPS yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="780f1-320">Rewrite Maps</span></span>
* <span data-ttu-id="780f1-321">CustomResponse eylemi</span><span class="sxs-lookup"><span data-stu-id="780f1-321">CustomResponse Action</span></span>
* <span data-ttu-id="780f1-322">Özel sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="780f1-322">Custom Server Variables</span></span>
* <span data-ttu-id="780f1-323">Joker karakterler</span><span class="sxs-lookup"><span data-stu-id="780f1-323">Wildcards</span></span>
* <span data-ttu-id="780f1-324">Action:CustomResponse</span><span class="sxs-lookup"><span data-stu-id="780f1-324">Action:CustomResponse</span></span>
* <span data-ttu-id="780f1-325">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="780f1-325">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="780f1-326">Desteklenen sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="780f1-326">Supported server variables</span></span>
<span data-ttu-id="780f1-327">Ara yazılım aşağıdaki IIS URL yeniden yazma modülünü sunucu değişkenleri destekler:</span><span class="sxs-lookup"><span data-stu-id="780f1-327">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>
* <span data-ttu-id="780f1-328">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="780f1-328">CONTENT_LENGTH</span></span>
* <span data-ttu-id="780f1-329">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="780f1-329">CONTENT_TYPE</span></span>
* <span data-ttu-id="780f1-330">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="780f1-330">HTTP_ACCEPT</span></span>
* <span data-ttu-id="780f1-331">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="780f1-331">HTTP_CONNECTION</span></span>
* <span data-ttu-id="780f1-332">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="780f1-332">HTTP_COOKIE</span></span>
* <span data-ttu-id="780f1-333">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="780f1-333">HTTP_HOST</span></span>
* <span data-ttu-id="780f1-334">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="780f1-334">HTTP_REFERER</span></span>
* <span data-ttu-id="780f1-335">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="780f1-335">HTTP_URL</span></span>
* <span data-ttu-id="780f1-336">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="780f1-336">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="780f1-337">HTTPS</span><span class="sxs-lookup"><span data-stu-id="780f1-337">HTTPS</span></span>
* <span data-ttu-id="780f1-338">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="780f1-338">LOCAL_ADDR</span></span>
* <span data-ttu-id="780f1-339">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="780f1-339">QUERY_STRING</span></span>
* <span data-ttu-id="780f1-340">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="780f1-340">REMOTE_ADDR</span></span>
* <span data-ttu-id="780f1-341">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="780f1-341">REMOTE_PORT</span></span>
* <span data-ttu-id="780f1-342">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="780f1-342">REQUEST_FILENAME</span></span>
* <span data-ttu-id="780f1-343">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="780f1-343">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="780f1-344">Edinebilirsiniz bir `IFileProvider` aracılığıyla bir `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="780f1-344">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="780f1-345">Bu yaklaşım, yeniden yazma konumu için daha fazla esneklik kuralları dosyaları sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="780f1-345">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="780f1-346">Sağladığınız yolun sunucusuna yeniden yazma kuralları dosyalarınızı dağıtılan emin olun.</span><span class="sxs-lookup"><span data-stu-id="780f1-346">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="780f1-347">Yöntem temelli kuralı</span><span class="sxs-lookup"><span data-stu-id="780f1-347">Method-based rule</span></span>
<span data-ttu-id="780f1-348">Kullanım `Add(Action<RewriteContext> applyRule)` bir yöntem kendi kural mantığı uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="780f1-348">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="780f1-349">`RewriteContext` Sunan `HttpContext` yönteminizi kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="780f1-349">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="780f1-350">`context.Result` Nasıl ek ardışık düzen belirler işleme işlenir.</span><span class="sxs-lookup"><span data-stu-id="780f1-350">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="780f1-351">bağlamı. Sonuç</span><span class="sxs-lookup"><span data-stu-id="780f1-351">context.Result</span></span>                       | <span data-ttu-id="780f1-352">Eylem</span><span class="sxs-lookup"><span data-stu-id="780f1-352">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="780f1-353">`RuleResult.ContinueRules` (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="780f1-353">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="780f1-354">Kuralları uygulama devam</span><span class="sxs-lookup"><span data-stu-id="780f1-354">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="780f1-355">Kuralları uygulanmasını durdurmak ve yanıtı gönder</span><span class="sxs-lookup"><span data-stu-id="780f1-355">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="780f1-356">Kuralları uygulanmasını durdurmak ve bağlam için bir sonraki ara yazılım gönderin</span><span class="sxs-lookup"><span data-stu-id="780f1-356">Stop applying rules and send the context to the next middleware</span></span> |

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="780f1-357">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="780f1-357">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="780f1-358">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="780f1-358">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

* * *
<span data-ttu-id="780f1-359">Örnek uygulama ile biten istekleri yollar için yönlendiren bir yöntem gösterilmektedir *.xml*.</span><span class="sxs-lookup"><span data-stu-id="780f1-359">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="780f1-360">Bir istek yapıyorsa verilen `/file.xml`, onu yönlendireceği `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="780f1-360">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="780f1-361">Durum kodu 301 (taşınmış kalıcı olarak) ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="780f1-361">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="780f1-362">İçin bir yeniden yönlendirme, açıkça yanıtın durum kodu ayarlamanız gerekir; Aksi takdirde, 200 (Tamam) durum kodu döndürülür ve yeniden yönlendirme istemcide karşılaşılmaz.</span><span class="sxs-lookup"><span data-stu-id="780f1-362">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="780f1-363">Özgün istek: `/file.xml`</span><span class="sxs-lookup"><span data-stu-id="780f1-363">Original Request: `/file.xml`</span></span>

![Geliştirici isteklerin ve yanıtların file.xml için izleme araçları ile bir tarayıcı penceresi](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="780f1-365">IRule tabanlı kuralı</span><span class="sxs-lookup"><span data-stu-id="780f1-365">IRule-based rule</span></span>
<span data-ttu-id="780f1-366">Kullanım `Add(IRule)` türeyen bir sınıf kendi kural mantığı uygulamak için `IRule`.</span><span class="sxs-lookup"><span data-stu-id="780f1-366">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="780f1-367">Kullanarak bir `IRule` yöntemi tabanlı kural yaklaşım kullanarak üzerinde daha fazla esneklik sağlar.</span><span class="sxs-lookup"><span data-stu-id="780f1-367">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="780f1-368">Burada, iletebilir parametrelerde bir oluşturucu, türetilmiş bir sınıf içerebilir `ApplyRule` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="780f1-368">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="780f1-369">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="780f1-369">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="780f1-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="780f1-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

* * *
<span data-ttu-id="780f1-371">Örnek uygulama için parametre değerlerini `extension` ve `newPath` birkaç koşullara uyan denetlenir.</span><span class="sxs-lookup"><span data-stu-id="780f1-371">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="780f1-372">`extension` Bir değer içermelidir ve değeri olmalıdır *.png*, *.jpg*, veya *.gif*.</span><span class="sxs-lookup"><span data-stu-id="780f1-372">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="780f1-373">Varsa `newPath` geçerli olmayan bir `ArgumentException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="780f1-373">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="780f1-374">Bir istek yapıyorsa verilen *image.png*, onu yönlendireceği `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="780f1-374">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="780f1-375">Bir istek yapıyorsa verilen *image.jpg*, onu yönlendireceği `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="780f1-375">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="780f1-376">Durum kodu 301 (kalıcı taşınmış) olarak ayarlanır ve `context.Result` kural işlemeyi durdur ve yanıt göndermek için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="780f1-376">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="780f1-377">Özgün istek: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="780f1-377">Original Request: `/image.png`</span></span>

![Geliştirici isteklerin ve yanıtların image.png için izleme araçları ile bir tarayıcı penceresi](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="780f1-379">Özgün istek: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="780f1-379">Original Request: `/image.jpg`</span></span>

![Geliştirici isteklerin ve yanıtların image.jpg için izleme araçları ile bir tarayıcı penceresi](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="780f1-381">Regex örnekleri</span><span class="sxs-lookup"><span data-stu-id="780f1-381">Regex examples</span></span>

| <span data-ttu-id="780f1-382">Hedef</span><span class="sxs-lookup"><span data-stu-id="780f1-382">Goal</span></span> | <span data-ttu-id="780f1-383">Regex dize &</span><span class="sxs-lookup"><span data-stu-id="780f1-383">Regex String &</span></span><br><span data-ttu-id="780f1-384">Eşleşme örneği</span><span class="sxs-lookup"><span data-stu-id="780f1-384">Match Example</span></span> | <span data-ttu-id="780f1-385">Değiştirme dizesini &</span><span class="sxs-lookup"><span data-stu-id="780f1-385">Replacement String &</span></span><br><span data-ttu-id="780f1-386">Çıkış örneği</span><span class="sxs-lookup"><span data-stu-id="780f1-386">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="780f1-387">Yol querystring yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="780f1-387">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="780f1-388">Şerit eğik</span><span class="sxs-lookup"><span data-stu-id="780f1-388">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="780f1-389">Sondaki eğik çizgi zorla</span><span class="sxs-lookup"><span data-stu-id="780f1-389">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="780f1-390">Belirli isteklere yeniden yazma işlemi kaçının</span><span class="sxs-lookup"><span data-stu-id="780f1-390">Avoid rewriting specific requests</span></span> | <span data-ttu-id="780f1-391">`^(.*)(?<!\.axd)$` Veya `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="780f1-391">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="780f1-392">Evet: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="780f1-392">Yes: `/resource.htm`</span></span><br><span data-ttu-id="780f1-393">Hayır: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="780f1-393">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="780f1-394">URL kesimleri yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="780f1-394">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="780f1-395">URL kesimi Değiştir</span><span class="sxs-lookup"><span data-stu-id="780f1-395">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="780f1-396">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="780f1-396">Additional resources</span></span>
* [<span data-ttu-id="780f1-397">Uygulama Başlatma</span><span class="sxs-lookup"><span data-stu-id="780f1-397">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="780f1-398">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="780f1-398">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="780f1-399">.NET normal ifadeler</span><span class="sxs-lookup"><span data-stu-id="780f1-399">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="780f1-400">Normal ifade dili - hızlı başvuru</span><span class="sxs-lookup"><span data-stu-id="780f1-400">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="780f1-401">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="780f1-401">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="780f1-402">URL yeniden yazma modülünü 2.0 (IIS için) kullanma</span><span class="sxs-lookup"><span data-stu-id="780f1-402">Using Url Rewrite Module 2.0 (for IIS)</span></span>](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="780f1-403">URL yeniden yazma modülü yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="780f1-403">URL Rewrite Module Configuration Reference</span></span>](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="780f1-404">IIS URL yeniden yazma modülü Forumu</span><span class="sxs-lookup"><span data-stu-id="780f1-404">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="780f1-405">Basit bir URL yapıya tutun</span><span class="sxs-lookup"><span data-stu-id="780f1-405">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="780f1-406">10 URL yeniden yazma ipuçları ve püf noktaları</span><span class="sxs-lookup"><span data-stu-id="780f1-406">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="780f1-407">Eğik çizgi veya eğik çizgi yok</span><span class="sxs-lookup"><span data-stu-id="780f1-407">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
