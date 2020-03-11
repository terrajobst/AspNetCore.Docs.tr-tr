---
title: ASP.NET Core içinde URL yeniden yazma ara yazılımı
author: rick-anderson
description: ASP.NET Core uygulamalarında URL yeniden yazma ve URL yeniden yazma ara yazılımı ile yeniden yönlendirme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/16/2019
uid: fundamentals/url-rewriting
ms.openlocfilehash: 7d63cf381f1d8a19ed4fb789348e36f94304ad63
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666469"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="cfdd6-103">ASP.NET Core içinde URL yeniden yazma ara yazılımı</span><span class="sxs-lookup"><span data-stu-id="cfdd6-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="cfdd6-104">X [MIKAEL Mengistu](https://github.com/mikaelm12) tarafından</span><span class="sxs-lookup"><span data-stu-id="cfdd6-104">By [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cfdd6-105">Bu belgede, ASP.NET Core uygulamalarında URL yeniden yazma ara yazılımı kullanma yönergeleriyle birlikte URL yeniden yazma tanıtılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-105">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

<span data-ttu-id="cfdd6-106">URL yeniden yazma, istek URL 'Lerini bir veya daha fazla önceden tanımlanmış kurala göre değiştirme işlemidir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="cfdd6-107">URL yeniden yazma, konumların ve adreslerin sıkı bir şekilde bağlanmaması için kaynak konumları ve adresleri arasında bir soyutlama oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses aren't tightly linked.</span></span> <span data-ttu-id="cfdd6-108">URL yeniden yazma işlemi birkaç senaryoda yararlı olur:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-108">URL rewriting is valuable in several scenarios to:</span></span>

* <span data-ttu-id="cfdd6-109">Sunucu kaynaklarını geçici olarak veya kalıcı olarak taşıyın veya değiştirin ve bu kaynakların kararlı konum belirleyicilerinin bakımını yapın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-109">Move or replace server resources temporarily or permanently and maintain stable locators for those resources.</span></span>
* <span data-ttu-id="cfdd6-110">İstek işlemeyi farklı uygulamalar arasında veya bir uygulamanın alanlarında bölme.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-110">Split request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="cfdd6-111">Gelen isteklerde URL segmentlerini kaldırın, ekleyin veya yeniden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-111">Remove, add, or reorganize URL segments on incoming requests.</span></span>
* <span data-ttu-id="cfdd6-112">Arama motoru Iyileştirmesi (SEO) için genel URL 'Leri iyileştirin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-112">Optimize public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="cfdd6-113">Ziyaretçilerin bir kaynak isteyerek döndürülen içeriği tahmin etmeye yardımcı olmak için kolay genel URL 'Lerin kullanılmasına izin verme.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-113">Permit the use of friendly public URLs to help visitors predict the content returned by requesting a resource.</span></span>
* <span data-ttu-id="cfdd6-114">Güvensiz istekleri güvenli uç noktalara yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-114">Redirect insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="cfdd6-115">Bir dış sitenin varlığı kendi içeriğine bağlayarak başka bir sitede barındırılan statik bir varlık kullandığı Hotlink 'i engelleyin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-115">Prevent hotlinking, where an external site uses a hosted static asset on another site by linking the asset into its own content.</span></span>

> [!NOTE]
> <span data-ttu-id="cfdd6-116">URL yeniden yazma, bir uygulamanın performansını azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-116">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="cfdd6-117">Uygun yerlerde kuralların sayısını ve karmaşıklığını sınırlayın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-117">Where feasible, limit the number and complexity of rules.</span></span>

<span data-ttu-id="cfdd6-118">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cfdd6-118">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="cfdd6-119">URL yeniden yönlendirme ve URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="cfdd6-119">URL redirect and URL rewrite</span></span>

<span data-ttu-id="cfdd6-120">*URL yeniden yönlendirme* ve *URL yeniden yazma* arasındaki ifade farkı daha hafif ancak istemcilere kaynak sağlamak için önemli etkileri vardır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-120">The difference in wording between *URL redirect* and *URL rewrite* is subtle but has important implications for providing resources to clients.</span></span> <span data-ttu-id="cfdd6-121">ASP.NET Core URL yeniden yazma ara yazılımı her ikisine de ihtiyacı verebilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-121">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="cfdd6-122">*URL yeniden yönlendirme* , istemcinin, ilk olarak istenen istemciden farklı bir adresteki kaynağa erişmesi için bir istemci tarafı işlemi içerir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-122">A *URL redirect* involves a client-side operation, where the client is instructed to access a resource at a different address than the client originally requested.</span></span> <span data-ttu-id="cfdd6-123">Bu, sunucuya gidiş dönüş gerektirir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-123">This requires a round trip to the server.</span></span> <span data-ttu-id="cfdd6-124">İstemci kaynak için yeni bir istek yaptığında, istemciye döndürülen yeniden yönlendirme URL 'SI tarayıcının adres çubuğunda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-124">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span>

<span data-ttu-id="cfdd6-125">`/resource` `/different-resource`*yeniden yönlendirilirse* sunucu, istemcinin yeniden yönlendirmenin geçici ya da kalıcı olduğunu belirten bir durum kodu ile `/different-resource` kaynağı elde etmesi gerektiğini yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-125">If `/resource` is *redirected* to `/different-resource`, the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span>

![Bir WebAPI hizmeti uç noktası, sürüm 1 ' den (v1) sunucudaki sürüm 2 ' ye (v2) geçici olarak değiştirildi.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="cfdd6-131">İstekleri farklı bir URL 'ye yönlendirirken, yanıtla birlikte durum kodunu belirterek yeniden yönlendirmenin kalıcı mi yoksa geçici mi olduğunu belirtin:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-131">When redirecting requests to a different URL, indicate whether the redirect is permanent or temporary by specifying the status code with the response:</span></span>

* <span data-ttu-id="cfdd6-132">*301-taşınan kalıcı* durum kodu, kaynağın yeni, kalıcı bir URL olduğu ve istemciye, gelecekteki tüm isteklerin kaynak için tüm istekleri yeni URL 'yi kullanması gerektiğini bildirmek istediğinizde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-132">The *301 - Moved Permanently* status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="cfdd6-133">*İstemci, 301 durum kodu alındığında yanıtı önbelleğe alabilir ve yeniden kullanabilir.*</span><span class="sxs-lookup"><span data-stu-id="cfdd6-133">*The client may cache and reuse the response when a 301 status code is received.*</span></span>

* <span data-ttu-id="cfdd6-134">*302-bulunan* durum kodu, yeniden yönlendirmenin geçici ya da genellikle değişikliğe tabi olduğu durumlarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-134">The *302 - Found* status code is used where the redirection is temporary or generally subject to change.</span></span> <span data-ttu-id="cfdd6-135">302 durum kodu, istemcinin URL 'YI depolayamadığını ve gelecekte kullanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-135">The 302 status code indicates to the client not to store the URL and use it in the future.</span></span>

<span data-ttu-id="cfdd6-136">Durum kodları hakkında daha fazla bilgi için bkz. [RFC 2616: durum kodu tanımları](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="cfdd6-136">For more information on status codes, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="cfdd6-137">*URL yeniden yazma* , istenen istemciden farklı bir kaynak adresinden kaynak sağlayan sunucu tarafı bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-137">A *URL rewrite* is a server-side operation that provides a resource from a different resource address than the client requested.</span></span> <span data-ttu-id="cfdd6-138">URL yeniden yazma, sunucuya gidiş dönüş gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-138">Rewriting a URL doesn't require a round trip to the server.</span></span> <span data-ttu-id="cfdd6-139">Yeniden yazan URL istemciye döndürülmüyor ve tarayıcının adres çubuğunda görünmüyor.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-139">The rewritten URL isn't returned to the client and doesn't appear in the browser's address bar.</span></span>

<span data-ttu-id="cfdd6-140">`/resource` `/different-resource`olarak *yeniden döndürülürse* , sunucu `/different-resource`kaynağı *dahili olarak* getirir ve döndürür.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-140">If `/resource` is *rewritten* to `/different-resource`, the server *internally* fetches and returns the resource at `/different-resource`.</span></span>

<span data-ttu-id="cfdd6-141">İstemci, yeniden yazan URL 'de kaynağı alabiliyor olsa da, istemci isteği yaptığında ve yanıtı aldığında kaynağın yeniden yazan URL 'de bulunduğunu bilgilendirmez.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-141">Although the client might be able to retrieve the resource at the rewritten URL, the client isn't informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Sunucu üzerinde sürüm 1 ' den (v1) sürüm 2 ' ye (v2) bir WebAPI hizmet uç noktası değiştirilmiştir.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="cfdd6-146">URL yeniden yazma örnek uygulaması</span><span class="sxs-lookup"><span data-stu-id="cfdd6-146">URL rewriting sample app</span></span>

<span data-ttu-id="cfdd6-147">[Örnek uygulamayla](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)birlikte YENIDEN yazma URL 'sinin özelliklerini inceleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-147">You can explore the features of the URL Rewriting Middleware with the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="cfdd6-148">Uygulama yeniden yönlendirme ve yeniden yazma kuralları uygular ve birkaç senaryo için yeniden yönlendirilen veya yeniden yazan URL 'YI gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-148">The app applies redirect and rewrite rules and shows the redirected or rewritten URL for several scenarios.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="cfdd6-149">URL yeniden yazma ara yazılımı ne zaman kullanılır</span><span class="sxs-lookup"><span data-stu-id="cfdd6-149">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="cfdd6-150">Aşağıdaki yaklaşımlardan birini kullandığımmdan URL yeniden yazma ara yazılımı kullanın:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-150">Use URL Rewriting Middleware when you're unable to use the following approaches:</span></span>

* [<span data-ttu-id="cfdd6-151">Windows Server 'da IIS ile URL yeniden yazma modülü</span><span class="sxs-lookup"><span data-stu-id="cfdd6-151">URL Rewrite module with IIS on Windows Server</span></span>](https://www.iis.net/downloads/microsoft/url-rewrite)
* [<span data-ttu-id="cfdd6-152">Apache Server 'da Apache mod_rewrite modülü</span><span class="sxs-lookup"><span data-stu-id="cfdd6-152">Apache mod_rewrite module on Apache Server</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="cfdd6-153">NGINX üzerinde URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="cfdd6-153">URL rewriting on Nginx</span></span>](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

<span data-ttu-id="cfdd6-154">Ayrıca, uygulama [http. sys sunucusunda](xref:fundamentals/servers/httpsys) barındırıldığı zaman ara yazılımı kullanın (eski adıyla webListener olarak adlandırılır).</span><span class="sxs-lookup"><span data-stu-id="cfdd6-154">Also, use the middleware when the app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener).</span></span>

<span data-ttu-id="cfdd6-155">IIS, Apache ve NGINX 'te sunucu tabanlı URL yeniden yazma teknolojilerini kullanmanın başlıca nedenleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-155">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, and Nginx are:</span></span>

* <span data-ttu-id="cfdd6-156">Ara yazılım bu modüllerin tüm özelliklerini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-156">The middleware doesn't support the full features of these modules.</span></span>

  <span data-ttu-id="cfdd6-157">Sunucu modüllerinin bazı özellikleri, IIS yeniden yazma modülünün `IsFile` ve `IsDirectory` kısıtlamaları gibi ASP.NET Core projelerle çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-157">Some of the features of the server modules don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="cfdd6-158">Bu senaryolarda, bunun yerine ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-158">In these scenarios, use the middleware instead.</span></span>
* <span data-ttu-id="cfdd6-159">Ara yazılım performansı büyük olasılıkla modüllerle eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-159">The performance of the middleware probably doesn't match that of the modules.</span></span>

  <span data-ttu-id="cfdd6-160">Sınama, performansı en iyi şekilde düşürür veya performans düşüklüğü göz ardı edilebilir olduğundan emin olmanın tek yoludur.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-160">Benchmarking is the only way to know for sure which approach degrades performance the most or if degraded performance is negligible.</span></span>

## <a name="package"></a><span data-ttu-id="cfdd6-161">Paket</span><span class="sxs-lookup"><span data-stu-id="cfdd6-161">Package</span></span>

<span data-ttu-id="cfdd6-162">URL yeniden yazma ara yazılımı, ASP.NET Core uygulamalarında örtük olarak bulunan [Microsoft. AspNetCore. yeniden yazma](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-162">URL Rewriting Middleware is provided by the [Microsoft.AspNetCore.Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) package, which is implicitly included in ASP.NET Core apps.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="cfdd6-163">Uzantı ve Seçenekler</span><span class="sxs-lookup"><span data-stu-id="cfdd6-163">Extension and options</span></span>

<span data-ttu-id="cfdd6-164">Yeniden yazma kurallarınızın her biri için uzantı yöntemleriyle [Rewriteoptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) sınıfının bir ÖRNEĞINI oluşturarak URL yeniden yazma ve yeniden yönlendirme kuralları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-164">Establish URL rewrite and redirect rules by creating an instance of the [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) class with extension methods for each of your rewrite rules.</span></span> <span data-ttu-id="cfdd6-165">Birden çok kuralı, işlenmeyi istediğiniz sırada zincirle.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-165">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="cfdd6-166">`RewriteOptions`, <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>ile istek ardışık düzenine eklendiği için, URL 'nin yeniden yazma ara moduna geçirilir:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-166">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>:</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

### <a name="redirect-non-www-to-www"></a><span data-ttu-id="cfdd6-167">Www olmayan www 'e yönlendirme</span><span class="sxs-lookup"><span data-stu-id="cfdd6-167">Redirect non-www to www</span></span>

<span data-ttu-id="cfdd6-168">Üç seçenek, uygulamanın`www` olmayan istekleri `www`yeniden yönlendirmesine izin verir:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-168">Three options permit the app to redirect non-`www` requests to `www`:</span></span>

* <span data-ttu-id="cfdd6-169">istek`www`değilse, <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*> &ndash; `www` alt etki alanına kalıcı olarak yeniden yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-169"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*> &ndash; Permanently redirect the request to the `www` subdomain if the request is non-`www`.</span></span> <span data-ttu-id="cfdd6-170">[Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) durum kodu ile yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-170">Redirects with a [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) status code.</span></span>

* <span data-ttu-id="cfdd6-171">gelen istek`www`değilse, isteği `www` alt etki alanına yönlendirmek &ndash; <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*>.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-171"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*> &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="cfdd6-172">[Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) durum kodu ile yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-172">Redirects with a [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) status code.</span></span> <span data-ttu-id="cfdd6-173">Aşırı yükleme, yanıt için durum kodu sağlamanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-173">An overload permits you to provide the status code for the response.</span></span> <span data-ttu-id="cfdd6-174">Bir durum kodu ataması için <xref:Microsoft.AspNetCore.Http.StatusCodes> sınıfının bir alanını kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-174">Use a field of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for a status code assignment.</span></span>

### <a name="url-redirect"></a><span data-ttu-id="cfdd6-175">URL yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="cfdd6-175">URL redirect</span></span>

<span data-ttu-id="cfdd6-176">İstekleri yeniden yönlendirmek için <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-176">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> to redirect requests.</span></span> <span data-ttu-id="cfdd6-177">İlk parametre gelen URL 'nin yolu ile eşleşen Regex içerir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-177">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="cfdd6-178">İkinci parametre değiştirme dizesidir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-178">The second parameter is the replacement string.</span></span> <span data-ttu-id="cfdd6-179">Varsa, üçüncü parametre durum kodunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-179">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="cfdd6-180">Durum kodunu belirtmezseniz, durum kodu varsayılan olarak *302-bulunur*; bu da kaynağın geçici olarak taşındığını veya değiştirildiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-180">If you don't specify the status code, the status code defaults to *302 - Found*, which indicates that the resource is temporarily moved or replaced.</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=9)]

<span data-ttu-id="cfdd6-181">Geliştirici araçları etkinleştirilmiş bir tarayıcıda, `/redirect-rule/1234/5678`yol ile örnek uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-181">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="cfdd6-182">Regex `redirect-rule/(.*)`istek yoluyla eşleşir ve yol `/redirected/1234/5678`ile değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-182">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="cfdd6-183">Yeniden yönlendirme URL 'SI, *302 tarafından bulunan* bir durum kodu ile istemciye geri gönderilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-183">The redirect URL is sent back to the client with a *302 - Found* status code.</span></span> <span data-ttu-id="cfdd6-184">Tarayıcı, tarayıcının adres çubuğunda görüntülenen yeniden yönlendirme URL 'SI üzerinde yeni bir istek oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-184">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="cfdd6-185">Örnek uygulamadaki hiçbir kural, yeniden yönlendirme URL 'SI üzerinde eşleşmediğinden:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-185">Since no rules in the sample app match on the redirect URL:</span></span>

* <span data-ttu-id="cfdd6-186">İkinci istek uygulamadan *200-Tamam* yanıtı alır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-186">The second request receives a *200 - OK* response from the app.</span></span>
* <span data-ttu-id="cfdd6-187">Yanıtın gövdesi, yeniden yönlendirme URL 'sini gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-187">The body of the response shows the redirect URL.</span></span>

<span data-ttu-id="cfdd6-188">Bir URL *yeniden yönlendirildiğinde*sunucuya gidiş dönüş yapılır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-188">A round trip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="cfdd6-189">Yeniden yönlendirme kuralları oluştururken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-189">Be cautious when establishing redirect rules.</span></span> <span data-ttu-id="cfdd6-190">Yeniden yönlendirme kuralları, bir yeniden yönlendirmeden sonra dahil olmak üzere, uygulamaya yapılan her istekte değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-190">Redirect rules are evaluated on every request to the app, including after a redirect.</span></span> <span data-ttu-id="cfdd6-191">Yanlışlıkla *sonsuz yeniden yönlendirmeler döngüsü*oluşturmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-191">It's easy to accidentally create a *loop of infinite redirects*.</span></span>

<span data-ttu-id="cfdd6-192">Özgün Istek: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-192">Original Request: `/redirect-rule/1234/5678`</span></span>

![İstekleri ve yanıtları izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="cfdd6-194">Parantez içinde yer alan ifadenin kısmına bir *yakalama grubu*denir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-194">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="cfdd6-195">İfadenin nokta (`.`), *herhangi bir karakterle eşleşir*anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-195">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="cfdd6-196">Yıldız işareti (`*`) *, önceki karakterle sıfır veya daha fazla kez eşleşme*olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-196">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="cfdd6-197">Bu nedenle, URL 'nin son iki yol kesimleri `1234/5678`, yakalama grubu `(.*)`tarafından yakalanır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-197">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="cfdd6-198">`redirect-rule/` sonra istek URL 'sinde sağladığınız herhangi bir değer bu tek yakalama grubu tarafından yakalanır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-198">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="cfdd6-199">Değiştirme dizesinde, yakalanan gruplar, dolar işareti (`$`) ve ardından yakalamanın sıra numarası ile birlikte dizeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-199">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="cfdd6-200">İlk yakalama grubu değeri `$1`, ikincisi `$2`ile alınır ve Regex içindeki yakalama grupları için sırayla devam eder.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-200">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="cfdd6-201">Örnek uygulamadaki yeniden yönlendirme kuralı Regex bölümünde yalnızca bir tane yakalanan grup bulunur, bu nedenle değiştirme dizesinde yalnızca bir eklenmiş grup vardır. bu `$1`.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-201">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="cfdd6-202">Kural uygulandığında, URL `/redirected/1234/5678`olur.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-202">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="cfdd6-203">Güvenli bir uç noktaya URL yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="cfdd6-203">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="cfdd6-204">HTTP isteklerini HTTPS protokolünü kullanarak aynı konağa ve yola yönlendirmek için <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-204">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> to redirect HTTP requests to the same host and path using the HTTPS protocol.</span></span> <span data-ttu-id="cfdd6-205">Durum kodu sağlanmazsa, ara yazılım varsayılan olarak *302-bulunur*.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-205">If the status code isn't supplied, the middleware defaults to *302 - Found*.</span></span> <span data-ttu-id="cfdd6-206">Bağlantı noktası sağlanmazsa:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-206">If the port isn't supplied:</span></span>

* <span data-ttu-id="cfdd6-207">Ara yazılım varsayılan olarak `null`.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-207">The middleware defaults to `null`.</span></span>
* <span data-ttu-id="cfdd6-208">Düzen `https` (HTTPS protokolü) olarak değişir ve istemci kaynağa 443 numaralı bağlantı noktasında erişir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-208">The scheme changes to `https` (HTTPS protocol), and the client accesses the resource on port 443.</span></span>

<span data-ttu-id="cfdd6-209">Aşağıdaki örnek, durum kodunun *301-kalıcı olarak taşınacağını* ve bağlantı noktasını 5001 olarak nasıl değiştirileceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-209">The following example shows how to set the status code to *301 - Moved Permanently* and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="cfdd6-210">Güvenli olmayan istekleri, bağlantı noktası 443 üzerinde güvenli HTTPS protokolü ile aynı konağa ve yola yönlendirmek için <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-210">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> to redirect insecure requests to the same host and path with secure HTTPS protocol on port 443.</span></span> <span data-ttu-id="cfdd6-211">Ara yazılım durum kodunu 301 olarak ayarlar ve *kalıcı olarak taşınır*.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-211">The middleware sets the status code to *301 - Moved Permanently*.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="cfdd6-212">Ek yeniden yönlendirme kuralları gereksinimi olmadan güvenli bir uç noktaya yönlendirilirken, HTTPS yeniden yönlendirme ara yazılımı kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-212">When redirecting to a secure endpoint without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="cfdd6-213">Daha fazla bilgi için bkz. [https 'Yi zorla](xref:security/enforcing-ssl#require-https) konusu.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-213">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="cfdd6-214">Örnek uygulama `AddRedirectToHttps` veya `AddRedirectToHttpsPermanent`nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-214">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="cfdd6-215">Uzantı yöntemini `RewriteOptions`ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-215">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="cfdd6-216">Herhangi bir URL 'de uygulamaya güvenli olmayan bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-216">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="cfdd6-217">Otomatik olarak imzalanan sertifikanın güvenilmeyen tarayıcı güvenlik uyarısını kapatın veya sertifikaya güvenmek için bir özel durum oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-217">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="cfdd6-218">`AddRedirectToHttps(301, 5001)`kullanan özgün Istek: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-218">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![İstekleri ve yanıtları izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="cfdd6-220">`AddRedirectToHttpsPermanent`kullanan özgün Istek: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-220">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![İstekleri ve yanıtları izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="cfdd6-222">URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="cfdd6-222">URL rewrite</span></span>

<span data-ttu-id="cfdd6-223">URL 'Leri yeniden yazma kuralı oluşturmak için <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-223">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> to create a rule for rewriting URLs.</span></span> <span data-ttu-id="cfdd6-224">İlk parametre gelen URL yolundaki eşleşme için Regex içerir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-224">The first parameter contains the regex for matching on the incoming URL path.</span></span> <span data-ttu-id="cfdd6-225">İkinci parametre değiştirme dizesidir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-225">The second parameter is the replacement string.</span></span> <span data-ttu-id="cfdd6-226">`skipRemainingRules: {true|false}`üçüncü parametresi, geçerli kural uygulanmışsa ek yeniden yazma kurallarının atlanıp atlanmayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-226">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=10-11)]

<span data-ttu-id="cfdd6-227">Özgün Istek: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-227">Original Request: `/rewrite-rule/1234/5678`</span></span>

![İsteği ve yanıtı izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="cfdd6-229">İfadenin başındaki simgeyi seçtiğinizde (`^`), eşleşmesinin URL yolunun başlangıcında başladığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-229">The carat (`^`) at the beginning of the expression means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="cfdd6-230">Önceki örnekte, yeniden yönlendirme kuralıyla `redirect-rule/(.*)`, Regex başlangıcında simgeyi seçtiğinizde (`^`) yok.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-230">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat (`^`) at the start of the regex.</span></span> <span data-ttu-id="cfdd6-231">Bu nedenle, başarılı bir eşleşme için yoldaki tüm karakterler `redirect-rule/` önüne çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-231">Therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="cfdd6-232">Yol</span><span class="sxs-lookup"><span data-stu-id="cfdd6-232">Path</span></span>                               | <span data-ttu-id="cfdd6-233">Eşleştirme</span><span class="sxs-lookup"><span data-stu-id="cfdd6-233">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="cfdd6-234">Yes</span><span class="sxs-lookup"><span data-stu-id="cfdd6-234">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="cfdd6-235">Yes</span><span class="sxs-lookup"><span data-stu-id="cfdd6-235">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="cfdd6-236">Yes</span><span class="sxs-lookup"><span data-stu-id="cfdd6-236">Yes</span></span>   |

<span data-ttu-id="cfdd6-237">Yeniden yazma kuralı `^rewrite-rule/(\d+)/(\d+)`, yalnızca `rewrite-rule/`başlarsa yollarla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-237">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="cfdd6-238">Aşağıdaki tabloda, eşleşen farkı aklınızda.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-238">In the following table, note the difference in matching.</span></span>

| <span data-ttu-id="cfdd6-239">Yol</span><span class="sxs-lookup"><span data-stu-id="cfdd6-239">Path</span></span>                              | <span data-ttu-id="cfdd6-240">Eşleştirme</span><span class="sxs-lookup"><span data-stu-id="cfdd6-240">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="cfdd6-241">Yes</span><span class="sxs-lookup"><span data-stu-id="cfdd6-241">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="cfdd6-242">Hayır</span><span class="sxs-lookup"><span data-stu-id="cfdd6-242">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="cfdd6-243">Hayır</span><span class="sxs-lookup"><span data-stu-id="cfdd6-243">No</span></span>    |

<span data-ttu-id="cfdd6-244">İfadenin `^rewrite-rule/` kısmını izleyerek, `(\d+)/(\d+)`iki yakalama grubu vardır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-244">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="cfdd6-245">`\d`, *bir sayıyla (sayı) eşleşme*anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-245">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="cfdd6-246">Artı işareti (`+`) *bir veya daha fazla önceki karakterden eşleşiyor*demektir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-246">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="cfdd6-247">Bu nedenle, URL bir sayı içermeli ve ardından İleri eğik çizgi ve ardından başka bir sayı içermelidir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-247">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="cfdd6-248">Bu yakalama grupları, `$1` ve `$2`olarak yeniden yazan URL 'sine eklenir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-248">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="cfdd6-249">Yeniden yazma kuralı değiştirme dizesi yakalanan grupları sorgu dizesine koyar.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-249">The rewrite rule replacement string places the captured groups into the query string.</span></span> <span data-ttu-id="cfdd6-250">`/rewritten?var1=1234&var2=5678`kaynağı almak için istenen `/rewrite-rule/1234/5678` yolu yeniden yazıldı.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-250">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="cfdd6-251">Özgün istekte bir sorgu dizesi varsa, URL yeniden yazdığınızda korunur.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-251">If a query string is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="cfdd6-252">Kaynağı almak için sunucuya gidiş dönüş yok.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-252">There's no round trip to the server to obtain the resource.</span></span> <span data-ttu-id="cfdd6-253">Kaynak varsa, bu, alınır ve istemciye *200-ok* durum kodu ile döndürülür.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-253">If the resource exists, it's fetched and returned to the client with a *200 - OK* status code.</span></span> <span data-ttu-id="cfdd6-254">İstemci yeniden yönlendirmediği için tarayıcının adres çubuğundaki URL değişmez.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-254">Because the client isn't redirected, the URL in the browser's address bar doesn't change.</span></span> <span data-ttu-id="cfdd6-255">İstemciler, sunucuda bir URL yeniden yazma işleminin gerçekleştiğini algılayamaz.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-255">Clients can't detect that a URL rewrite operation occurred on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="cfdd6-256">Eşleşen kuralların hesaplama maliyeti ve uygulama yanıt süresini arttığı için mümkün olduğunda `skipRemainingRules: true` kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-256">Use `skipRemainingRules: true` whenever possible because matching rules is computationally expensive and increases app response time.</span></span> <span data-ttu-id="cfdd6-257">En hızlı uygulama yanıtı için:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-257">For the fastest app response:</span></span>
>
> * <span data-ttu-id="cfdd6-258">En sık eşleşen kuraldan en az sıklıkta eşleşen kurala göre yeniden yazma kuralları.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-258">Order rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="cfdd6-259">Bir eşleşme gerçekleştiğinde ve ek kural işleme gerekli olmadığında kalan kuralların işlenmesini atlayın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-259">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-mod_rewrite"></a><span data-ttu-id="cfdd6-260">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="cfdd6-260">Apache mod_rewrite</span></span>

<span data-ttu-id="cfdd6-261"><xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*>ile Apache mod_rewrite kuralları uygulayın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-261">Apply Apache mod_rewrite rules with <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*>.</span></span> <span data-ttu-id="cfdd6-262">Kurallar dosyasının uygulamayla birlikte dağıtıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-262">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="cfdd6-263">Daha fazla bilgi ve mod_rewrite kuralları örnekleri için bkz. [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="cfdd6-263">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

<span data-ttu-id="cfdd6-264">*ApacheModRewrite. txt* kuralları dosyasındaki kuralları okumak için bir <xref:System.IO.StreamReader> kullanılır:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-264">A <xref:System.IO.StreamReader> is used to read the rules from the *ApacheModRewrite.txt* rules file:</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=3-4,12)]

<span data-ttu-id="cfdd6-265">Örnek uygulama, istekleri `/redirected?id=$1``/apache-mod-rules-redirect/(.\*)` yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-265">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="cfdd6-266">Yanıt durum kodu *302-bulundu*.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-266">The response status code is *302 - Found*.</span></span>

[!code[](url-rewriting/samples/3.x/SampleApp/ApacheModRewrite.txt)]

<span data-ttu-id="cfdd6-267">Özgün Istek: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-267">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![İstekleri ve yanıtları izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_apache_mod_redirect.png)

<span data-ttu-id="cfdd6-269">Ara yazılım aşağıdaki Apache mod_rewrite sunucu değişkenlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-269">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="cfdd6-270">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="cfdd6-270">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="cfdd6-271">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="cfdd6-271">HTTP_ACCEPT</span></span>
* <span data-ttu-id="cfdd6-272">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="cfdd6-272">HTTP_CONNECTION</span></span>
* <span data-ttu-id="cfdd6-273">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="cfdd6-273">HTTP_COOKIE</span></span>
* <span data-ttu-id="cfdd6-274">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="cfdd6-274">HTTP_FORWARDED</span></span>
* <span data-ttu-id="cfdd6-275">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="cfdd6-275">HTTP_HOST</span></span>
* <span data-ttu-id="cfdd6-276">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="cfdd6-276">HTTP_REFERER</span></span>
* <span data-ttu-id="cfdd6-277">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="cfdd6-277">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="cfdd6-278">HTTPS</span><span class="sxs-lookup"><span data-stu-id="cfdd6-278">HTTPS</span></span>
* <span data-ttu-id="cfdd6-279">IPV6</span><span class="sxs-lookup"><span data-stu-id="cfdd6-279">IPV6</span></span>
* <span data-ttu-id="cfdd6-280">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="cfdd6-280">QUERY_STRING</span></span>
* <span data-ttu-id="cfdd6-281">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="cfdd6-281">REMOTE_ADDR</span></span>
* <span data-ttu-id="cfdd6-282">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="cfdd6-282">REMOTE_PORT</span></span>
* <span data-ttu-id="cfdd6-283">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="cfdd6-283">REQUEST_FILENAME</span></span>
* <span data-ttu-id="cfdd6-284">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="cfdd6-284">REQUEST_METHOD</span></span>
* <span data-ttu-id="cfdd6-285">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="cfdd6-285">REQUEST_SCHEME</span></span>
* <span data-ttu-id="cfdd6-286">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="cfdd6-286">REQUEST_URI</span></span>
* <span data-ttu-id="cfdd6-287">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="cfdd6-287">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="cfdd6-288">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="cfdd6-288">SERVER_ADDR</span></span>
* <span data-ttu-id="cfdd6-289">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="cfdd6-289">SERVER_PORT</span></span>
* <span data-ttu-id="cfdd6-290">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="cfdd6-290">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="cfdd6-291">TIME</span><span class="sxs-lookup"><span data-stu-id="cfdd6-291">TIME</span></span>
* <span data-ttu-id="cfdd6-292">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="cfdd6-292">TIME_DAY</span></span>
* <span data-ttu-id="cfdd6-293">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="cfdd6-293">TIME_HOUR</span></span>
* <span data-ttu-id="cfdd6-294">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="cfdd6-294">TIME_MIN</span></span>
* <span data-ttu-id="cfdd6-295">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="cfdd6-295">TIME_MON</span></span>
* <span data-ttu-id="cfdd6-296">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="cfdd6-296">TIME_SEC</span></span>
* <span data-ttu-id="cfdd6-297">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="cfdd6-297">TIME_WDAY</span></span>
* <span data-ttu-id="cfdd6-298">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="cfdd6-298">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="cfdd6-299">IIS URL yeniden yazma modülü kuralları</span><span class="sxs-lookup"><span data-stu-id="cfdd6-299">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="cfdd6-300">IIS URL yeniden yazma modülü için geçerli olan kural kümesini kullanmak için <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-300">To use the same rule set that applies to the IIS URL Rewrite Module, use <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>.</span></span> <span data-ttu-id="cfdd6-301">Kurallar dosyasının uygulamayla birlikte dağıtıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-301">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="cfdd6-302">Windows Server IIS 'de çalışırken uygulamanın *Web. config* dosyasını kullanmak için ara yazılımı yönlendirmeyin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-302">Don't direct the middleware to use the app's *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="cfdd6-303">IIS ile, IIS yeniden yazma modülüyle çakışmalardan kaçınmak için bu kuralların uygulamanın *Web. config* dosyası dışında depolanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-303">With IIS, these rules should be stored outside of the app's *web.config* file in order to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="cfdd6-304">Daha fazla bilgi ve IIS URL yeniden yazma modülü kurallarının örnekleri için bkz. [Using URL yeniden yazma modülü 2,0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) ve [URL yeniden yazma modülü yapılandırma başvurusu](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="cfdd6-304">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

<span data-ttu-id="cfdd6-305"><xref:System.IO.StreamReader> *Iisurlyeniden yazma. xml* kuralları dosyasındaki kuralları okumak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-305">A <xref:System.IO.StreamReader> is used to read the rules from the *IISUrlRewrite.xml* rules file:</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5-6,13)]

<span data-ttu-id="cfdd6-306">Örnek uygulama, `/iis-rules-rewrite/(.*)` olan istekleri `/rewritten?id=$1`olarak yeniden yazar.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-306">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="cfdd6-307">Yanıt, istemciye *200-ok* durum kodu ile gönderilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-307">The response is sent to the client with a *200 - OK* status code.</span></span>

[!code-xml[](url-rewriting/samples/3.x/SampleApp/IISUrlRewrite.xml)]

<span data-ttu-id="cfdd6-308">Özgün Istek: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-308">Original Request: `/iis-rules-rewrite/1234`</span></span>

![İsteği ve yanıtı izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="cfdd6-310">Uygulamanızı istenmeyen yollarla etkileyebilecek sunucu düzeyi kurallara sahip etkin bir IIS yeniden yazma modülünüzün olması halinde, bir uygulama için IIS yeniden yazma modülünü devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-310">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="cfdd6-311">Daha fazla bilgi için bkz. [IIS modüllerini devre dışı bırakma](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="cfdd6-311">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="cfdd6-312">Desteklenmeyen özellikler</span><span class="sxs-lookup"><span data-stu-id="cfdd6-312">Unsupported features</span></span>

<span data-ttu-id="cfdd6-313">ASP.NET Core 2. x ile yayınlanan ara yazılım, aşağıdaki IIS URL yeniden yazma modülü özelliklerini desteklemez:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-313">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="cfdd6-314">Giden Kuralları</span><span class="sxs-lookup"><span data-stu-id="cfdd6-314">Outbound Rules</span></span>
* <span data-ttu-id="cfdd6-315">Özel sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="cfdd6-315">Custom Server Variables</span></span>
* <span data-ttu-id="cfdd6-316">Joker karakterler</span><span class="sxs-lookup"><span data-stu-id="cfdd6-316">Wildcards</span></span>
* <span data-ttu-id="cfdd6-317">LogRewrittenUrl 'Si</span><span class="sxs-lookup"><span data-stu-id="cfdd6-317">LogRewrittenUrl</span></span>

#### <a name="supported-server-variables"></a><span data-ttu-id="cfdd6-318">Desteklenen sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="cfdd6-318">Supported server variables</span></span>

<span data-ttu-id="cfdd6-319">Ara yazılım aşağıdaki IIS URL yeniden yazma modülü sunucu değişkenlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-319">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="cfdd6-320">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="cfdd6-320">CONTENT_LENGTH</span></span>
* <span data-ttu-id="cfdd6-321">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="cfdd6-321">CONTENT_TYPE</span></span>
* <span data-ttu-id="cfdd6-322">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="cfdd6-322">HTTP_ACCEPT</span></span>
* <span data-ttu-id="cfdd6-323">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="cfdd6-323">HTTP_CONNECTION</span></span>
* <span data-ttu-id="cfdd6-324">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="cfdd6-324">HTTP_COOKIE</span></span>
* <span data-ttu-id="cfdd6-325">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="cfdd6-325">HTTP_HOST</span></span>
* <span data-ttu-id="cfdd6-326">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="cfdd6-326">HTTP_REFERER</span></span>
* <span data-ttu-id="cfdd6-327">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="cfdd6-327">HTTP_URL</span></span>
* <span data-ttu-id="cfdd6-328">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="cfdd6-328">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="cfdd6-329">HTTPS</span><span class="sxs-lookup"><span data-stu-id="cfdd6-329">HTTPS</span></span>
* <span data-ttu-id="cfdd6-330">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="cfdd6-330">LOCAL_ADDR</span></span>
* <span data-ttu-id="cfdd6-331">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="cfdd6-331">QUERY_STRING</span></span>
* <span data-ttu-id="cfdd6-332">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="cfdd6-332">REMOTE_ADDR</span></span>
* <span data-ttu-id="cfdd6-333">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="cfdd6-333">REMOTE_PORT</span></span>
* <span data-ttu-id="cfdd6-334">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="cfdd6-334">REQUEST_FILENAME</span></span>
* <span data-ttu-id="cfdd6-335">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="cfdd6-335">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="cfdd6-336">Ayrıca, bir <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>üzerinden <xref:Microsoft.Extensions.FileProviders.IFileProvider> elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-336">You can also obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> via a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>.</span></span> <span data-ttu-id="cfdd6-337">Bu yaklaşım, yeniden yazma kuralları dosyalarınızın konumu için daha fazla esneklik sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-337">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="cfdd6-338">Yeniden yazma kuralları dosyalarınızın sağladığınız yoldaki sunucuya dağıtıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-338">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="cfdd6-339">Yöntem tabanlı kural</span><span class="sxs-lookup"><span data-stu-id="cfdd6-339">Method-based rule</span></span>

<span data-ttu-id="cfdd6-340">Bir yöntemde kendi kural mantığınızı uygulamak için <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-340">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> to implement your own rule logic in a method.</span></span> <span data-ttu-id="cfdd6-341">`Add` <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>kullanıma sunar ve bu <xref:Microsoft.AspNetCore.Http.HttpContext> yönteminizin kullanım için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-341">`Add` exposes the <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>, which makes available the <xref:Microsoft.AspNetCore.Http.HttpContext> for use in your method.</span></span> <span data-ttu-id="cfdd6-342">[Rewritecontext. Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*) , ek ardışık düzen işlemenin nasıl işlendiğini belirler.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-342">The [RewriteContext.Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*) determines how additional pipeline processing is handled.</span></span> <span data-ttu-id="cfdd6-343">Değeri aşağıdaki tabloda açıklanan <xref:Microsoft.AspNetCore.Rewrite.RuleResult> alanlardan birine ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-343">Set the value to one of the <xref:Microsoft.AspNetCore.Rewrite.RuleResult> fields described in the following table.</span></span>

| `RewriteContext.Result`              | <span data-ttu-id="cfdd6-344">Eylem</span><span class="sxs-lookup"><span data-stu-id="cfdd6-344">Action</span></span>                                                           |
| ------------------------------------ | ---------------------------------------------------------------- |
| <span data-ttu-id="cfdd6-345">`RuleResult.ContinueRules` (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="cfdd6-345">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="cfdd6-346">Kuralları uygulamaya devam edin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-346">Continue applying rules.</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="cfdd6-347">Kuralları uygulamayı durdurun ve yanıtı gönderin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-347">Stop applying rules and send the response.</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="cfdd6-348">Kuralları uygulamayı durdurun ve bağlamı bir sonraki ara yazılıma gönderin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-348">Stop applying rules and send the context to the next middleware.</span></span> |

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=14)]

<span data-ttu-id="cfdd6-349">Örnek uygulama, *. xml*ile biten yollar için istekleri yeniden yönlendiren bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-349">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="cfdd6-350">`/file.xml`için bir istek yapılırsa, istek `/xmlfiles/file.xml`yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-350">If a request is made for `/file.xml`, the request is redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="cfdd6-351">Durum kodu *301*olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-351">The status code is set to *301 - Moved Permanently*.</span></span> <span data-ttu-id="cfdd6-352">Tarayıcı */xmlfiles/File.xml*için yeni bir istek yaptığında, statik dosya ara yazılımı dosyayı *Wwwroot/xmlfiles* klasöründen istemciye sunar.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-352">When the browser makes a new request for */xmlfiles/file.xml*, Static File Middleware serves the file to the client from the *wwwroot/xmlfiles* folder.</span></span> <span data-ttu-id="cfdd6-353">Yeniden yönlendirme için, yanıtın durum kodunu açık olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-353">For a redirect, explicitly set the status code of the response.</span></span> <span data-ttu-id="cfdd6-354">Aksi takdirde, *200-ok* durum kodu döndürülür ve yeniden yönlendirme istemcide gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-354">Otherwise, a *200 - OK* status code is returned, and the redirect doesn't occur on the client.</span></span>

<span data-ttu-id="cfdd6-355">*RewriteRules.cs*:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-355">*RewriteRules.cs*:</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/RewriteRules.cs?name=snippet_RedirectXmlFileRequests&highlight=14-18)]

<span data-ttu-id="cfdd6-356">Bu yaklaşım ayrıca istekleri yeniden yazabilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-356">This approach can also rewrite requests.</span></span> <span data-ttu-id="cfdd6-357">Örnek uygulama, *dosya. txt* metin dosyasına *Wwwroot* klasöründen hizmeti sağlamak için herhangi bir metin dosyası isteğinin yolunu yeniden yazmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-357">The sample app demonstrates rewriting the path for any text file request to serve the *file.txt* text file from the *wwwroot* folder.</span></span> <span data-ttu-id="cfdd6-358">Statik dosya ara yazılımı, güncelleştirilmiş istek yoluna göre dosyayı sunar:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-358">Static File Middleware serves the file based on the updated request path:</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=15,22)]

<span data-ttu-id="cfdd6-359">*RewriteRules.cs*:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-359">*RewriteRules.cs*:</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/RewriteRules.cs?name=snippet_RewriteTextFileRequests&highlight=7-8)]

### <a name="irule-based-rule"></a><span data-ttu-id="cfdd6-360">Irule tabanlı kural</span><span class="sxs-lookup"><span data-stu-id="cfdd6-360">IRule-based rule</span></span>

<span data-ttu-id="cfdd6-361"><xref:Microsoft.AspNetCore.Rewrite.IRule> arabirimini uygulayan bir sınıfta kural mantığını kullanmak için <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-361">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> to use rule logic in a class that implements the <xref:Microsoft.AspNetCore.Rewrite.IRule> interface.</span></span> <span data-ttu-id="cfdd6-362">`IRule`, Yöntem tabanlı kural yaklaşımını kullanarak daha fazla esneklik sağlar.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-362">`IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="cfdd6-363">Uygulama sınıfınız <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> yöntemi için parametreleri geçirebilmeniz için bir Oluşturucu içerebilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-363">Your implementation class may include a constructor that allows you can pass in parameters for the <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> method.</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="cfdd6-364">`extension` ve `newPath` örnek uygulamadaki parametrelerin değerleri, çeşitli koşullara uyacak şekilde denetlenir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-364">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="cfdd6-365">`extension` bir değer içermeli ve değerin *. png*, *. jpg*veya *. gif*olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-365">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="cfdd6-366">`newPath` geçerli değilse, bir <xref:System.ArgumentException> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-366">If the `newPath` isn't valid, an <xref:System.ArgumentException> is thrown.</span></span> <span data-ttu-id="cfdd6-367">*Image. png*için bir istek yapılırsa, istek `/png-images/image.png`yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-367">If a request is made for *image.png*, the request is redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="cfdd6-368">*Image. jpg*için bir istek yapılırsa, istek `/jpg-images/image.jpg`yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-368">If a request is made for *image.jpg*, the request is redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="cfdd6-369">Durum kodu 301 olarak ayarlanır ve *kalıcı olarak taşınır*ve `context.Result` kuralları işlemeyi durdur ve yanıtı gönder olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-369">The status code is set to *301 - Moved Permanently*, and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/RewriteRules.cs?name=snippet_RedirectImageRequests)]

<span data-ttu-id="cfdd6-370">Özgün Istek: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-370">Original Request: `/image.png`</span></span>

![Görüntü. png için istekleri ve yanıtları izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="cfdd6-372">Özgün Istek: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-372">Original Request: `/image.jpg`</span></span>

![Image. jpg için istekleri ve yanıtları izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="cfdd6-374">Regex örnekleri</span><span class="sxs-lookup"><span data-stu-id="cfdd6-374">Regex examples</span></span>

| <span data-ttu-id="cfdd6-375">Hedef</span><span class="sxs-lookup"><span data-stu-id="cfdd6-375">Goal</span></span> | <span data-ttu-id="cfdd6-376">Regex dize &</span><span class="sxs-lookup"><span data-stu-id="cfdd6-376">Regex String &</span></span><br><span data-ttu-id="cfdd6-377">Match örneği</span><span class="sxs-lookup"><span data-stu-id="cfdd6-377">Match Example</span></span> | <span data-ttu-id="cfdd6-378">Değiştirme dizesi &</span><span class="sxs-lookup"><span data-stu-id="cfdd6-378">Replacement String &</span></span><br><span data-ttu-id="cfdd6-379">Çıkış Örneği</span><span class="sxs-lookup"><span data-stu-id="cfdd6-379">Output Example</span></span> |
| ---- | ------------------------------- | -------------------------------------- |
| <span data-ttu-id="cfdd6-380">Yolu QueryString 'e yeniden yazın</span><span class="sxs-lookup"><span data-stu-id="cfdd6-380">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="cfdd6-381">Eğik çizgiyi çıkar</span><span class="sxs-lookup"><span data-stu-id="cfdd6-381">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="cfdd6-382">Sondaki eğik çizgiyi zorla</span><span class="sxs-lookup"><span data-stu-id="cfdd6-382">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="cfdd6-383">Belirli istekleri yeniden yazmayı önleyin</span><span class="sxs-lookup"><span data-stu-id="cfdd6-383">Avoid rewriting specific requests</span></span> | <span data-ttu-id="cfdd6-384">`^(.*)(?<!\.axd)$` veya `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-384">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="cfdd6-385">Evet: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-385">Yes: `/resource.htm`</span></span><br><span data-ttu-id="cfdd6-386">Hayır: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-386">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="cfdd6-387">URL segmentlerini yeniden Düzenle</span><span class="sxs-lookup"><span data-stu-id="cfdd6-387">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="cfdd6-388">URL segmentini değiştirme</span><span class="sxs-lookup"><span data-stu-id="cfdd6-388">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="cfdd6-389">Bu belgede, ASP.NET Core uygulamalarında URL yeniden yazma ara yazılımı kullanma yönergeleriyle birlikte URL yeniden yazma tanıtılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-389">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

<span data-ttu-id="cfdd6-390">URL yeniden yazma, istek URL 'Lerini bir veya daha fazla önceden tanımlanmış kurala göre değiştirme işlemidir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-390">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="cfdd6-391">URL yeniden yazma, konumların ve adreslerin sıkı bir şekilde bağlanmaması için kaynak konumları ve adresleri arasında bir soyutlama oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-391">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses aren't tightly linked.</span></span> <span data-ttu-id="cfdd6-392">URL yeniden yazma işlemi birkaç senaryoda yararlı olur:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-392">URL rewriting is valuable in several scenarios to:</span></span>

* <span data-ttu-id="cfdd6-393">Sunucu kaynaklarını geçici olarak veya kalıcı olarak taşıyın veya değiştirin ve bu kaynakların kararlı konum belirleyicilerinin bakımını yapın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-393">Move or replace server resources temporarily or permanently and maintain stable locators for those resources.</span></span>
* <span data-ttu-id="cfdd6-394">İstek işlemeyi farklı uygulamalar arasında veya bir uygulamanın alanlarında bölme.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-394">Split request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="cfdd6-395">Gelen isteklerde URL segmentlerini kaldırın, ekleyin veya yeniden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-395">Remove, add, or reorganize URL segments on incoming requests.</span></span>
* <span data-ttu-id="cfdd6-396">Arama motoru Iyileştirmesi (SEO) için genel URL 'Leri iyileştirin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-396">Optimize public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="cfdd6-397">Ziyaretçilerin bir kaynak isteyerek döndürülen içeriği tahmin etmeye yardımcı olmak için kolay genel URL 'Lerin kullanılmasına izin verme.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-397">Permit the use of friendly public URLs to help visitors predict the content returned by requesting a resource.</span></span>
* <span data-ttu-id="cfdd6-398">Güvensiz istekleri güvenli uç noktalara yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-398">Redirect insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="cfdd6-399">Bir dış sitenin varlığı kendi içeriğine bağlayarak başka bir sitede barındırılan statik bir varlık kullandığı Hotlink 'i engelleyin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-399">Prevent hotlinking, where an external site uses a hosted static asset on another site by linking the asset into its own content.</span></span>

> [!NOTE]
> <span data-ttu-id="cfdd6-400">URL yeniden yazma, bir uygulamanın performansını azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-400">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="cfdd6-401">Uygun yerlerde kuralların sayısını ve karmaşıklığını sınırlayın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-401">Where feasible, limit the number and complexity of rules.</span></span>

<span data-ttu-id="cfdd6-402">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cfdd6-402">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="cfdd6-403">URL yeniden yönlendirme ve URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="cfdd6-403">URL redirect and URL rewrite</span></span>

<span data-ttu-id="cfdd6-404">*URL yeniden yönlendirme* ve *URL yeniden yazma* arasındaki ifade farkı daha hafif ancak istemcilere kaynak sağlamak için önemli etkileri vardır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-404">The difference in wording between *URL redirect* and *URL rewrite* is subtle but has important implications for providing resources to clients.</span></span> <span data-ttu-id="cfdd6-405">ASP.NET Core URL yeniden yazma ara yazılımı her ikisine de ihtiyacı verebilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-405">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="cfdd6-406">*URL yeniden yönlendirme* , istemcinin, ilk olarak istenen istemciden farklı bir adresteki kaynağa erişmesi için bir istemci tarafı işlemi içerir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-406">A *URL redirect* involves a client-side operation, where the client is instructed to access a resource at a different address than the client originally requested.</span></span> <span data-ttu-id="cfdd6-407">Bu, sunucuya gidiş dönüş gerektirir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-407">This requires a round trip to the server.</span></span> <span data-ttu-id="cfdd6-408">İstemci kaynak için yeni bir istek yaptığında, istemciye döndürülen yeniden yönlendirme URL 'SI tarayıcının adres çubuğunda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-408">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span>

<span data-ttu-id="cfdd6-409">`/resource` `/different-resource`*yeniden yönlendirilirse* sunucu, istemcinin yeniden yönlendirmenin geçici ya da kalıcı olduğunu belirten bir durum kodu ile `/different-resource` kaynağı elde etmesi gerektiğini yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-409">If `/resource` is *redirected* to `/different-resource`, the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span>

![Bir WebAPI hizmeti uç noktası, sürüm 1 ' den (v1) sunucudaki sürüm 2 ' ye (v2) geçici olarak değiştirildi.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="cfdd6-415">İstekleri farklı bir URL 'ye yönlendirirken, yanıtla birlikte durum kodunu belirterek yeniden yönlendirmenin kalıcı mi yoksa geçici mi olduğunu belirtin:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-415">When redirecting requests to a different URL, indicate whether the redirect is permanent or temporary by specifying the status code with the response:</span></span>

* <span data-ttu-id="cfdd6-416">*301-taşınan kalıcı* durum kodu, kaynağın yeni, kalıcı bir URL olduğu ve istemciye, gelecekteki tüm isteklerin kaynak için tüm istekleri yeni URL 'yi kullanması gerektiğini bildirmek istediğinizde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-416">The *301 - Moved Permanently* status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="cfdd6-417">*İstemci, 301 durum kodu alındığında yanıtı önbelleğe alabilir ve yeniden kullanabilir.*</span><span class="sxs-lookup"><span data-stu-id="cfdd6-417">*The client may cache and reuse the response when a 301 status code is received.*</span></span>

* <span data-ttu-id="cfdd6-418">*302-bulunan* durum kodu, yeniden yönlendirmenin geçici ya da genellikle değişikliğe tabi olduğu durumlarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-418">The *302 - Found* status code is used where the redirection is temporary or generally subject to change.</span></span> <span data-ttu-id="cfdd6-419">302 durum kodu, istemcinin URL 'YI depolayamadığını ve gelecekte kullanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-419">The 302 status code indicates to the client not to store the URL and use it in the future.</span></span>

<span data-ttu-id="cfdd6-420">Durum kodları hakkında daha fazla bilgi için bkz. [RFC 2616: durum kodu tanımları](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="cfdd6-420">For more information on status codes, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="cfdd6-421">*URL yeniden yazma* , istenen istemciden farklı bir kaynak adresinden kaynak sağlayan sunucu tarafı bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-421">A *URL rewrite* is a server-side operation that provides a resource from a different resource address than the client requested.</span></span> <span data-ttu-id="cfdd6-422">URL yeniden yazma, sunucuya gidiş dönüş gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-422">Rewriting a URL doesn't require a round trip to the server.</span></span> <span data-ttu-id="cfdd6-423">Yeniden yazan URL istemciye döndürülmüyor ve tarayıcının adres çubuğunda görünmüyor.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-423">The rewritten URL isn't returned to the client and doesn't appear in the browser's address bar.</span></span>

<span data-ttu-id="cfdd6-424">`/resource` `/different-resource`olarak *yeniden döndürülürse* , sunucu `/different-resource`kaynağı *dahili olarak* getirir ve döndürür.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-424">If `/resource` is *rewritten* to `/different-resource`, the server *internally* fetches and returns the resource at `/different-resource`.</span></span>

<span data-ttu-id="cfdd6-425">İstemci, yeniden yazan URL 'de kaynağı alabiliyor olsa da, istemci isteği yaptığında ve yanıtı aldığında kaynağın yeniden yazan URL 'de bulunduğunu bilgilendirmez.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-425">Although the client might be able to retrieve the resource at the rewritten URL, the client isn't informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Sunucu üzerinde sürüm 1 ' den (v1) sürüm 2 ' ye (v2) bir WebAPI hizmet uç noktası değiştirilmiştir.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="cfdd6-430">URL yeniden yazma örnek uygulaması</span><span class="sxs-lookup"><span data-stu-id="cfdd6-430">URL rewriting sample app</span></span>

<span data-ttu-id="cfdd6-431">[Örnek uygulamayla](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)birlikte YENIDEN yazma URL 'sinin özelliklerini inceleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-431">You can explore the features of the URL Rewriting Middleware with the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="cfdd6-432">Uygulama yeniden yönlendirme ve yeniden yazma kuralları uygular ve birkaç senaryo için yeniden yönlendirilen veya yeniden yazan URL 'YI gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-432">The app applies redirect and rewrite rules and shows the redirected or rewritten URL for several scenarios.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="cfdd6-433">URL yeniden yazma ara yazılımı ne zaman kullanılır</span><span class="sxs-lookup"><span data-stu-id="cfdd6-433">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="cfdd6-434">Aşağıdaki yaklaşımlardan birini kullandığımmdan URL yeniden yazma ara yazılımı kullanın:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-434">Use URL Rewriting Middleware when you're unable to use the following approaches:</span></span>

* [<span data-ttu-id="cfdd6-435">Windows Server 'da IIS ile URL yeniden yazma modülü</span><span class="sxs-lookup"><span data-stu-id="cfdd6-435">URL Rewrite module with IIS on Windows Server</span></span>](https://www.iis.net/downloads/microsoft/url-rewrite)
* [<span data-ttu-id="cfdd6-436">Apache Server 'da Apache mod_rewrite modülü</span><span class="sxs-lookup"><span data-stu-id="cfdd6-436">Apache mod_rewrite module on Apache Server</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="cfdd6-437">NGINX üzerinde URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="cfdd6-437">URL rewriting on Nginx</span></span>](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

<span data-ttu-id="cfdd6-438">Ayrıca, uygulama [http. sys sunucusunda](xref:fundamentals/servers/httpsys) barındırıldığı zaman ara yazılımı kullanın (eski adıyla webListener olarak adlandırılır).</span><span class="sxs-lookup"><span data-stu-id="cfdd6-438">Also, use the middleware when the app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener).</span></span>

<span data-ttu-id="cfdd6-439">IIS, Apache ve NGINX 'te sunucu tabanlı URL yeniden yazma teknolojilerini kullanmanın başlıca nedenleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-439">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, and Nginx are:</span></span>

* <span data-ttu-id="cfdd6-440">Ara yazılım bu modüllerin tüm özelliklerini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-440">The middleware doesn't support the full features of these modules.</span></span>

  <span data-ttu-id="cfdd6-441">Sunucu modüllerinin bazı özellikleri, IIS yeniden yazma modülünün `IsFile` ve `IsDirectory` kısıtlamaları gibi ASP.NET Core projelerle çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-441">Some of the features of the server modules don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="cfdd6-442">Bu senaryolarda, bunun yerine ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-442">In these scenarios, use the middleware instead.</span></span>
* <span data-ttu-id="cfdd6-443">Ara yazılım performansı büyük olasılıkla modüllerle eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-443">The performance of the middleware probably doesn't match that of the modules.</span></span>

  <span data-ttu-id="cfdd6-444">Sınama, performansı en iyi şekilde düşürür veya performans düşüklüğü göz ardı edilebilir olduğundan emin olmanın tek yoludur.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-444">Benchmarking is the only way to know for sure which approach degrades performance the most or if degraded performance is negligible.</span></span>

## <a name="package"></a><span data-ttu-id="cfdd6-445">Paket</span><span class="sxs-lookup"><span data-stu-id="cfdd6-445">Package</span></span>

<span data-ttu-id="cfdd6-446">Ara yazılımı projenize dahil etmek için, [Microsoft. aspnetcore. yeniden yazma](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) paketini içeren proje dosyasındaki [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app) öğesine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-446">To include the middleware in your project, add a package reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the project file, which contains the [Microsoft.AspNetCore.Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) package.</span></span>

<span data-ttu-id="cfdd6-447">`Microsoft.AspNetCore.App` metapackage kullanmadığınız durumlarda, `Microsoft.AspNetCore.Rewrite` paketine bir proje başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-447">When not using the `Microsoft.AspNetCore.App` metapackage, add a project reference to the `Microsoft.AspNetCore.Rewrite` package.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="cfdd6-448">Uzantı ve Seçenekler</span><span class="sxs-lookup"><span data-stu-id="cfdd6-448">Extension and options</span></span>

<span data-ttu-id="cfdd6-449">Yeniden yazma kurallarınızın her biri için uzantı yöntemleriyle [Rewriteoptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) sınıfının bir ÖRNEĞINI oluşturarak URL yeniden yazma ve yeniden yönlendirme kuralları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-449">Establish URL rewrite and redirect rules by creating an instance of the [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) class with extension methods for each of your rewrite rules.</span></span> <span data-ttu-id="cfdd6-450">Birden çok kuralı, işlenmeyi istediğiniz sırada zincirle.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-450">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="cfdd6-451">`RewriteOptions`, <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>ile istek ardışık düzenine eklendiği için, URL 'nin yeniden yazma ara moduna geçirilir:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-451">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

### <a name="redirect-non-www-to-www"></a><span data-ttu-id="cfdd6-452">Www olmayan www 'e yönlendirme</span><span class="sxs-lookup"><span data-stu-id="cfdd6-452">Redirect non-www to www</span></span>

<span data-ttu-id="cfdd6-453">Üç seçenek, uygulamanın`www` olmayan istekleri `www`yeniden yönlendirmesine izin verir:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-453">Three options permit the app to redirect non-`www` requests to `www`:</span></span>

* <span data-ttu-id="cfdd6-454">istek`www`değilse, <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*> &ndash; `www` alt etki alanına kalıcı olarak yeniden yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-454"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*> &ndash; Permanently redirect the request to the `www` subdomain if the request is non-`www`.</span></span> <span data-ttu-id="cfdd6-455">[Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) durum kodu ile yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-455">Redirects with a [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) status code.</span></span>

* <span data-ttu-id="cfdd6-456">gelen istek`www`değilse, isteği `www` alt etki alanına yönlendirmek &ndash; <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*>.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-456"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*> &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="cfdd6-457">[Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) durum kodu ile yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-457">Redirects with a [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) status code.</span></span> <span data-ttu-id="cfdd6-458">Aşırı yükleme, yanıt için durum kodu sağlamanıza izin verir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-458">An overload permits you to provide the status code for the response.</span></span> <span data-ttu-id="cfdd6-459">Bir durum kodu ataması için <xref:Microsoft.AspNetCore.Http.StatusCodes> sınıfının bir alanını kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-459">Use a field of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for a status code assignment.</span></span>

### <a name="url-redirect"></a><span data-ttu-id="cfdd6-460">URL yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="cfdd6-460">URL redirect</span></span>

<span data-ttu-id="cfdd6-461">İstekleri yeniden yönlendirmek için <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-461">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> to redirect requests.</span></span> <span data-ttu-id="cfdd6-462">İlk parametre gelen URL 'nin yolu ile eşleşen Regex içerir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-462">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="cfdd6-463">İkinci parametre değiştirme dizesidir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-463">The second parameter is the replacement string.</span></span> <span data-ttu-id="cfdd6-464">Varsa, üçüncü parametre durum kodunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-464">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="cfdd6-465">Durum kodunu belirtmezseniz, durum kodu varsayılan olarak *302-bulunur*; bu da kaynağın geçici olarak taşındığını veya değiştirildiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-465">If you don't specify the status code, the status code defaults to *302 - Found*, which indicates that the resource is temporarily moved or replaced.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=9)]

<span data-ttu-id="cfdd6-466">Geliştirici araçları etkinleştirilmiş bir tarayıcıda, `/redirect-rule/1234/5678`yol ile örnek uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-466">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="cfdd6-467">Regex `redirect-rule/(.*)`istek yoluyla eşleşir ve yol `/redirected/1234/5678`ile değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-467">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="cfdd6-468">Yeniden yönlendirme URL 'SI, *302 tarafından bulunan* bir durum kodu ile istemciye geri gönderilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-468">The redirect URL is sent back to the client with a *302 - Found* status code.</span></span> <span data-ttu-id="cfdd6-469">Tarayıcı, tarayıcının adres çubuğunda görüntülenen yeniden yönlendirme URL 'SI üzerinde yeni bir istek oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-469">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="cfdd6-470">Örnek uygulamadaki hiçbir kural, yeniden yönlendirme URL 'SI üzerinde eşleşmediğinden:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-470">Since no rules in the sample app match on the redirect URL:</span></span>

* <span data-ttu-id="cfdd6-471">İkinci istek uygulamadan *200-Tamam* yanıtı alır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-471">The second request receives a *200 - OK* response from the app.</span></span>
* <span data-ttu-id="cfdd6-472">Yanıtın gövdesi, yeniden yönlendirme URL 'sini gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-472">The body of the response shows the redirect URL.</span></span>

<span data-ttu-id="cfdd6-473">Bir URL *yeniden yönlendirildiğinde*sunucuya gidiş dönüş yapılır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-473">A round trip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="cfdd6-474">Yeniden yönlendirme kuralları oluştururken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-474">Be cautious when establishing redirect rules.</span></span> <span data-ttu-id="cfdd6-475">Yeniden yönlendirme kuralları, bir yeniden yönlendirmeden sonra dahil olmak üzere, uygulamaya yapılan her istekte değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-475">Redirect rules are evaluated on every request to the app, including after a redirect.</span></span> <span data-ttu-id="cfdd6-476">Yanlışlıkla *sonsuz yeniden yönlendirmeler döngüsü*oluşturmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-476">It's easy to accidentally create a *loop of infinite redirects*.</span></span>

<span data-ttu-id="cfdd6-477">Özgün Istek: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-477">Original Request: `/redirect-rule/1234/5678`</span></span>

![İstekleri ve yanıtları izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="cfdd6-479">Parantez içinde yer alan ifadenin kısmına bir *yakalama grubu*denir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-479">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="cfdd6-480">İfadenin nokta (`.`), *herhangi bir karakterle eşleşir*anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-480">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="cfdd6-481">Yıldız işareti (`*`) *, önceki karakterle sıfır veya daha fazla kez eşleşme*olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-481">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="cfdd6-482">Bu nedenle, URL 'nin son iki yol kesimleri `1234/5678`, yakalama grubu `(.*)`tarafından yakalanır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-482">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="cfdd6-483">`redirect-rule/` sonra istek URL 'sinde sağladığınız herhangi bir değer bu tek yakalama grubu tarafından yakalanır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-483">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="cfdd6-484">Değiştirme dizesinde, yakalanan gruplar, dolar işareti (`$`) ve ardından yakalamanın sıra numarası ile birlikte dizeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-484">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="cfdd6-485">İlk yakalama grubu değeri `$1`, ikincisi `$2`ile alınır ve Regex içindeki yakalama grupları için sırayla devam eder.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-485">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="cfdd6-486">Örnek uygulamadaki yeniden yönlendirme kuralı Regex bölümünde yalnızca bir tane yakalanan grup bulunur, bu nedenle değiştirme dizesinde yalnızca bir eklenmiş grup vardır. bu `$1`.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-486">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="cfdd6-487">Kural uygulandığında, URL `/redirected/1234/5678`olur.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-487">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="cfdd6-488">Güvenli bir uç noktaya URL yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="cfdd6-488">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="cfdd6-489">HTTP isteklerini HTTPS protokolünü kullanarak aynı konağa ve yola yönlendirmek için <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-489">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> to redirect HTTP requests to the same host and path using the HTTPS protocol.</span></span> <span data-ttu-id="cfdd6-490">Durum kodu sağlanmazsa, ara yazılım varsayılan olarak *302-bulunur*.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-490">If the status code isn't supplied, the middleware defaults to *302 - Found*.</span></span> <span data-ttu-id="cfdd6-491">Bağlantı noktası sağlanmazsa:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-491">If the port isn't supplied:</span></span>

* <span data-ttu-id="cfdd6-492">Ara yazılım varsayılan olarak `null`.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-492">The middleware defaults to `null`.</span></span>
* <span data-ttu-id="cfdd6-493">Düzen `https` (HTTPS protokolü) olarak değişir ve istemci kaynağa 443 numaralı bağlantı noktasında erişir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-493">The scheme changes to `https` (HTTPS protocol), and the client accesses the resource on port 443.</span></span>

<span data-ttu-id="cfdd6-494">Aşağıdaki örnek, durum kodunun *301-kalıcı olarak taşınacağını* ve bağlantı noktasını 5001 olarak nasıl değiştirileceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-494">The following example shows how to set the status code to *301 - Moved Permanently* and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="cfdd6-495">Güvenli olmayan istekleri, bağlantı noktası 443 üzerinde güvenli HTTPS protokolü ile aynı konağa ve yola yönlendirmek için <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-495">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> to redirect insecure requests to the same host and path with secure HTTPS protocol on port 443.</span></span> <span data-ttu-id="cfdd6-496">Ara yazılım durum kodunu 301 olarak ayarlar ve *kalıcı olarak taşınır*.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-496">The middleware sets the status code to *301 - Moved Permanently*.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="cfdd6-497">Ek yeniden yönlendirme kuralları gereksinimi olmadan güvenli bir uç noktaya yönlendirilirken, HTTPS yeniden yönlendirme ara yazılımı kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-497">When redirecting to a secure endpoint without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="cfdd6-498">Daha fazla bilgi için bkz. [https 'Yi zorla](xref:security/enforcing-ssl#require-https) konusu.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-498">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="cfdd6-499">Örnek uygulama `AddRedirectToHttps` veya `AddRedirectToHttpsPermanent`nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-499">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="cfdd6-500">Uzantı yöntemini `RewriteOptions`ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-500">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="cfdd6-501">Herhangi bir URL 'de uygulamaya güvenli olmayan bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-501">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="cfdd6-502">Otomatik olarak imzalanan sertifikanın güvenilmeyen tarayıcı güvenlik uyarısını kapatın veya sertifikaya güvenmek için bir özel durum oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-502">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="cfdd6-503">`AddRedirectToHttps(301, 5001)`kullanan özgün Istek: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-503">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![İstekleri ve yanıtları izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="cfdd6-505">`AddRedirectToHttpsPermanent`kullanan özgün Istek: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-505">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![İstekleri ve yanıtları izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="cfdd6-507">URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="cfdd6-507">URL rewrite</span></span>

<span data-ttu-id="cfdd6-508">URL 'Leri yeniden yazma kuralı oluşturmak için <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-508">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> to create a rule for rewriting URLs.</span></span> <span data-ttu-id="cfdd6-509">İlk parametre gelen URL yolundaki eşleşme için Regex içerir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-509">The first parameter contains the regex for matching on the incoming URL path.</span></span> <span data-ttu-id="cfdd6-510">İkinci parametre değiştirme dizesidir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-510">The second parameter is the replacement string.</span></span> <span data-ttu-id="cfdd6-511">`skipRemainingRules: {true|false}`üçüncü parametresi, geçerli kural uygulanmışsa ek yeniden yazma kurallarının atlanıp atlanmayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-511">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=10-11)]

<span data-ttu-id="cfdd6-512">Özgün Istek: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-512">Original Request: `/rewrite-rule/1234/5678`</span></span>

![İsteği ve yanıtı izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="cfdd6-514">İfadenin başındaki simgeyi seçtiğinizde (`^`), eşleşmesinin URL yolunun başlangıcında başladığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-514">The carat (`^`) at the beginning of the expression means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="cfdd6-515">Önceki örnekte, yeniden yönlendirme kuralıyla `redirect-rule/(.*)`, Regex başlangıcında simgeyi seçtiğinizde (`^`) yok.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-515">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat (`^`) at the start of the regex.</span></span> <span data-ttu-id="cfdd6-516">Bu nedenle, başarılı bir eşleşme için yoldaki tüm karakterler `redirect-rule/` önüne çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-516">Therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="cfdd6-517">Yol</span><span class="sxs-lookup"><span data-stu-id="cfdd6-517">Path</span></span>                               | <span data-ttu-id="cfdd6-518">Eşleştirme</span><span class="sxs-lookup"><span data-stu-id="cfdd6-518">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="cfdd6-519">Yes</span><span class="sxs-lookup"><span data-stu-id="cfdd6-519">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="cfdd6-520">Yes</span><span class="sxs-lookup"><span data-stu-id="cfdd6-520">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="cfdd6-521">Yes</span><span class="sxs-lookup"><span data-stu-id="cfdd6-521">Yes</span></span>   |

<span data-ttu-id="cfdd6-522">Yeniden yazma kuralı `^rewrite-rule/(\d+)/(\d+)`, yalnızca `rewrite-rule/`başlarsa yollarla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-522">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="cfdd6-523">Aşağıdaki tabloda, eşleşen farkı aklınızda.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-523">In the following table, note the difference in matching.</span></span>

| <span data-ttu-id="cfdd6-524">Yol</span><span class="sxs-lookup"><span data-stu-id="cfdd6-524">Path</span></span>                              | <span data-ttu-id="cfdd6-525">Eşleştirme</span><span class="sxs-lookup"><span data-stu-id="cfdd6-525">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="cfdd6-526">Yes</span><span class="sxs-lookup"><span data-stu-id="cfdd6-526">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="cfdd6-527">Hayır</span><span class="sxs-lookup"><span data-stu-id="cfdd6-527">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="cfdd6-528">Hayır</span><span class="sxs-lookup"><span data-stu-id="cfdd6-528">No</span></span>    |

<span data-ttu-id="cfdd6-529">İfadenin `^rewrite-rule/` kısmını izleyerek, `(\d+)/(\d+)`iki yakalama grubu vardır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-529">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="cfdd6-530">`\d`, *bir sayıyla (sayı) eşleşme*anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-530">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="cfdd6-531">Artı işareti (`+`) *bir veya daha fazla önceki karakterden eşleşiyor*demektir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-531">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="cfdd6-532">Bu nedenle, URL bir sayı içermeli ve ardından İleri eğik çizgi ve ardından başka bir sayı içermelidir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-532">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="cfdd6-533">Bu yakalama grupları, `$1` ve `$2`olarak yeniden yazan URL 'sine eklenir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-533">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="cfdd6-534">Yeniden yazma kuralı değiştirme dizesi yakalanan grupları sorgu dizesine koyar.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-534">The rewrite rule replacement string places the captured groups into the query string.</span></span> <span data-ttu-id="cfdd6-535">`/rewritten?var1=1234&var2=5678`kaynağı almak için istenen `/rewrite-rule/1234/5678` yolu yeniden yazıldı.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-535">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="cfdd6-536">Özgün istekte bir sorgu dizesi varsa, URL yeniden yazdığınızda korunur.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-536">If a query string is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="cfdd6-537">Kaynağı almak için sunucuya gidiş dönüş yok.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-537">There's no round trip to the server to obtain the resource.</span></span> <span data-ttu-id="cfdd6-538">Kaynak varsa, bu, alınır ve istemciye *200-ok* durum kodu ile döndürülür.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-538">If the resource exists, it's fetched and returned to the client with a *200 - OK* status code.</span></span> <span data-ttu-id="cfdd6-539">İstemci yeniden yönlendirmediği için tarayıcının adres çubuğundaki URL değişmez.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-539">Because the client isn't redirected, the URL in the browser's address bar doesn't change.</span></span> <span data-ttu-id="cfdd6-540">İstemciler, sunucuda bir URL yeniden yazma işleminin gerçekleştiğini algılayamaz.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-540">Clients can't detect that a URL rewrite operation occurred on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="cfdd6-541">Eşleşen kuralların hesaplama maliyeti ve uygulama yanıt süresini arttığı için mümkün olduğunda `skipRemainingRules: true` kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-541">Use `skipRemainingRules: true` whenever possible because matching rules is computationally expensive and increases app response time.</span></span> <span data-ttu-id="cfdd6-542">En hızlı uygulama yanıtı için:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-542">For the fastest app response:</span></span>
>
> * <span data-ttu-id="cfdd6-543">En sık eşleşen kuraldan en az sıklıkta eşleşen kurala göre yeniden yazma kuralları.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-543">Order rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="cfdd6-544">Bir eşleşme gerçekleştiğinde ve ek kural işleme gerekli olmadığında kalan kuralların işlenmesini atlayın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-544">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-mod_rewrite"></a><span data-ttu-id="cfdd6-545">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="cfdd6-545">Apache mod_rewrite</span></span>

<span data-ttu-id="cfdd6-546"><xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*>ile Apache mod_rewrite kuralları uygulayın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-546">Apply Apache mod_rewrite rules with <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*>.</span></span> <span data-ttu-id="cfdd6-547">Kurallar dosyasının uygulamayla birlikte dağıtıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-547">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="cfdd6-548">Daha fazla bilgi ve mod_rewrite kuralları örnekleri için bkz. [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="cfdd6-548">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

<span data-ttu-id="cfdd6-549">*ApacheModRewrite. txt* kuralları dosyasındaki kuralları okumak için bir <xref:System.IO.StreamReader> kullanılır:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-549">A <xref:System.IO.StreamReader> is used to read the rules from the *ApacheModRewrite.txt* rules file:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=3-4,12)]

<span data-ttu-id="cfdd6-550">Örnek uygulama, istekleri `/redirected?id=$1``/apache-mod-rules-redirect/(.\*)` yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-550">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="cfdd6-551">Yanıt durum kodu *302-bulundu*.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-551">The response status code is *302 - Found*.</span></span>

[!code[](url-rewriting/samples/2.x/SampleApp/ApacheModRewrite.txt)]

<span data-ttu-id="cfdd6-552">Özgün Istek: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-552">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![İstekleri ve yanıtları izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_apache_mod_redirect.png)

<span data-ttu-id="cfdd6-554">Ara yazılım aşağıdaki Apache mod_rewrite sunucu değişkenlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-554">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="cfdd6-555">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="cfdd6-555">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="cfdd6-556">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="cfdd6-556">HTTP_ACCEPT</span></span>
* <span data-ttu-id="cfdd6-557">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="cfdd6-557">HTTP_CONNECTION</span></span>
* <span data-ttu-id="cfdd6-558">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="cfdd6-558">HTTP_COOKIE</span></span>
* <span data-ttu-id="cfdd6-559">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="cfdd6-559">HTTP_FORWARDED</span></span>
* <span data-ttu-id="cfdd6-560">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="cfdd6-560">HTTP_HOST</span></span>
* <span data-ttu-id="cfdd6-561">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="cfdd6-561">HTTP_REFERER</span></span>
* <span data-ttu-id="cfdd6-562">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="cfdd6-562">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="cfdd6-563">HTTPS</span><span class="sxs-lookup"><span data-stu-id="cfdd6-563">HTTPS</span></span>
* <span data-ttu-id="cfdd6-564">IPV6</span><span class="sxs-lookup"><span data-stu-id="cfdd6-564">IPV6</span></span>
* <span data-ttu-id="cfdd6-565">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="cfdd6-565">QUERY_STRING</span></span>
* <span data-ttu-id="cfdd6-566">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="cfdd6-566">REMOTE_ADDR</span></span>
* <span data-ttu-id="cfdd6-567">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="cfdd6-567">REMOTE_PORT</span></span>
* <span data-ttu-id="cfdd6-568">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="cfdd6-568">REQUEST_FILENAME</span></span>
* <span data-ttu-id="cfdd6-569">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="cfdd6-569">REQUEST_METHOD</span></span>
* <span data-ttu-id="cfdd6-570">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="cfdd6-570">REQUEST_SCHEME</span></span>
* <span data-ttu-id="cfdd6-571">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="cfdd6-571">REQUEST_URI</span></span>
* <span data-ttu-id="cfdd6-572">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="cfdd6-572">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="cfdd6-573">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="cfdd6-573">SERVER_ADDR</span></span>
* <span data-ttu-id="cfdd6-574">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="cfdd6-574">SERVER_PORT</span></span>
* <span data-ttu-id="cfdd6-575">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="cfdd6-575">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="cfdd6-576">TIME</span><span class="sxs-lookup"><span data-stu-id="cfdd6-576">TIME</span></span>
* <span data-ttu-id="cfdd6-577">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="cfdd6-577">TIME_DAY</span></span>
* <span data-ttu-id="cfdd6-578">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="cfdd6-578">TIME_HOUR</span></span>
* <span data-ttu-id="cfdd6-579">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="cfdd6-579">TIME_MIN</span></span>
* <span data-ttu-id="cfdd6-580">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="cfdd6-580">TIME_MON</span></span>
* <span data-ttu-id="cfdd6-581">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="cfdd6-581">TIME_SEC</span></span>
* <span data-ttu-id="cfdd6-582">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="cfdd6-582">TIME_WDAY</span></span>
* <span data-ttu-id="cfdd6-583">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="cfdd6-583">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="cfdd6-584">IIS URL yeniden yazma modülü kuralları</span><span class="sxs-lookup"><span data-stu-id="cfdd6-584">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="cfdd6-585">IIS URL yeniden yazma modülü için geçerli olan kural kümesini kullanmak için <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-585">To use the same rule set that applies to the IIS URL Rewrite Module, use <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>.</span></span> <span data-ttu-id="cfdd6-586">Kurallar dosyasının uygulamayla birlikte dağıtıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-586">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="cfdd6-587">Windows Server IIS 'de çalışırken uygulamanın *Web. config* dosyasını kullanmak için ara yazılımı yönlendirmeyin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-587">Don't direct the middleware to use the app's *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="cfdd6-588">IIS ile, IIS yeniden yazma modülüyle çakışmalardan kaçınmak için bu kuralların uygulamanın *Web. config* dosyası dışında depolanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-588">With IIS, these rules should be stored outside of the app's *web.config* file in order to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="cfdd6-589">Daha fazla bilgi ve IIS URL yeniden yazma modülü kurallarının örnekleri için bkz. [Using URL yeniden yazma modülü 2,0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) ve [URL yeniden yazma modülü yapılandırma başvurusu](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="cfdd6-589">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

<span data-ttu-id="cfdd6-590"><xref:System.IO.StreamReader> *Iisurlyeniden yazma. xml* kuralları dosyasındaki kuralları okumak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-590">A <xref:System.IO.StreamReader> is used to read the rules from the *IISUrlRewrite.xml* rules file:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5-6,13)]

<span data-ttu-id="cfdd6-591">Örnek uygulama, `/iis-rules-rewrite/(.*)` olan istekleri `/rewritten?id=$1`olarak yeniden yazar.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-591">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="cfdd6-592">Yanıt, istemciye *200-ok* durum kodu ile gönderilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-592">The response is sent to the client with a *200 - OK* status code.</span></span>

[!code-xml[](url-rewriting/samples/2.x/SampleApp/IISUrlRewrite.xml)]

<span data-ttu-id="cfdd6-593">Özgün Istek: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-593">Original Request: `/iis-rules-rewrite/1234`</span></span>

![İsteği ve yanıtı izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="cfdd6-595">Uygulamanızı istenmeyen yollarla etkileyebilecek sunucu düzeyi kurallara sahip etkin bir IIS yeniden yazma modülünüzün olması halinde, bir uygulama için IIS yeniden yazma modülünü devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-595">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="cfdd6-596">Daha fazla bilgi için bkz. [IIS modüllerini devre dışı bırakma](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="cfdd6-596">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="cfdd6-597">Desteklenmeyen özellikler</span><span class="sxs-lookup"><span data-stu-id="cfdd6-597">Unsupported features</span></span>

<span data-ttu-id="cfdd6-598">ASP.NET Core 2. x ile yayınlanan ara yazılım, aşağıdaki IIS URL yeniden yazma modülü özelliklerini desteklemez:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-598">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="cfdd6-599">Giden Kuralları</span><span class="sxs-lookup"><span data-stu-id="cfdd6-599">Outbound Rules</span></span>
* <span data-ttu-id="cfdd6-600">Özel sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="cfdd6-600">Custom Server Variables</span></span>
* <span data-ttu-id="cfdd6-601">Joker karakterler</span><span class="sxs-lookup"><span data-stu-id="cfdd6-601">Wildcards</span></span>
* <span data-ttu-id="cfdd6-602">LogRewrittenUrl 'Si</span><span class="sxs-lookup"><span data-stu-id="cfdd6-602">LogRewrittenUrl</span></span>

#### <a name="supported-server-variables"></a><span data-ttu-id="cfdd6-603">Desteklenen sunucu değişkenleri</span><span class="sxs-lookup"><span data-stu-id="cfdd6-603">Supported server variables</span></span>

<span data-ttu-id="cfdd6-604">Ara yazılım aşağıdaki IIS URL yeniden yazma modülü sunucu değişkenlerini destekler:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-604">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="cfdd6-605">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="cfdd6-605">CONTENT_LENGTH</span></span>
* <span data-ttu-id="cfdd6-606">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="cfdd6-606">CONTENT_TYPE</span></span>
* <span data-ttu-id="cfdd6-607">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="cfdd6-607">HTTP_ACCEPT</span></span>
* <span data-ttu-id="cfdd6-608">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="cfdd6-608">HTTP_CONNECTION</span></span>
* <span data-ttu-id="cfdd6-609">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="cfdd6-609">HTTP_COOKIE</span></span>
* <span data-ttu-id="cfdd6-610">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="cfdd6-610">HTTP_HOST</span></span>
* <span data-ttu-id="cfdd6-611">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="cfdd6-611">HTTP_REFERER</span></span>
* <span data-ttu-id="cfdd6-612">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="cfdd6-612">HTTP_URL</span></span>
* <span data-ttu-id="cfdd6-613">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="cfdd6-613">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="cfdd6-614">HTTPS</span><span class="sxs-lookup"><span data-stu-id="cfdd6-614">HTTPS</span></span>
* <span data-ttu-id="cfdd6-615">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="cfdd6-615">LOCAL_ADDR</span></span>
* <span data-ttu-id="cfdd6-616">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="cfdd6-616">QUERY_STRING</span></span>
* <span data-ttu-id="cfdd6-617">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="cfdd6-617">REMOTE_ADDR</span></span>
* <span data-ttu-id="cfdd6-618">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="cfdd6-618">REMOTE_PORT</span></span>
* <span data-ttu-id="cfdd6-619">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="cfdd6-619">REQUEST_FILENAME</span></span>
* <span data-ttu-id="cfdd6-620">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="cfdd6-620">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="cfdd6-621">Ayrıca, bir <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>üzerinden <xref:Microsoft.Extensions.FileProviders.IFileProvider> elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-621">You can also obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> via a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>.</span></span> <span data-ttu-id="cfdd6-622">Bu yaklaşım, yeniden yazma kuralları dosyalarınızın konumu için daha fazla esneklik sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-622">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="cfdd6-623">Yeniden yazma kuralları dosyalarınızın sağladığınız yoldaki sunucuya dağıtıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-623">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="cfdd6-624">Yöntem tabanlı kural</span><span class="sxs-lookup"><span data-stu-id="cfdd6-624">Method-based rule</span></span>

<span data-ttu-id="cfdd6-625">Bir yöntemde kendi kural mantığınızı uygulamak için <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-625">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> to implement your own rule logic in a method.</span></span> <span data-ttu-id="cfdd6-626">`Add` <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>kullanıma sunar ve bu <xref:Microsoft.AspNetCore.Http.HttpContext> yönteminizin kullanım için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-626">`Add` exposes the <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>, which makes available the <xref:Microsoft.AspNetCore.Http.HttpContext> for use in your method.</span></span> <span data-ttu-id="cfdd6-627">[Rewritecontext. Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*) , ek ardışık düzen işlemenin nasıl işlendiğini belirler.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-627">The [RewriteContext.Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*) determines how additional pipeline processing is handled.</span></span> <span data-ttu-id="cfdd6-628">Değeri aşağıdaki tabloda açıklanan <xref:Microsoft.AspNetCore.Rewrite.RuleResult> alanlardan birine ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-628">Set the value to one of the <xref:Microsoft.AspNetCore.Rewrite.RuleResult> fields described in the following table.</span></span>

| `RewriteContext.Result`              | <span data-ttu-id="cfdd6-629">Eylem</span><span class="sxs-lookup"><span data-stu-id="cfdd6-629">Action</span></span>                                                           |
| ------------------------------------ | ---------------------------------------------------------------- |
| <span data-ttu-id="cfdd6-630">`RuleResult.ContinueRules` (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="cfdd6-630">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="cfdd6-631">Kuralları uygulamaya devam edin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-631">Continue applying rules.</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="cfdd6-632">Kuralları uygulamayı durdurun ve yanıtı gönderin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-632">Stop applying rules and send the response.</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="cfdd6-633">Kuralları uygulamayı durdurun ve bağlamı bir sonraki ara yazılıma gönderin.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-633">Stop applying rules and send the context to the next middleware.</span></span> |

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=14)]

<span data-ttu-id="cfdd6-634">Örnek uygulama, *. xml*ile biten yollar için istekleri yeniden yönlendiren bir yöntemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-634">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="cfdd6-635">`/file.xml`için bir istek yapılırsa, istek `/xmlfiles/file.xml`yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-635">If a request is made for `/file.xml`, the request is redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="cfdd6-636">Durum kodu *301*olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-636">The status code is set to *301 - Moved Permanently*.</span></span> <span data-ttu-id="cfdd6-637">Tarayıcı */xmlfiles/File.xml*için yeni bir istek yaptığında, statik dosya ara yazılımı dosyayı *Wwwroot/xmlfiles* klasöründen istemciye sunar.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-637">When the browser makes a new request for */xmlfiles/file.xml*, Static File Middleware serves the file to the client from the *wwwroot/xmlfiles* folder.</span></span> <span data-ttu-id="cfdd6-638">Yeniden yönlendirme için, yanıtın durum kodunu açık olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-638">For a redirect, explicitly set the status code of the response.</span></span> <span data-ttu-id="cfdd6-639">Aksi takdirde, *200-ok* durum kodu döndürülür ve yeniden yönlendirme istemcide gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-639">Otherwise, a *200 - OK* status code is returned, and the redirect doesn't occur on the client.</span></span>

<span data-ttu-id="cfdd6-640">*RewriteRules.cs*:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-640">*RewriteRules.cs*:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectXmlFileRequests&highlight=14-18)]

<span data-ttu-id="cfdd6-641">Bu yaklaşım ayrıca istekleri yeniden yazabilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-641">This approach can also rewrite requests.</span></span> <span data-ttu-id="cfdd6-642">Örnek uygulama, *dosya. txt* metin dosyasına *Wwwroot* klasöründen hizmeti sağlamak için herhangi bir metin dosyası isteğinin yolunu yeniden yazmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-642">The sample app demonstrates rewriting the path for any text file request to serve the *file.txt* text file from the *wwwroot* folder.</span></span> <span data-ttu-id="cfdd6-643">Statik dosya ara yazılımı, güncelleştirilmiş istek yoluna göre dosyayı sunar:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-643">Static File Middleware serves the file based on the updated request path:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=15,22)]

<span data-ttu-id="cfdd6-644">*RewriteRules.cs*:</span><span class="sxs-lookup"><span data-stu-id="cfdd6-644">*RewriteRules.cs*:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RewriteTextFileRequests&highlight=7-8)]

### <a name="irule-based-rule"></a><span data-ttu-id="cfdd6-645">Irule tabanlı kural</span><span class="sxs-lookup"><span data-stu-id="cfdd6-645">IRule-based rule</span></span>

<span data-ttu-id="cfdd6-646"><xref:Microsoft.AspNetCore.Rewrite.IRule> arabirimini uygulayan bir sınıfta kural mantığını kullanmak için <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-646">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> to use rule logic in a class that implements the <xref:Microsoft.AspNetCore.Rewrite.IRule> interface.</span></span> <span data-ttu-id="cfdd6-647">`IRule`, Yöntem tabanlı kural yaklaşımını kullanarak daha fazla esneklik sağlar.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-647">`IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="cfdd6-648">Uygulama sınıfınız <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> yöntemi için parametreleri geçirebilmeniz için bir Oluşturucu içerebilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-648">Your implementation class may include a constructor that allows you can pass in parameters for the <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> method.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="cfdd6-649">`extension` ve `newPath` örnek uygulamadaki parametrelerin değerleri, çeşitli koşullara uyacak şekilde denetlenir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-649">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="cfdd6-650">`extension` bir değer içermeli ve değerin *. png*, *. jpg*veya *. gif*olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-650">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="cfdd6-651">`newPath` geçerli değilse, bir <xref:System.ArgumentException> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-651">If the `newPath` isn't valid, an <xref:System.ArgumentException> is thrown.</span></span> <span data-ttu-id="cfdd6-652">*Image. png*için bir istek yapılırsa, istek `/png-images/image.png`yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-652">If a request is made for *image.png*, the request is redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="cfdd6-653">*Image. jpg*için bir istek yapılırsa, istek `/jpg-images/image.jpg`yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-653">If a request is made for *image.jpg*, the request is redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="cfdd6-654">Durum kodu 301 olarak ayarlanır ve *kalıcı olarak taşınır*ve `context.Result` kuralları işlemeyi durdur ve yanıtı gönder olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="cfdd6-654">The status code is set to *301 - Moved Permanently*, and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectImageRequests)]

<span data-ttu-id="cfdd6-655">Özgün Istek: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-655">Original Request: `/image.png`</span></span>

![Görüntü. png için istekleri ve yanıtları izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="cfdd6-657">Özgün Istek: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-657">Original Request: `/image.jpg`</span></span>

![Image. jpg için istekleri ve yanıtları izleyen Geliştirici Araçları tarayıcı penceresi](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="cfdd6-659">Regex örnekleri</span><span class="sxs-lookup"><span data-stu-id="cfdd6-659">Regex examples</span></span>

| <span data-ttu-id="cfdd6-660">Hedef</span><span class="sxs-lookup"><span data-stu-id="cfdd6-660">Goal</span></span> | <span data-ttu-id="cfdd6-661">Regex dize &</span><span class="sxs-lookup"><span data-stu-id="cfdd6-661">Regex String &</span></span><br><span data-ttu-id="cfdd6-662">Match örneği</span><span class="sxs-lookup"><span data-stu-id="cfdd6-662">Match Example</span></span> | <span data-ttu-id="cfdd6-663">Değiştirme dizesi &</span><span class="sxs-lookup"><span data-stu-id="cfdd6-663">Replacement String &</span></span><br><span data-ttu-id="cfdd6-664">Çıkış Örneği</span><span class="sxs-lookup"><span data-stu-id="cfdd6-664">Output Example</span></span> |
| ---- | ------------------------------- | -------------------------------------- |
| <span data-ttu-id="cfdd6-665">Yolu QueryString 'e yeniden yazın</span><span class="sxs-lookup"><span data-stu-id="cfdd6-665">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="cfdd6-666">Eğik çizgiyi çıkar</span><span class="sxs-lookup"><span data-stu-id="cfdd6-666">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="cfdd6-667">Sondaki eğik çizgiyi zorla</span><span class="sxs-lookup"><span data-stu-id="cfdd6-667">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="cfdd6-668">Belirli istekleri yeniden yazmayı önleyin</span><span class="sxs-lookup"><span data-stu-id="cfdd6-668">Avoid rewriting specific requests</span></span> | <span data-ttu-id="cfdd6-669">`^(.*)(?<!\.axd)$` veya `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-669">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="cfdd6-670">Evet: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-670">Yes: `/resource.htm`</span></span><br><span data-ttu-id="cfdd6-671">Hayır: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="cfdd6-671">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="cfdd6-672">URL segmentlerini yeniden Düzenle</span><span class="sxs-lookup"><span data-stu-id="cfdd6-672">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="cfdd6-673">URL segmentini değiştirme</span><span class="sxs-lookup"><span data-stu-id="cfdd6-673">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="cfdd6-674">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="cfdd6-674">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="cfdd6-675">.NET 'teki normal ifadeler</span><span class="sxs-lookup"><span data-stu-id="cfdd6-675">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="cfdd6-676">Normal ifade dili-hızlı başvuru</span><span class="sxs-lookup"><span data-stu-id="cfdd6-676">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="cfdd6-677">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="cfdd6-677">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="cfdd6-678">URL yeniden yazma modülünü kullanma 2,0 (IIS için)</span><span class="sxs-lookup"><span data-stu-id="cfdd6-678">Using Url Rewrite Module 2.0 (for IIS)</span></span>](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="cfdd6-679">URL yeniden yazma modülü yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="cfdd6-679">URL Rewrite Module Configuration Reference</span></span>](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="cfdd6-680">IIS URL yeniden yazma modülü Forumu</span><span class="sxs-lookup"><span data-stu-id="cfdd6-680">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="cfdd6-681">Basit URL yapısını saklama</span><span class="sxs-lookup"><span data-stu-id="cfdd6-681">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="cfdd6-682">10 URL yeniden yazma Ipuçları ve püf noktaları</span><span class="sxs-lookup"><span data-stu-id="cfdd6-682">10 URL Rewriting Tips and Tricks</span></span>](https://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="cfdd6-683">Eğik çizgi veya eğik çizgi</span><span class="sxs-lookup"><span data-stu-id="cfdd6-683">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
