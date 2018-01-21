---
title: ASP.NET Core 2. 0 ' yenilikler nelerdir?
author: rick-anderson
description: ASP.NET Core 2. 0 ' yenilikler nelerdir?
ms.author: riande
manager: wpickett
ms.date: 07/10/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: aspnetcore-2.0
ms.openlocfilehash: 992afc2766e817ef007e20ade44e3ddd1d404f90
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="whats-new-in-aspnet-core-20"></a><span data-ttu-id="c2666-103">ASP.NET Core 2. 0 ' yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="c2666-103">What's new in ASP.NET Core 2.0</span></span>

<span data-ttu-id="c2666-104">Bu makalede, ASP.NET Core 2. 0'da, en önemli değişiklikler ile ilgili belgelere bağlantılar vurgular.</span><span class="sxs-lookup"><span data-stu-id="c2666-104">This article highlights the most significant changes in ASP.NET Core 2.0, with links to relevant documentation.</span></span>

## <a name="razor-pages"></a><span data-ttu-id="c2666-105">Razor sayfalarının</span><span class="sxs-lookup"><span data-stu-id="c2666-105">Razor Pages</span></span>

<span data-ttu-id="c2666-106">Razor sayfalarının sayfa odaklı senaryoları daha kolay ve daha üretken kodlama yapar ASP.NET Core MVC yeni özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="c2666-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="c2666-107">Daha fazla bilgi için bkz: giriş ve öğretici:</span><span class="sxs-lookup"><span data-stu-id="c2666-107">For more information, see the introduction and tutorial:</span></span>

* [<span data-ttu-id="c2666-108">Razor sayfalarının giriş</span><span class="sxs-lookup"><span data-stu-id="c2666-108">Introduction to Razor Pages</span></span>](xref:mvc/razor-pages/index)
* [<span data-ttu-id="c2666-109">Razor Sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="c2666-109">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a><span data-ttu-id="c2666-110">ASP.NET Core metapackage</span><span class="sxs-lookup"><span data-stu-id="c2666-110">ASP.NET Core metapackage</span></span>

<span data-ttu-id="c2666-111">Yeni bir ASP.NET Core metapackage yapılan ve iç ve 3. taraf bağımlılıklarını birlikte ASP.NET Core ve Entity Framework Çekirdek ekipleri tarafından desteklenen paketleri içerir.</span><span class="sxs-lookup"><span data-stu-id="c2666-111">A new ASP.NET Core metapackage includes all of the packages made and supported by the ASP.NET Core and Entity Framework Core teams, along with their internal and 3rd-party dependencies.</span></span> <span data-ttu-id="c2666-112">Artık tek tek ASP.NET Core paketiyle özellikleri seçmek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="c2666-112">You no longer need to choose individual ASP.NET Core features by package.</span></span> <span data-ttu-id="c2666-113">Tüm özellikleri dahil edilen [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) paket.</span><span class="sxs-lookup"><span data-stu-id="c2666-113">All features are included in the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package.</span></span> <span data-ttu-id="c2666-114">Bu paket varsayılan şablonları kullanın.</span><span class="sxs-lookup"><span data-stu-id="c2666-114">The default templates use this package.</span></span>

<span data-ttu-id="c2666-115">Daha fazla bilgi için bkz: [Microsoft.AspNetCore.All metapackage ASP.NET Core 2.0 için](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="c2666-115">For more information, see [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0](xref:fundamentals/metapackage).</span></span>

## <a name="runtime-store"></a><span data-ttu-id="c2666-116">Çalışma zamanı deposu</span><span class="sxs-lookup"><span data-stu-id="c2666-116">Runtime Store</span></span>

<span data-ttu-id="c2666-117">Kullanan uygulamalar `Microsoft.AspNetCore.All` metapackage otomatik olarak avantajından yeni bir .NET çekirdeği çalışma zamanı deposu.</span><span class="sxs-lookup"><span data-stu-id="c2666-117">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the new .NET Core Runtime Store.</span></span> <span data-ttu-id="c2666-118">Depo ASP.NET Core 2.0 uygulamaları çalıştırmak için gereken tüm çalışma zamanı varlıklarını içerir.</span><span class="sxs-lookup"><span data-stu-id="c2666-118">The Store contains all the runtime assets needed to run ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="c2666-119">Kullandığınızda `Microsoft.AspNetCore.All` metapackage, başvurulan ASP.NET Core NuGet paketlerini hiçbir varlıklarından, uygulama ile dağıtılır, hedef sistemde zaten bulundukları olduğundan.</span><span class="sxs-lookup"><span data-stu-id="c2666-119">When you use the `Microsoft.AspNetCore.All` metapackage, no assets from the referenced ASP.NET Core NuGet packages are deployed with the application because they already reside on the target system.</span></span> <span data-ttu-id="c2666-120">Çalışma zamanı deposu varlıkları, ayrıca uygulama başlangıç zamanını geliştirmek için önceden derlenmiş.</span><span class="sxs-lookup"><span data-stu-id="c2666-120">The assets in the Runtime Store are also precompiled to improve application startup time.</span></span>

<span data-ttu-id="c2666-121">Daha fazla bilgi için bkz: [çalışma zamanı deposu](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)</span><span class="sxs-lookup"><span data-stu-id="c2666-121">For more information, see [Runtime store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="c2666-122">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="c2666-122">.NET Standard 2.0</span></span>

<span data-ttu-id="c2666-123">ASP.NET Core 2.0 paketleri .NET standart 2.0 hedefleyin.</span><span class="sxs-lookup"><span data-stu-id="c2666-123">The ASP.NET Core 2.0 packages target .NET Standard 2.0.</span></span> <span data-ttu-id="c2666-124">Paketleri diğer .NET standart 2.0 kitaplıkları tarafından başvurulabilir ve .NET, .NET Core 2.0 ve .NET Framework 4.6.1 dahil olmak üzere, .NET standart 2.0 ile uyumlu uygulamalar üzerinde çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="c2666-124">The packages can be referenced by other .NET Standard 2.0 libraries, and they can run on .NET Standard 2.0-compliant implementations of .NET, including .NET Core 2.0 and .NET Framework 4.6.1.</span></span> 

<span data-ttu-id="c2666-125">`Microsoft.AspNetCore.All` .NET Core 2.0 çalışma zamanı deposuyla kullanılmak üzere tasarlandığından metapackage .NET Core 2.0 yalnızca hedefler.</span><span class="sxs-lookup"><span data-stu-id="c2666-125">The `Microsoft.AspNetCore.All` metapackage targets .NET Core 2.0 only, because it is intended to be used with the .NET Core 2.0 Runtime Store.</span></span>

## <a name="configuration-update"></a><span data-ttu-id="c2666-126">Yapılandırma güncelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="c2666-126">Configuration update</span></span>

<span data-ttu-id="c2666-127">Bir `IConfiguration` örneği, varsayılan ASP.NET Core 2.0 olarak Hizmetleri kapsayıcıya eklenir.</span><span class="sxs-lookup"><span data-stu-id="c2666-127">An `IConfiguration` instance is added to the services container by default in ASP.NET Core 2.0.</span></span> <span data-ttu-id="c2666-128">`IConfiguration`Hizmetler kapsayıcısı kapsayıcıdan yapılandırma değerlerini almak üzere uygulamalar için kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="c2666-128">`IConfiguration` in the services container makes it easier for applications to retrieve configuration values from the container.</span></span>

<span data-ttu-id="c2666-129">Planlanan belgelerine durumu hakkında daha fazla bilgi için bkz: [GitHub sorunu](https://github.com/aspnet/Docs/issues/3387).</span><span class="sxs-lookup"><span data-stu-id="c2666-129">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3387).</span></span>

## <a name="logging-update"></a><span data-ttu-id="c2666-130">Güncelleştirme günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="c2666-130">Logging update</span></span>

<span data-ttu-id="c2666-131">ASP.NET Core 2. 0'günlüğe kaydetme varsayılan olarak bağımlılık ekleme (dı) sisteme eklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="c2666-131">In ASP.NET Core 2.0, logging is incorporated into the dependency injection (DI) system by default.</span></span> <span data-ttu-id="c2666-132">Sağlayıcıları eklemek ve filtrelemeyi yapılandırmak *Program.cs* dosyası içinde yerine *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="c2666-132">You add providers and configure filtering in the *Program.cs* file instead of in the *Startup.cs* file.</span></span> <span data-ttu-id="c2666-133">Ve varsayılan `ILoggerFactory` olanak sağlayan bir şekilde filtrelenmesini destekler, hem sağlayıcı çapraz filtreleme ve özel sağlayıcı filtreleme için esnek bir yaklaşım kullanın.</span><span class="sxs-lookup"><span data-stu-id="c2666-133">And the default `ILoggerFactory` supports filtering in a way that lets you use one flexible approach for both cross-provider filtering and specific-provider filtering.</span></span>

<span data-ttu-id="c2666-134">Daha fazla bilgi için bkz: [günlük giriş](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="c2666-134">For more information, see [Introduction to Logging](xref:fundamentals/logging/index).</span></span>

## <a name="authentication-update"></a><span data-ttu-id="c2666-135">Kimlik doğrulama güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="c2666-135">Authentication update</span></span>

<span data-ttu-id="c2666-136">Yeni bir kimlik doğrulama modeli dı kullanarak bir uygulama kimlik doğrulamasını yapılandırmak daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="c2666-136">A new authentication model makes it easier to configure authentication for an application using DI.</span></span>

<span data-ttu-id="c2666-137">Yeni şablonlar web uygulamaları için kimlik doğrulaması yapılandırmak için kullanılabilir ve [Azure AD B2C] kullanarak API web (https://azure.microsoft.com/services/active-directory-b2c/).</span><span class="sxs-lookup"><span data-stu-id="c2666-137">New templates are available for configuring authentication for web apps and web APIs using [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/).</span></span>

<span data-ttu-id="c2666-138">Planlanan belgelerine durumu hakkında daha fazla bilgi için bkz: [GitHub sorunu](https://github.com/aspnet/Docs/issues/3054).</span><span class="sxs-lookup"><span data-stu-id="c2666-138">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3054).</span></span>

## <a name="identity-update"></a><span data-ttu-id="c2666-139">Kimlik güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="c2666-139">Identity update</span></span>

<span data-ttu-id="c2666-140">Bu yapı daha kolay güvenli yaptık ASP.NET Core 2. 0 ' kimliğini kullanarak API web.</span><span class="sxs-lookup"><span data-stu-id="c2666-140">We've made it easier to build secure web APIs using Identity in ASP.NET Core 2.0.</span></span> <span data-ttu-id="c2666-141">Web API kullanarak erişmek için erişim belirteçleri elde edebilirsiniz [Microsoft kimlik doğrulama kitaplığı (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span><span class="sxs-lookup"><span data-stu-id="c2666-141">You can acquire access tokens for accessing your web APIs using the [Microsoft Authentication Library (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span></span>

<span data-ttu-id="c2666-142">2.0 kimlik doğrulama değişiklikleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="c2666-142">For more information on authentication changes in 2.0, see the following resources:</span></span>

* [<span data-ttu-id="c2666-143">Hesap doğrulama ve ASP.NET Core parola kurtarma</span><span class="sxs-lookup"><span data-stu-id="c2666-143">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="c2666-144">ASP.NET Core Doğrulayıcı uygulamalar için etkinleştirme QR kodu oluşturma</span><span class="sxs-lookup"><span data-stu-id="c2666-144">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>](xref:security/authentication/identity-enable-qrcodes)
* [<span data-ttu-id="c2666-145">Geçirme kimlik doğrulaması ve ASP.NET Core 2.0 için kimliği</span><span class="sxs-lookup"><span data-stu-id="c2666-145">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a><span data-ttu-id="c2666-146">SPA şablonları</span><span class="sxs-lookup"><span data-stu-id="c2666-146">SPA templates</span></span>

<span data-ttu-id="c2666-147">Angular, Aurelia, Knockout.js, React.js ve React.js Redux ile tek sayfa uygulama (SPA) proje şablonları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c2666-147">Single Page Application (SPA) project templates for Angular, Aurelia, Knockout.js, React.js, and React.js with Redux are available.</span></span> <span data-ttu-id="c2666-148">Açısal şablon Açısal 4'e güncelleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="c2666-148">The Angular template has been updated to Angular 4.</span></span> <span data-ttu-id="c2666-149">Angular ve tepki şablonları varsayılan olarak bulunur; diğer şablon alma hakkında daha fazla bilgi için bkz: [yeni SPA projesi oluşturma](xref:client-side/spa-services#creating-a-new-project).</span><span class="sxs-lookup"><span data-stu-id="c2666-149">The Angular and React templates are available by default; for information about how to get the other templates, see [Creating a new SPA project](xref:client-side/spa-services#creating-a-new-project).</span></span> <span data-ttu-id="c2666-150">ASP.NET Core içinde SPA oluşturma hakkında daha fazla bilgi için bkz: [kullanarak JavaScriptServices tek sayfa uygulamaları oluşturma için](xref:client-side/spa-services).</span><span class="sxs-lookup"><span data-stu-id="c2666-150">For information about how to build a SPA in ASP.NET Core, see [Using JavaScriptServices for Creating Single Page Applications](xref:client-side/spa-services).</span></span>

## <a name="kestrel-improvements"></a><span data-ttu-id="c2666-151">Kestrel geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="c2666-151">Kestrel improvements</span></span>

<span data-ttu-id="c2666-152">Kestrel web sunucusu Internet'e yönelik sunucu daha uygun sağlayan yeni özellikler vardır.</span><span class="sxs-lookup"><span data-stu-id="c2666-152">The Kestrel web server has new features that make it more suitable as an Internet-facing server.</span></span> <span data-ttu-id="c2666-153">Sunucu kısıtlaması yapılandırma seçeneklerinde sayısı ekledik `KestrelServerOptions` sınıfı yeni `Limits` özelliği.</span><span class="sxs-lookup"><span data-stu-id="c2666-153">We’ve added a number of server constraint configuration options in the `KestrelServerOptions` class’s new `Limits` property.</span></span> <span data-ttu-id="c2666-154">Aşağıdakiler için sınırlar artık ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c2666-154">You can now add limits for the following:</span></span>

- <span data-ttu-id="c2666-155">En fazla istemci bağlantıları</span><span class="sxs-lookup"><span data-stu-id="c2666-155">Maximum client connections</span></span>
- <span data-ttu-id="c2666-156">En büyük istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="c2666-156">Maximum request body size</span></span>
- <span data-ttu-id="c2666-157">En düşük istek gövdesi veri oranı</span><span class="sxs-lookup"><span data-stu-id="c2666-157">Minimum request body data rate</span></span>

<span data-ttu-id="c2666-158">Daha fazla bilgi için bkz: [Kestrel web server ASP.NET Core uygulamasında](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="c2666-158">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel).</span></span>

## <a name="weblistener-renamed-to-httpsys"></a><span data-ttu-id="c2666-159">WebListener HTTP.sys'ye yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="c2666-159">WebListener renamed to HTTP.sys</span></span>

<span data-ttu-id="c2666-160">Paketleri `Microsoft.AspNetCore.Server.WebListener` ve `Microsoft.Net.Http.Server` yeni bir pakete birleştirilmiş `Microsoft.AspNetCore.Server.HttpSys`.</span><span class="sxs-lookup"><span data-stu-id="c2666-160">The packages `Microsoft.AspNetCore.Server.WebListener` and `Microsoft.Net.Http.Server` have been merged into a new package `Microsoft.AspNetCore.Server.HttpSys`.</span></span> <span data-ttu-id="c2666-161">Ad alanları eşleşecek şekilde güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="c2666-161">The namespaces have been updated to match.</span></span>

<span data-ttu-id="c2666-162">Daha fazla bilgi için bkz: [HTTP.sys web server ASP.NET Core uygulamasında](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="c2666-162">For more information, see [HTTP.sys web server implementation in ASP.NET Core](xref:fundamentals/servers/httpsys).</span></span>

## <a name="enhanced-http-header-support"></a><span data-ttu-id="c2666-163">Gelişmiş HTTP üstbilgisi desteği</span><span class="sxs-lookup"><span data-stu-id="c2666-163">Enhanced HTTP header support</span></span>

<span data-ttu-id="c2666-164">MVC iletmek için kullanılırken bir `FileStreamResult` veya `FileContentResult`, şimdi ayarlama seçeneğiniz vardır bir `ETag` veya `LastModified` , iletme içerik tarihi.</span><span class="sxs-lookup"><span data-stu-id="c2666-164">When using MVC to transmit a `FileStreamResult` or a `FileContentResult`, you now have the option to set an `ETag` or a `LastModified` date on the content you transmit.</span></span> <span data-ttu-id="c2666-165">Bu değerleri kod ile döndürülen içeriği aşağıdakine benzer ayarlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c2666-165">You can set these values on the returned content with code similar to the following:</span></span>

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

<span data-ttu-id="c2666-166">Ziyaretçileri döndürülen dosya için uygun HTTP üst bilgileri ile donatılmış `ETag` ve `LastModified` değerleri.</span><span class="sxs-lookup"><span data-stu-id="c2666-166">The file returned to your visitors will be decorated with the appropriate HTTP headers for the `ETag` and `LastModified` values.</span></span>

<span data-ttu-id="c2666-167">Bir uygulama ziyaretçi istekleri içeriği, bir aralık istek üstbilgisi ile ASP.NET, tanıyabilir ve bu başlığı tanıtıcı.</span><span class="sxs-lookup"><span data-stu-id="c2666-167">If an application visitor requests content with a Range Request header, ASP.NET will recognize that and handle that header.</span></span> <span data-ttu-id="c2666-168">İstenen içerik kısmen alınabilir, ASP.NET uygun şekilde atlayın ve bayt yalnızca istenen kümesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="c2666-168">If the requested content can be partially delivered, ASP.NET will appropriately skip and return just the requested set of bytes.</span></span>  <span data-ttu-id="c2666-169">Tüm özel işleyicileri uyum veya bu özelliği işlemek için yöntemlerin içine yazma gerekmez; Bu otomatik olarak sizin için gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c2666-169">You do not need to write any special handlers into your methods to adapt or handle this feature; it is automatically handled for you.</span></span>

## <a name="hosting-startup-and-application-insights"></a><span data-ttu-id="c2666-170">Başlatma ve Application Insights barındırma</span><span class="sxs-lookup"><span data-stu-id="c2666-170">Hosting startup and Application Insights</span></span>

<span data-ttu-id="c2666-171">Barındırma ortamları artık ek paket bağımlılıkları ekleme ve kod uygulama başlatma sırasında uygulama açıkça bir bağımlılık alın veya herhangi bir yöntem çağrısı gerek yürütün.</span><span class="sxs-lookup"><span data-stu-id="c2666-171">Hosting environments can now inject extra package dependencies and execute code during application startup, without the application needing to explicitly take a dependency or call any methods.</span></span> <span data-ttu-id="c2666-172">Bu özellik, uygulamanın önceden bilmenize gerek olmadan bu ortam için "ışık li" özelliklere belirli ortamlara etkinleştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c2666-172">This feature can be used to enable certain environments to "light-up" features unique to that environment without the application needing to know ahead of time.</span></span> 

<span data-ttu-id="c2666-173">ASP.NET Core 2. 0'bu özelliği otomatik olarak (sonra seçim) ve Visual Studio'da hata ayıklama sırasında Application Insights Tanılama'yı etkinleştirmek için kullanılan Azure App Services çalıştırırken.</span><span class="sxs-lookup"><span data-stu-id="c2666-173">In ASP.NET Core 2.0, this feature is used to automatically enable Application Insights diagnostics when debugging in Visual Studio and (after opting in) when running in Azure App Services.</span></span> <span data-ttu-id="c2666-174">Sonuç olarak, proje şablonları artık Application Insights paketler ve kod varsayılan olarak ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c2666-174">As a result, the project templates no longer add Application Insights packages and code by default.</span></span>

<span data-ttu-id="c2666-175">Planlanan belgelerine durumu hakkında daha fazla bilgi için bkz: [GitHub sorunu](https://github.com/aspnet/Docs/issues/3389).</span><span class="sxs-lookup"><span data-stu-id="c2666-175">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3389).</span></span>

## <a name="automatic-use-of-anti-forgery-tokens"></a><span data-ttu-id="c2666-176">Sahteciliğe karşı koruma belirteçlerini otomatik olarak kullanılmasını</span><span class="sxs-lookup"><span data-stu-id="c2666-176">Automatic use of anti-forgery tokens</span></span>

<span data-ttu-id="c2666-177">ASP.NET Core her zaman HTML olarak kodlanacak içeriğinizi varsayılan olarak yardımcı, ancak yeni sürümle sizi siteler arası istek sahtekarlığı (XSRF) saldırılarını önlemeye yardımcı olmak için fazladan bir adım yönlendiriyoruz.</span><span class="sxs-lookup"><span data-stu-id="c2666-177">ASP.NET Core has always helped HTML-encode your content by default, but with the new version we’re taking an extra step to help prevent cross-site request forgery (XSRF) attacks.</span></span> <span data-ttu-id="c2666-178">ASP.NET Core artık varsayılan olarak sahteciliğe karşı koruma belirteçleri yayma ve bunları form POST eylemleri ve ek yapılandırma olmaksızın sayfaları doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="c2666-178">ASP.NET Core will now emit anti-forgery tokens by default and validate them on form POST actions and pages without extra configuration.</span></span>

<span data-ttu-id="c2666-179">Daha fazla bilgi için bkz: [önleme siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını ASP.NET Core içinde](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="c2666-179">For more information, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](xref:security/anti-request-forgery).</span></span>

## <a name="automatic-precompilation"></a><span data-ttu-id="c2666-180">Otomatik ön derlemesi</span><span class="sxs-lookup"><span data-stu-id="c2666-180">Automatic precompilation</span></span>

<span data-ttu-id="c2666-181">Razor görünüm ön derleme yayımlama sırasında Yayımla çıkış boyutu ve uygulama azaltma varsayılan olarak etkin başlangıç zamanı.</span><span class="sxs-lookup"><span data-stu-id="c2666-181">Razor view pre-compilation is enabled during publish by default, reducing the publish output size and application startup time.</span></span>

## <a name="razor-support-for-c-71"></a><span data-ttu-id="c2666-182">C# 7.1 Razor desteği</span><span class="sxs-lookup"><span data-stu-id="c2666-182">Razor support for C# 7.1</span></span>

<span data-ttu-id="c2666-183">Razor görüntüleme altyapısı, yeni Roslyn derleyicisi ile çalışmak için güncelleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="c2666-183">The Razor view engine has been updated to work with the new Roslyn compiler.</span></span> <span data-ttu-id="c2666-184">Varsayılan ifadeleri, olayla tanımlama grubu adları ve desen eşleştirme genel türlerle gibi C# 7.1 özellikleri için destek içerir.</span><span class="sxs-lookup"><span data-stu-id="c2666-184">That includes support for C# 7.1 features like Default Expressions, Inferred Tuple Names, and Pattern-Matching with Generics.</span></span> <span data-ttu-id="c2666-185">C# 7.1 projenizde kullanmak için aşağıdaki özellik proje dosyanıza ekleyin ve çözümü yeniden yükle:</span><span class="sxs-lookup"><span data-stu-id="c2666-185">To use C# 7.1 in your project, add the following property in your project file and then reload the solution:</span></span>

```xml
<LangVersion>latest</LangVersion>
```

<span data-ttu-id="c2666-186">C# 7.1 özellikleri durumu hakkında daha fazla bilgi için bkz: [Roslyn GitHub deposuna](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span><span class="sxs-lookup"><span data-stu-id="c2666-186">For information about the status of C# 7.1 features, see [the Roslyn GitHub repository](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span></span>

## <a name="other-documentation-updates-for-20"></a><span data-ttu-id="c2666-187">Diğer belge güncelleştirmeleri 2.0</span><span class="sxs-lookup"><span data-stu-id="c2666-187">Other documentation updates for 2.0</span></span>

* [<span data-ttu-id="c2666-188">Visual Studio yayımlama profilleri ASP.NET Core uygulama dağıtımı için</span><span class="sxs-lookup"><span data-stu-id="c2666-188">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>](xref:host-and-deploy/visual-studio-publish-profiles)
* [<span data-ttu-id="c2666-189">Anahtar Yönetimi</span><span class="sxs-lookup"><span data-stu-id="c2666-189">Key Management</span></span>](xref:security/data-protection/implementation/key-management)
* [<span data-ttu-id="c2666-190">Facebook kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c2666-190">Configuring Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="c2666-191">Twitter kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c2666-191">Configuring Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="c2666-192">Google kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c2666-192">Configuring Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="c2666-193">Microsoft Account kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c2666-193">Configuring Microsoft Account authentication</span></span>](xref:security/authentication/microsoft-logins)

## <a name="migration-guidance"></a><span data-ttu-id="c2666-194">Geçiş Kılavuzu</span><span class="sxs-lookup"><span data-stu-id="c2666-194">Migration guidance</span></span>

<span data-ttu-id="c2666-195">ASP.NET Core 1.x uygulamaları ASP.NET Core 2.0 geçirme hakkında yönergeler için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="c2666-195">For guidance on how to migrate ASP.NET Core 1.x applications to ASP.NET Core 2.0, see the following resources:</span></span>

* [<span data-ttu-id="c2666-196">ASP.NET çekirdek geçirme 1.x ASP.NET Core 2.0 için</span><span class="sxs-lookup"><span data-stu-id="c2666-196">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/index)
* [<span data-ttu-id="c2666-197">Geçirme kimlik doğrulaması ve ASP.NET Core 2.0 için kimliği</span><span class="sxs-lookup"><span data-stu-id="c2666-197">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a><span data-ttu-id="c2666-198">Ek Bilgiler</span><span class="sxs-lookup"><span data-stu-id="c2666-198">Additional Information</span></span>

<span data-ttu-id="c2666-199">Değişiklikleri tam listesi için bkz: [ASP.NET Core 2.0 sürüm notları](https://github.com/aspnet/Home/releases/tag/2.0.0).</span><span class="sxs-lookup"><span data-stu-id="c2666-199">For the complete list of changes, see the [ASP.NET Core 2.0 Release Notes](https://github.com/aspnet/Home/releases/tag/2.0.0).</span></span>

<span data-ttu-id="c2666-200">ASP.NET Core geliştirme takımın ilerleme durumunu ve planları ile bağlanmak istiyorsanız, haftalık olarak ayarlamak [ASP.NET topluluk Standup](https://live.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="c2666-200">If you’d like to connect with the ASP.NET Core development team’s progress and plans, tune in to the weekly [ASP.NET Community Standup](https://live.asp.net/).</span></span>
