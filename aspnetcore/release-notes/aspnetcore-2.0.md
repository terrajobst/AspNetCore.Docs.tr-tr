---
title: ASP.NET Core 2.0 yenilikleri
author: rick-anderson
description: ASP.NET Core 2.0 yenilikleri hakkında bilgi edinin.
ms.author: riande
ms.date: 07/10/2017
uid: aspnetcore-2.0
ms.openlocfilehash: 07e490fd844f0f9044afce37651ef680ed0c894d
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898355"
---
# <a name="whats-new-in-aspnet-core-20"></a><span data-ttu-id="541dd-103">ASP.NET Core 2.0 yenilikleri</span><span class="sxs-lookup"><span data-stu-id="541dd-103">What's new in ASP.NET Core 2.0</span></span>

<span data-ttu-id="541dd-104">Bu makalede, ASP.NET Core 2. 0'da, en önemli değişiklikler ile ilgili belgelere bağlantılar vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="541dd-104">This article highlights the most significant changes in ASP.NET Core 2.0, with links to relevant documentation.</span></span>

## <a name="razor-pages"></a><span data-ttu-id="541dd-105">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="541dd-105">Razor Pages</span></span>

<span data-ttu-id="541dd-106">Razor sayfaları, ASP.NET Core MVC sayfası odaklı senaryolar daha kolay ve daha üretken kodlama sağlayan, yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="541dd-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="541dd-107">Daha fazla bilgi için giriş ve öğreticiye bakın:</span><span class="sxs-lookup"><span data-stu-id="541dd-107">For more information, see the introduction and tutorial:</span></span>

* [<span data-ttu-id="541dd-108">Razor Pages’e giriş</span><span class="sxs-lookup"><span data-stu-id="541dd-108">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="541dd-109">Razor Sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="541dd-109">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a><span data-ttu-id="541dd-110">ASP.NET Core metapackage</span><span class="sxs-lookup"><span data-stu-id="541dd-110">ASP.NET Core metapackage</span></span>

<span data-ttu-id="541dd-111">Yeni bir ASP.NET Core metapackage yapılan ve ASP.NET Core ve Entity Framework Core takımlar, iç ve 3. taraf bağımlılıklarıyla tarafından desteklenen paketler içerir.</span><span class="sxs-lookup"><span data-stu-id="541dd-111">A new ASP.NET Core metapackage includes all of the packages made and supported by the ASP.NET Core and Entity Framework Core teams, along with their internal and 3rd-party dependencies.</span></span> <span data-ttu-id="541dd-112">Artık tek tek ASP.NET Core, paketi tarafından özellikleri seçmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="541dd-112">You no longer need to choose individual ASP.NET Core features by package.</span></span> <span data-ttu-id="541dd-113">Tüm özellikler dahil [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) paket.</span><span class="sxs-lookup"><span data-stu-id="541dd-113">All features are included in the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package.</span></span> <span data-ttu-id="541dd-114">Varsayılan şablonlar bu paketi kullanın.</span><span class="sxs-lookup"><span data-stu-id="541dd-114">The default templates use this package.</span></span>

<span data-ttu-id="541dd-115">Daha fazla bilgi için [Microsoft.AspNetCore.All metapackage ASP.NET Core 2.0 için](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="541dd-115">For more information, see [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0](xref:fundamentals/metapackage).</span></span>

## <a name="runtime-store"></a><span data-ttu-id="541dd-116">Çalışma zamanı Store</span><span class="sxs-lookup"><span data-stu-id="541dd-116">Runtime Store</span></span>

<span data-ttu-id="541dd-117">Kullanan uygulamalar `Microsoft.AspNetCore.All` metapackage otomatik olarak yeni bir .NET Core çalışma zamanı Store avantajlarından yararlanın.</span><span class="sxs-lookup"><span data-stu-id="541dd-117">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the new .NET Core Runtime Store.</span></span> <span data-ttu-id="541dd-118">Store, ASP.NET Core 2.0 uygulamaları çalıştırmak için gerekli olan tüm çalışma zamanı varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="541dd-118">The Store contains all the runtime assets needed to run ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="541dd-119">Kullanırken `Microsoft.AspNetCore.All` metapackage, hiçbir varlıklarından başvurulan bir ASP.NET Core NuGet paket, uygulama ile dağıtılır, çünkü bunlar zaten hedef sistemde bulunurlar.</span><span class="sxs-lookup"><span data-stu-id="541dd-119">When you use the `Microsoft.AspNetCore.All` metapackage, no assets from the referenced ASP.NET Core NuGet packages are deployed with the application because they already reside on the target system.</span></span> <span data-ttu-id="541dd-120">Çalışma zamanı Store varlıkları önceden derlenmiş uygulama başlatma çıkış süresini kısaltın.</span><span class="sxs-lookup"><span data-stu-id="541dd-120">The assets in the Runtime Store are also precompiled to improve application startup time.</span></span>

<span data-ttu-id="541dd-121">Daha fazla bilgi için [çalışma zamanı deposu](/dotnet/core/deploying/runtime-store)</span><span class="sxs-lookup"><span data-stu-id="541dd-121">For more information, see [Runtime store](/dotnet/core/deploying/runtime-store)</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="541dd-122">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="541dd-122">.NET Standard 2.0</span></span>

<span data-ttu-id="541dd-123">ASP.NET Core 2.0 paketleri .NET Standard 2.0 hedefleyin.</span><span class="sxs-lookup"><span data-stu-id="541dd-123">The ASP.NET Core 2.0 packages target .NET Standard 2.0.</span></span> <span data-ttu-id="541dd-124">Paketleri diğer .NET Standard 2.0 kitaplıkları tarafından başvurulabilir ve .NET, .NET Core 2.0 ve .NET Framework 4.6.1 dahil olmak üzere, .NET Standard 2.0 uyumlu uygulamalar üzerinde çalıştırabilirler.</span><span class="sxs-lookup"><span data-stu-id="541dd-124">The packages can be referenced by other .NET Standard 2.0 libraries, and they can run on .NET Standard 2.0-compliant implementations of .NET, including .NET Core 2.0 and .NET Framework 4.6.1.</span></span> 

<span data-ttu-id="541dd-125">`Microsoft.AspNetCore.All` İle .NET Core 2.0 çalışma zamanı Store kullanılmaya yönelik olduğundan metapackage .NET Core 2.0 yalnızca hedefler.</span><span class="sxs-lookup"><span data-stu-id="541dd-125">The `Microsoft.AspNetCore.All` metapackage targets .NET Core 2.0 only, because it's intended to be used with the .NET Core 2.0 Runtime Store.</span></span>

## <a name="configuration-update"></a><span data-ttu-id="541dd-126">Yapılandırma güncelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="541dd-126">Configuration update</span></span>

<span data-ttu-id="541dd-127">Bir `IConfiguration` örneği, varsayılan olarak ASP.NET Core 2.0 Hizmetleri kapsayıcıya eklenir.</span><span class="sxs-lookup"><span data-stu-id="541dd-127">An `IConfiguration` instance is added to the services container by default in ASP.NET Core 2.0.</span></span> <span data-ttu-id="541dd-128">`IConfiguration` Hizmetleri kapsayıcı kapsayıcıdan yapılandırma değerlerini almak uygulamaları kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="541dd-128">`IConfiguration` in the services container makes it easier for applications to retrieve configuration values from the container.</span></span>

<span data-ttu-id="541dd-129">Planlanan belgeleri durumu hakkında daha fazla bilgi için bkz [GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/3387).</span><span class="sxs-lookup"><span data-stu-id="541dd-129">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/3387).</span></span>

## <a name="logging-update"></a><span data-ttu-id="541dd-130">Güncelleştirme günlüğü</span><span class="sxs-lookup"><span data-stu-id="541dd-130">Logging update</span></span>

<span data-ttu-id="541dd-131">ASP.NET Core 2.0 sürümünde, günlüğe kaydetme varsayılan olarak bağımlılık ekleme (dı) sisteme eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="541dd-131">In ASP.NET Core 2.0, logging is incorporated into the dependency injection (DI) system by default.</span></span> <span data-ttu-id="541dd-132">Sağlayıcıları ekleyin ve filtrelemeyi yapılandırma *Program.cs* dosyası içinde yerine *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="541dd-132">You add providers and configure filtering in the *Program.cs* file instead of in the *Startup.cs* file.</span></span> <span data-ttu-id="541dd-133">Ve varsayılan `ILoggerFactory` hem sağlayıcı çapraz filtreleme ve özel sağlayıcı filtreleme için esnek bir yaklaşım sağlayan bir şekilde filtrelemeyi kullanın.</span><span class="sxs-lookup"><span data-stu-id="541dd-133">And the default `ILoggerFactory` supports filtering in a way that lets you use one flexible approach for both cross-provider filtering and specific-provider filtering.</span></span>

<span data-ttu-id="541dd-134">Daha fazla bilgi için [günlük giriş](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="541dd-134">For more information, see [Introduction to Logging](xref:fundamentals/logging/index).</span></span>

## <a name="authentication-update"></a><span data-ttu-id="541dd-135">Kimlik doğrulama güncelleştirmesini</span><span class="sxs-lookup"><span data-stu-id="541dd-135">Authentication update</span></span>

<span data-ttu-id="541dd-136">Yeni bir kimlik doğrulama modeli DI kullanarak bir uygulama için kimlik doğrulamasını yapılandırmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="541dd-136">A new authentication model makes it easier to configure authentication for an application using DI.</span></span>

<span data-ttu-id="541dd-137">Yeni şablonlar web apps için kimlik doğrulaması yapılandırmak için kullanılabilir ve web API'leri [Azure AD B2C] kullanarak (https://azure.microsoft.com/services/active-directory-b2c/).</span><span class="sxs-lookup"><span data-stu-id="541dd-137">New templates are available for configuring authentication for web apps and web APIs using [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/).</span></span>

<span data-ttu-id="541dd-138">Planlanan belgeleri durumu hakkında daha fazla bilgi için bkz [GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/3054).</span><span class="sxs-lookup"><span data-stu-id="541dd-138">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/3054).</span></span>

## <a name="identity-update"></a><span data-ttu-id="541dd-139">Kimlik güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="541dd-139">Identity update</span></span>

<span data-ttu-id="541dd-140">Oluşturmayı kolay güvenli kolaylaştırdık web API'leri kimliğini kullanarak ASP.NET Core 2. 0 '.</span><span class="sxs-lookup"><span data-stu-id="541dd-140">We've made it easier to build secure web APIs using Identity in ASP.NET Core 2.0.</span></span> <span data-ttu-id="541dd-141">Web API'leri kullanarak erişmek için erişim belirteçleri edinebilir [Microsoft Authentication Library (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span><span class="sxs-lookup"><span data-stu-id="541dd-141">You can acquire access tokens for accessing your web APIs using the [Microsoft Authentication Library (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span></span>

<span data-ttu-id="541dd-142">Kimlik doğrulaması değişiklikleri 2.0 hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="541dd-142">For more information on authentication changes in 2.0, see the following resources:</span></span>

* [<span data-ttu-id="541dd-143">Hesap onaylama ve parola kurtarma ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="541dd-143">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="541dd-144">ASP.NET core'da authenticator uygulamaları için QR kodu oluşturmayı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="541dd-144">Enable QR Code generation for authenticator apps in ASP.NET Core</span></span>](xref:security/authentication/identity-enable-qrcodes)
* [<span data-ttu-id="541dd-145">ASP.NET Core 2.0 kimlik doğrulaması ve kimlik geçirme</span><span class="sxs-lookup"><span data-stu-id="541dd-145">Migrate Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a><span data-ttu-id="541dd-146">SPA şablonları</span><span class="sxs-lookup"><span data-stu-id="541dd-146">SPA templates</span></span>

<span data-ttu-id="541dd-147">Tek sayfa uygulama (SPA) proje şablonları Angular, Aurelia Knockout.js React.js ve Redux ile React.js için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="541dd-147">Single Page Application (SPA) project templates for Angular, Aurelia, Knockout.js, React.js, and React.js with Redux are available.</span></span> <span data-ttu-id="541dd-148">Angular şablonu Angular 4'e güncelleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="541dd-148">The Angular template has been updated to Angular 4.</span></span> <span data-ttu-id="541dd-149">Angular ve React şablonları, varsayılan olarak mevcuttur; diğer şablonları alma hakkında daha fazla bilgi için bkz: [yeni bir SPA proje oluşturma](xref:client-side/spa-services#creating-a-new-project).</span><span class="sxs-lookup"><span data-stu-id="541dd-149">The Angular and React templates are available by default; for information about how to get the other templates, see [Create a new SPA project](xref:client-side/spa-services#creating-a-new-project).</span></span> <span data-ttu-id="541dd-150">ASP.NET core'da bir SPA oluşturma hakkında daha fazla bilgi için bkz: [oluşturma tek sayfalı uygulamalar için JavaScriptServices kullanma](xref:client-side/spa-services).</span><span class="sxs-lookup"><span data-stu-id="541dd-150">For information about how to build a SPA in ASP.NET Core, see [Use JavaScriptServices for Creating Single Page Applications](xref:client-side/spa-services).</span></span>

## <a name="kestrel-improvements"></a><span data-ttu-id="541dd-151">Kestrel'i geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="541dd-151">Kestrel improvements</span></span>

<span data-ttu-id="541dd-152">Kestrel'i web sunucusu, Internet'e yönelik olarak daha uygun hale getiren yeni özelliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="541dd-152">The Kestrel web server has new features that make it more suitable as an Internet-facing server.</span></span> <span data-ttu-id="541dd-153">İçinde sunucu kısıtlaması yapılandırma seçeneklerinin bir sayı eklenir `KestrelServerOptions` sınıf yeni `Limits` özelliği.</span><span class="sxs-lookup"><span data-stu-id="541dd-153">A number of server constraint configuration options are added in the `KestrelServerOptions` class's new `Limits` property.</span></span> <span data-ttu-id="541dd-154">Aşağıdaki sınırlar ekleyin:</span><span class="sxs-lookup"><span data-stu-id="541dd-154">Add limits for the following:</span></span>

* <span data-ttu-id="541dd-155">En fazla istemci bağlantısı</span><span class="sxs-lookup"><span data-stu-id="541dd-155">Maximum client connections</span></span>
* <span data-ttu-id="541dd-156">En fazla istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="541dd-156">Maximum request body size</span></span>
* <span data-ttu-id="541dd-157">En az bir istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="541dd-157">Minimum request body data rate</span></span>

<span data-ttu-id="541dd-158">Daha fazla bilgi için [Kestrel web sunucusu uygulaması ASP.NET core'da](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="541dd-158">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel).</span></span>

## <a name="weblistener-renamed-to-httpsys"></a><span data-ttu-id="541dd-159">WebListener HTTP.sys için yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="541dd-159">WebListener renamed to HTTP.sys</span></span>

<span data-ttu-id="541dd-160">Paketleri `Microsoft.AspNetCore.Server.WebListener` ve `Microsoft.Net.Http.Server` yeni bir paket birleştirilir `Microsoft.AspNetCore.Server.HttpSys`.</span><span class="sxs-lookup"><span data-stu-id="541dd-160">The packages `Microsoft.AspNetCore.Server.WebListener` and `Microsoft.Net.Http.Server` have been merged into a new package `Microsoft.AspNetCore.Server.HttpSys`.</span></span> <span data-ttu-id="541dd-161">Ad alanlarını eşleşecek şekilde güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="541dd-161">The namespaces have been updated to match.</span></span>

<span data-ttu-id="541dd-162">Daha fazla bilgi için [HTTP.sys sunucu uygulamasından ASP.NET Core web](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="541dd-162">For more information, see [HTTP.sys web server implementation in ASP.NET Core](xref:fundamentals/servers/httpsys).</span></span>

## <a name="enhanced-http-header-support"></a><span data-ttu-id="541dd-163">HTTP üst bilgisi için gelişmiş destek</span><span class="sxs-lookup"><span data-stu-id="541dd-163">Enhanced HTTP header support</span></span>

<span data-ttu-id="541dd-164">MVC aktarmaya kullanırken bir `FileStreamResult` veya `FileContentResult`, şimdi Ayarla seçeneğine sahip bir `ETag` veya `LastModified` , iletme içeriğe tarih.</span><span class="sxs-lookup"><span data-stu-id="541dd-164">When using MVC to transmit a `FileStreamResult` or a `FileContentResult`, you now have the option to set an `ETag` or a `LastModified` date on the content you transmit.</span></span> <span data-ttu-id="541dd-165">Bu değerleri üzerinde kod döndürülen içerikle aşağıdakine benzer ayarlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="541dd-165">You can set these values on the returned content with code similar to the following:</span></span>

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

<span data-ttu-id="541dd-166">Ziyaretçilerinizin için döndürülen dosya için uygun HTTP üst bilgileri ile donatılmış `ETag` ve `LastModified` değerleri.</span><span class="sxs-lookup"><span data-stu-id="541dd-166">The file returned to your visitors will be decorated with the appropriate HTTP headers for the `ETag` and `LastModified` values.</span></span>

<span data-ttu-id="541dd-167">Bir uygulama ziyaretçi isteği içeriği, bir aralık isteği üst bilgisi ile ASP.NET Core istek tanır ve üst bilgi olarak işler.</span><span class="sxs-lookup"><span data-stu-id="541dd-167">If an application visitor requests content with a Range Request header, ASP.NET Core recognizes the request and handles the header.</span></span> <span data-ttu-id="541dd-168">Talep edilen içeriği kısmen edinebilir, ASP.NET Core, uygun şekilde atlar ve yalnızca istenen bayt kümesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="541dd-168">If the requested content can be partially delivered, ASP.NET Core appropriately skips and returns just the requested set of bytes.</span></span> <span data-ttu-id="541dd-169">Herhangi bir özel işleyicileri uyarlayabilir veya bu özelliği işlemek için yöntemlerinizi yazmanız gerekmez; Bu otomatik olarak sizin yerinize gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="541dd-169">You don't need to write any special handlers into your methods to adapt or handle this feature; it's automatically handled for you.</span></span>

## <a name="hosting-startup-and-application-insights"></a><span data-ttu-id="541dd-170">Başlangıç ve Application Insights'ı barındırma</span><span class="sxs-lookup"><span data-stu-id="541dd-170">Hosting startup and Application Insights</span></span>

<span data-ttu-id="541dd-171">Barındırma ortamları artık ek paket bağımlılıkları ekleme ve kod uygulama başlatma sırasında açıkça bir bağımlılık olması veya herhangi bir yöntem çağrısı gerek olmadan uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="541dd-171">Hosting environments can now inject extra package dependencies and execute code during application startup, without the application needing to explicitly take a dependency or call any methods.</span></span> <span data-ttu-id="541dd-172">Bu özellik, söz konusu ortama "ışık yukarı" özelliklere belirli ortamlara uygulamanın önceden bilmenize gerek olmadan etkinleştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="541dd-172">This feature can be used to enable certain environments to "light-up" features unique to that environment without the application needing to know ahead of time.</span></span> 

<span data-ttu-id="541dd-173">ASP.NET Core 2.0 sürümünde bu özellik otomatik olarak (sonra seçim) ve Visual Studio'da hata ayıklama sırasında Application Insights Tanılama'yı etkinleştirmek için kullanılan Azure uygulama hizmetleri çalışırken.</span><span class="sxs-lookup"><span data-stu-id="541dd-173">In ASP.NET Core 2.0, this feature is used to automatically enable Application Insights diagnostics when debugging in Visual Studio and (after opting in) when running in Azure App Services.</span></span> <span data-ttu-id="541dd-174">Sonuç olarak, proje şablonları artık Application Insights paketlerini ve kod varsayılan olarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="541dd-174">As a result, the project templates no longer add Application Insights packages and code by default.</span></span>

<span data-ttu-id="541dd-175">Planlanan belgeleri durumu hakkında daha fazla bilgi için bkz [GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/3389).</span><span class="sxs-lookup"><span data-stu-id="541dd-175">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/3389).</span></span>

## <a name="automatic-use-of-anti-forgery-tokens"></a><span data-ttu-id="541dd-176">Sahteciliğe karşı koruma belirteçleri otomatik kullanımı</span><span class="sxs-lookup"><span data-stu-id="541dd-176">Automatic use of anti-forgery tokens</span></span>

<span data-ttu-id="541dd-177">ASP.NET Core, siteler arası istek sahteciliği (XSRF) saldırılarını HTML olarak kodlanacak içeriği varsayılan olarak, ancak fazladan bir adım yardımcı olması için geçen yeni sürüm ile her zaman olmuştur.</span><span class="sxs-lookup"><span data-stu-id="541dd-177">ASP.NET Core has always helped HTML-encode content by default, but with the new version an extra step is taken to help prevent cross-site request forgery (XSRF) attacks.</span></span> <span data-ttu-id="541dd-178">ASP.NET Core, artık varsayılan olarak sahteciliğe karşı koruma belirteçleri yayma ve bunları form POST eylemlerinin ve ek yapılandırma olmaksızın sayfaları doğrulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="541dd-178">ASP.NET Core will now emit anti-forgery tokens by default and validate them on form POST actions and pages without extra configuration.</span></span>

<span data-ttu-id="541dd-179">Daha fazla bilgi için [önlemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="541dd-179">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="automatic-precompilation"></a><span data-ttu-id="541dd-180">Otomatik kod ön derlemesi</span><span class="sxs-lookup"><span data-stu-id="541dd-180">Automatic precompilation</span></span>

<span data-ttu-id="541dd-181">Razor görünümü önceden derleme yayımlama sırasında uygulama ve yayımlama boyutu azaltma, varsayılan olarak etkin başlangıç zamanı.</span><span class="sxs-lookup"><span data-stu-id="541dd-181">Razor view pre-compilation is enabled during publish by default, reducing the publish output size and application startup time.</span></span>

<span data-ttu-id="541dd-182">Daha fazla bilgi için [Razor görünüm derlemesi ve içinde ASP.NET Core ön derleme](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="541dd-182">For more information, see [Razor view compilation and precompilation in ASP.NET Core](xref:mvc/views/view-compilation).</span></span>

## <a name="razor-support-for-c-71"></a><span data-ttu-id="541dd-183">C# 7.1 Razor desteği</span><span class="sxs-lookup"><span data-stu-id="541dd-183">Razor support for C# 7.1</span></span>

<span data-ttu-id="541dd-184">Razor görünüm altyapısı yeni Roslyn derleyicisi ile çalışacak şekilde güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="541dd-184">The Razor view engine has been updated to work with the new Roslyn compiler.</span></span> <span data-ttu-id="541dd-185">C# 7.1 varsayılan ifadeleri, Çıkarsanan demet adları ve türlerle desen eşleştirme gibi özellikler için destek içerir.</span><span class="sxs-lookup"><span data-stu-id="541dd-185">That includes support for C# 7.1 features like Default Expressions, Inferred Tuple Names, and Pattern-Matching with Generics.</span></span> <span data-ttu-id="541dd-186">Projenizde C# 7.1 kullanmak için proje dosyanızda aşağıdaki özelliği ekleyin ve ardından çözümü yeniden yükleyin:</span><span class="sxs-lookup"><span data-stu-id="541dd-186">To use C# 7.1 in your project, add the following property in your project file and then reload the solution:</span></span>

```xml
<LangVersion>latest</LangVersion>
```

<span data-ttu-id="541dd-187">C# 7.1 özelliklerini durumu hakkında daha fazla bilgi için bkz. [Roslyn GitHub deposunu](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span><span class="sxs-lookup"><span data-stu-id="541dd-187">For information about the status of C# 7.1 features, see [the Roslyn GitHub repository](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span></span>

## <a name="other-documentation-updates-for-20"></a><span data-ttu-id="541dd-188">2.0 için diğer belge güncelleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="541dd-188">Other documentation updates for 2.0</span></span>

* [<span data-ttu-id="541dd-189">Visual Studio yayımlama profilleri için ASP.NET Core uygulaması dağıtımı</span><span class="sxs-lookup"><span data-stu-id="541dd-189">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>](xref:host-and-deploy/visual-studio-publish-profiles)
* [<span data-ttu-id="541dd-190">Anahtar Yönetimi</span><span class="sxs-lookup"><span data-stu-id="541dd-190">Key Management</span></span>](xref:security/data-protection/implementation/key-management)
* [<span data-ttu-id="541dd-191">Facebook kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="541dd-191">Configure Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="541dd-192">Twitter kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="541dd-192">Configure Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="541dd-193">Google kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="541dd-193">Configure Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="541dd-194">Microsoft Account kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="541dd-194">Configure Microsoft Account authentication</span></span>](xref:security/authentication/microsoft-logins)

## <a name="migration-guidance"></a><span data-ttu-id="541dd-195">Geçiş Kılavuzu</span><span class="sxs-lookup"><span data-stu-id="541dd-195">Migration guidance</span></span>

<span data-ttu-id="541dd-196">ASP.NET Core 1.x ASP.NET Core 2.0 uygulamaları geçirme konusunda yönergeler için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="541dd-196">For guidance on how to migrate ASP.NET Core 1.x applications to ASP.NET Core 2.0, see the following resources:</span></span>

* [<span data-ttu-id="541dd-197">ASP.NET Core geçiş 1.x ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="541dd-197">Migrate from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/index)
* [<span data-ttu-id="541dd-198">ASP.NET Core 2.0 kimlik doğrulaması ve kimlik geçirme</span><span class="sxs-lookup"><span data-stu-id="541dd-198">Migrate Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a><span data-ttu-id="541dd-199">Ek Bilgiler</span><span class="sxs-lookup"><span data-stu-id="541dd-199">Additional Information</span></span>

<span data-ttu-id="541dd-200">Değişikliklerin tam listesi için bkz. [ASP.NET Core 2.0 sürüm notları](https://github.com/aspnet/Home/releases/tag/2.0.0).</span><span class="sxs-lookup"><span data-stu-id="541dd-200">For the complete list of changes, see the [ASP.NET Core 2.0 Release Notes](https://github.com/aspnet/Home/releases/tag/2.0.0).</span></span>

<span data-ttu-id="541dd-201">ASP.NET Core geliştirme takımın ilerleme durumu ve planları ile bağlanmak için etkinliğindeki [ASP.NET topluluğu toplantısında](https://live.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="541dd-201">To connect with the ASP.NET Core development team's progress and plans, tune in to the [ASP.NET Community Standup](https://live.asp.net/).</span></span>
