---
title: Barındırma ve dağıtım ASP.NET Core Blazor WebAssembly
author: guardrex
description: ASP.NET Core, Içerik teslim ağları (CDN), dosya sunucuları ve GitHub sayfalarını kullanarak bir Blazor uygulamasını nasıl barındırılacağını ve dağıtacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: a0a11f3aed9035000e79844fbec7cdd17b73fdaa
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007343"
---
# <a name="host-and-deploy-aspnet-core-blazor-webassembly"></a><span data-ttu-id="7ab15-103">Barındırma ve dağıtım ASP.NET Core Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="7ab15-103">Host and deploy ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="7ab15-104">, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="7ab15-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="7ab15-105">[Blazor WebAssembly barındırma modeliyle](xref:blazor/hosting-models#blazor-webassembly):</span><span class="sxs-lookup"><span data-stu-id="7ab15-105">With the [Blazor WebAssembly hosting model](xref:blazor/hosting-models#blazor-webassembly):</span></span>

* <span data-ttu-id="7ab15-106">Blazor uygulaması, bağımlılıkları ve .NET çalışma zamanı tarayıcıya indirilir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-106">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="7ab15-107">Uygulama doğrudan tarayıcı kullanıcı arabirimi iş parçacığında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="7ab15-107">The app is executed directly on the browser UI thread.</span></span>

<span data-ttu-id="7ab15-108">Aşağıdaki dağıtım stratejileri desteklenir:</span><span class="sxs-lookup"><span data-stu-id="7ab15-108">The following deployment strategies are supported:</span></span>

* <span data-ttu-id="7ab15-109">Blazor uygulaması, bir ASP.NET Core uygulaması tarafından sunulur.</span><span class="sxs-lookup"><span data-stu-id="7ab15-109">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="7ab15-110">Bu strateji [ASP.NET Core bölümünde barındırılan dağıtımda](#hosted-deployment-with-aspnet-core) ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="7ab15-110">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
* <span data-ttu-id="7ab15-111">Blazor uygulaması, .NET, Blazor uygulamasına hizmet vermek için kullanılmayan bir statik barındırma Web sunucusuna veya hizmetine yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-111">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="7ab15-112">Bu strateji, bir Blazor WebAssembly uygulamasını IIS alt uygulaması olarak barındırma hakkında bilgi içeren [tek başına dağıtım](#standalone-deployment) bölümünde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="7ab15-112">This strategy is covered in the [Standalone deployment](#standalone-deployment) section, which includes information on hosting a Blazor WebAssembly app as an IIS sub-app.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="7ab15-113">Doğru yönlendirme için URL 'Leri yeniden yazın</span><span class="sxs-lookup"><span data-stu-id="7ab15-113">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="7ab15-114">Bir Blazor WebAssembly uygulamasındaki sayfa bileşenlerine yönelik yönlendirme istekleri, bir Blazor sunucusu barındırılan uygulamasındaki yönlendirme istekleri kadar basittir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-114">Routing requests for page components in a Blazor WebAssembly app isn't as straightforward as routing requests in a Blazor Server, hosted app.</span></span> <span data-ttu-id="7ab15-115">İki bileşeni olan bir Blazor WebAssembly uygulaması düşünün:</span><span class="sxs-lookup"><span data-stu-id="7ab15-115">Consider a Blazor WebAssembly app with two components:</span></span>

* <span data-ttu-id="7ab15-116">*Main. razor* &ndash; uygulamanın köküne yüklenir ve `About` bileşenine bir bağlantı içerir (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="7ab15-116">*Main.razor* &ndash; Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="7ab15-117">*. Razor* &ndash; `About` bileşeni hakkında.</span><span class="sxs-lookup"><span data-stu-id="7ab15-117">*About.razor* &ndash; `About` component.</span></span>

<span data-ttu-id="7ab15-118">Uygulamanın varsayılan belgesi, tarayıcının adres çubuğu kullanılarak istendiğinde (örneğin, `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="7ab15-118">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="7ab15-119">Tarayıcı bir istek yapar.</span><span class="sxs-lookup"><span data-stu-id="7ab15-119">The browser makes a request.</span></span>
1. <span data-ttu-id="7ab15-120">Varsayılan sayfa döndürülür, bu genellikle *index. html*'dir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-120">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="7ab15-121">uygulamanın *index. html* önyükleme.</span><span class="sxs-lookup"><span data-stu-id="7ab15-121">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="7ab15-122">Blazor 'in yönlendirici yükleri ve Razor `Main` bileşeni işlenir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-122">Blazor's router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="7ab15-123">Ana sayfada, Blazor yönlendiricisi tarayıcının Internet üzerinde @no__t-@no__t 2 için `www.contoso.com` ' e yönelik bir istek yapmasını durdurduğundan `About` bileşeninin bağlantısını seçtiğinizde istemci üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="7ab15-123">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="7ab15-124">*Blazor WebAssembly uygulaması içindeki* iç uç noktalara yönelik tüm istekler aynı şekilde çalışır: İstekler tarayıcı tabanlı istekleri Internet 'teki sunucu tarafından barındırılan kaynaklara tetiklemez.</span><span class="sxs-lookup"><span data-stu-id="7ab15-124">All of the requests for internal endpoints *within the Blazor WebAssembly app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="7ab15-125">Yönlendirici istekleri dahili olarak işler.</span><span class="sxs-lookup"><span data-stu-id="7ab15-125">The router handles the requests internally.</span></span>

<span data-ttu-id="7ab15-126">@No__t-0 için tarayıcının adres çubuğu kullanılarak bir istek yapılırsa, istek başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="7ab15-126">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="7ab15-127">Uygulamanın Internet ana bilgisayarında böyle bir kaynak yok, bu nedenle *404-bulunamayan* bir yanıt döndürülür.</span><span class="sxs-lookup"><span data-stu-id="7ab15-127">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="7ab15-128">Tarayıcılar, istemci tarafı sayfaları için Internet tabanlı konaklara istek yaptığından, Web sunucuları ve barındırma hizmetleri, fiziksel olarak sunucu üzerinde olmayan kaynakların tüm isteklerini *Dizin. html* sayfasına yeniden yazmalıdır.</span><span class="sxs-lookup"><span data-stu-id="7ab15-128">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="7ab15-129">*İndex. html* döndürüldüğünde, uygulamanın Blazor yönlendiricisi, doğru kaynakla yararlanır ve yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-129">When *index.html* is returned, the app's Blazor router takes over and responds with the correct resource.</span></span>

<span data-ttu-id="7ab15-130">Bir IIS sunucusuna dağıtım yaparken, URL yeniden yazma modülünü uygulamanın yayınlanan *Web. config* dosyasıyla birlikte kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ab15-130">When deploying to an IIS server, you can use the URL Rewrite Module with the app's published *web.config* file.</span></span> <span data-ttu-id="7ab15-131">Daha fazla bilgi için [IIS](#iis) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="7ab15-131">For more information, see the [IIS](#iis) section.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="7ab15-132">ASP.NET Core ile barındırılan dağıtım</span><span class="sxs-lookup"><span data-stu-id="7ab15-132">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="7ab15-133">*Barındırılan bir dağıtım* , Web sunucusu üzerinde çalışan bir [ASP.NET Core](xref:index) uygulamasından tarayıcıları Blazor webassembly uygulamasına sunar.</span><span class="sxs-lookup"><span data-stu-id="7ab15-133">A *hosted deployment* serves the Blazor WebAssembly app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="7ab15-134">Blazor uygulaması, yayımlanan çıktıda ASP.NET Core uygulamasına dahildir, böylece iki uygulama birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="7ab15-134">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="7ab15-135">ASP.NET Core uygulamasını barındırabilen bir Web sunucusu gereklidir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-135">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="7ab15-136">Barındırılan bir dağıtım için, Visual Studio **Blazor WebAssembly uygulama** projesi şablonunu ( [DotNet new](/dotnet/core/tools/dotnet-new) komutu kullanılırken `blazorwasm` şablonu) **barındırılan** seçeneği seçili olarak içerir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-136">For a hosted deployment, Visual Studio includes the **Blazor WebAssembly App** project template (`blazorwasm` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) with the **Hosted** option selected.</span></span>

<span data-ttu-id="7ab15-137">Uygulama barındırma ve dağıtım ASP.NET Core hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="7ab15-137">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="7ab15-138">Azure App Service dağıtma hakkında daha fazla bilgi için bkz. <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="7ab15-138">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="7ab15-139">Tek başına dağıtım</span><span class="sxs-lookup"><span data-stu-id="7ab15-139">Standalone deployment</span></span>

<span data-ttu-id="7ab15-140">*Tek başına dağıtım* , doğrudan istemciler tarafından istenen statik dosyalar kümesi olarak Blazor WebAssembly uygulamasına hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-140">A *standalone deployment* serves the Blazor WebAssembly app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="7ab15-141">Herhangi bir statik dosya sunucusu Blazor uygulamasını sunabilir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-141">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="7ab15-142">Tek başına dağıtım varlıkları *bin/Release/{Target Framework}/publish/{ASSEMBLY Name}/Dist* klasöründe yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="7ab15-142">Standalone deployment assets are published to the *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span>

### <a name="iis"></a><span data-ttu-id="7ab15-143">IIS</span><span class="sxs-lookup"><span data-stu-id="7ab15-143">IIS</span></span>

<span data-ttu-id="7ab15-144">IIS, Blazor uygulamaları için özellikli bir statik dosya sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="7ab15-144">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="7ab15-145">IIS 'yi Blazor barındıracak şekilde yapılandırmak için bkz. [IIS 'de statik Web sitesi oluşturma](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="7ab15-145">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="7ab15-146">Yayımlanan varlıklar */BIN/Release/{Target Framework}/Publish* klasöründe oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7ab15-146">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="7ab15-147">Web sunucusunda veya barındırma hizmetinde *Yayımlama* klasörünün içeriğini barındırın.</span><span class="sxs-lookup"><span data-stu-id="7ab15-147">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="7ab15-148">Web. config</span><span class="sxs-lookup"><span data-stu-id="7ab15-148">web.config</span></span>

<span data-ttu-id="7ab15-149">Bir Blazor projesi yayımlandığında, aşağıdaki IIS yapılandırmasıyla bir *Web. config* dosyası oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="7ab15-149">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="7ab15-150">MIME türleri aşağıdaki dosya uzantıları için ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="7ab15-150">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="7ab15-151">*. dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="7ab15-151">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="7ab15-152">*. json* &ndash; `application/json`</span><span class="sxs-lookup"><span data-stu-id="7ab15-152">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="7ab15-153">*.* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="7ab15-153">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="7ab15-154">*. WOFF* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="7ab15-154">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="7ab15-155">*. woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="7ab15-155">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="7ab15-156">Aşağıdaki MIME türleri için HTTP sıkıştırması etkindir:</span><span class="sxs-lookup"><span data-stu-id="7ab15-156">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="7ab15-157">URL yeniden yazma modülü kuralları oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="7ab15-157">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="7ab15-158">Uygulamanın statik varlıklarının bulunduğu alt dizini ( *{Assembly Name}/dist/{PATH istenen}* ) sunar.</span><span class="sxs-lookup"><span data-stu-id="7ab15-158">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="7ab15-159">Dosya olmayan varlıklar için isteklerin statik varlıklar klasöründe ( *{Assembly Name}/Dist/index.html*) uygulamanın varsayılan belgesine yeniden YÖNLENDIRILMESI için Spa geri dönüş yönlendirmesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7ab15-159">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="7ab15-160">URL yeniden yazma modülünü yükler</span><span class="sxs-lookup"><span data-stu-id="7ab15-160">Install the URL Rewrite Module</span></span>

<span data-ttu-id="7ab15-161">[URL yeniden yazma modülü](https://www.iis.net/downloads/microsoft/url-rewrite) , URL 'leri yeniden yazmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-161">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="7ab15-162">Modül varsayılan olarak yüklenmez ve bir Web sunucusu (IIS) rol hizmeti özelliği olarak yükleme için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="7ab15-162">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="7ab15-163">Modül IIS Web sitesinden indirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-163">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="7ab15-164">Modülünü yüklemek için Web Platformu Yükleyicisi 'ni kullanın:</span><span class="sxs-lookup"><span data-stu-id="7ab15-164">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="7ab15-165">Yerel olarak, [URL yeniden yazma modülü İndirmeleri sayfasına](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)gidin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-165">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="7ab15-166">Ingilizce sürümünde, WebPI yükleyicisini indirmek için **WebPI** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-166">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="7ab15-167">Diğer diller için, yükleyiciyi indirmek üzere sunucu için uygun mimariyi (x86/x64) seçin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-167">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="7ab15-168">Yükleyiciyi sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="7ab15-168">Copy the installer to the server.</span></span> <span data-ttu-id="7ab15-169">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7ab15-169">Run the installer.</span></span> <span data-ttu-id="7ab15-170">, **Install** düğmesini seçin ve lisans koşullarını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-170">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="7ab15-171">Yüklemesi tamamlandıktan sonra sunucu yeniden başlatması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-171">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="7ab15-172">Web sitesini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7ab15-172">Configure the website</span></span>

<span data-ttu-id="7ab15-173">Web sitesinin **fiziksel yolunu** uygulamanın klasörüne ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7ab15-173">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="7ab15-174">Klasör şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="7ab15-174">The folder contains:</span></span>

* <span data-ttu-id="7ab15-175">Gerekli yeniden yönlendirme kuralları ve dosya içerik türleri dahil olmak üzere IIS 'nin Web sitesini yapılandırmak için kullandığı *Web. config* dosyası.</span><span class="sxs-lookup"><span data-stu-id="7ab15-175">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="7ab15-176">Uygulamanın statik varlık klasörü.</span><span class="sxs-lookup"><span data-stu-id="7ab15-176">The app's static asset folder.</span></span>

#### <a name="host-as-an-iis-sub-app"></a><span data-ttu-id="7ab15-177">IIS alt uygulaması olarak barındırma</span><span class="sxs-lookup"><span data-stu-id="7ab15-177">Host as an IIS sub-app</span></span>

<span data-ttu-id="7ab15-178">Tek başına bir uygulama bir IIS alt uygulaması olarak barındırılıyorsa, aşağıdakilerden birini yapın:</span><span class="sxs-lookup"><span data-stu-id="7ab15-178">If a standalone app is hosted as an IIS sub-app, perform either of the following:</span></span>

* <span data-ttu-id="7ab15-179">Devralınan ASP.NET Core modülü işleyicisini devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="7ab15-179">Disable the inherited ASP.NET Core Module handler.</span></span>

  <span data-ttu-id="7ab15-180">Dosyaya `<handlers>` bölümü ekleyerek Blazor uygulamasının yayınlanan *Web. config* dosyasındaki işleyiciyi kaldırın:</span><span class="sxs-lookup"><span data-stu-id="7ab15-180">Remove the handler in the Blazor app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* <span data-ttu-id="7ab15-181">@No__t-2 @no__t 3 olarak ayarlanan `<location>` öğesi kullanarak kök (üst) uygulamanın `<system.webServer>` bölümünü devralmayı devre dışı bırakın:</span><span class="sxs-lookup"><span data-stu-id="7ab15-181">Disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <handlers>
          <add name="aspNetCore" ... />
        </handlers>
        <aspNetCore ... />
      </system.webServer>
    </location>
  </configuration>
  ```

<span data-ttu-id="7ab15-182">İşleyicinin kaldırılması veya devralma devre dışı bırakılması, [uygulamanın temel yolunun yapılandırılmasına](xref:host-and-deploy/blazor/index#app-base-path)ek olarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-182">Removing the handler or disabling inheritance is performed in addition to [configuring the app's base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span> <span data-ttu-id="7ab15-183">Uygulamanın *index. html* dosyasındaki uygulama temel yolunu, IIS 'de alt uygulamayı YAPıLANDıRıRKEN kullanılan IIS diğer adına ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7ab15-183">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="7ab15-184">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="7ab15-184">Troubleshooting</span></span>

<span data-ttu-id="7ab15-185">*500-Iç sunucu hatası* ALıNMıŞSA ve IIS Yöneticisi Web sitesinin yapılandırmasına erişmeye çalışırken hatalar OLUŞTURURSA, URL yeniden yazma modülünün yüklü olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="7ab15-185">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="7ab15-186">Modül yüklü olmadığında, *Web. config* dosyası IIS tarafından ayrıştırılamaz.</span><span class="sxs-lookup"><span data-stu-id="7ab15-186">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="7ab15-187">Bu, IIS yöneticisinin Web sitesinin yapılandırmasını ve Web sitesinin Blazor 'in statik dosyalarına hizmet etmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="7ab15-187">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="7ab15-188">IIS ile dağıtım sorunlarını giderme hakkında daha fazla bilgi için bkz. <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="7ab15-188">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="7ab15-189">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7ab15-189">Azure Storage</span></span>

<span data-ttu-id="7ab15-190">[Azure depolama](/azure/storage/) statik dosya barındırma, sunucusuz Blazor uygulamasının barındırılmasına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ab15-190">[Azure Storage](/azure/storage/) static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="7ab15-191">Özel etki alanı adları, Azure Content Delivery Network (CDN) ve HTTPS desteklenir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-191">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="7ab15-192">Blob hizmeti bir depolama hesabında barındırılan statik Web sitesi için etkinleştirildiğinde:</span><span class="sxs-lookup"><span data-stu-id="7ab15-192">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="7ab15-193">**Dizin belgesi adını** `index.html` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7ab15-193">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="7ab15-194">**Hata belge yolunu** `index.html` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7ab15-194">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="7ab15-195">Razor bileşenleri ve diğer dosya olmayan uç noktaları, blob hizmeti tarafından depolanan statik içerikte fiziksel yollarda yer vermez.</span><span class="sxs-lookup"><span data-stu-id="7ab15-195">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="7ab15-196">Blazor yönlendiricisinin işlemesi gereken bu kaynaklardan birine yönelik bir istek alındığında, blob hizmeti tarafından oluşturulan *404-bulunamayan* hata, isteği **hata belge yoluna**yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-196">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="7ab15-197">*İndex. html* blobu döndürülür ve Blazor yönlendiricisi yolu yükler ve işler.</span><span class="sxs-lookup"><span data-stu-id="7ab15-197">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="7ab15-198">Daha fazla bilgi için bkz. [Azure Storage 'Da statik Web sitesi barındırma](/azure/storage/blobs/storage-blob-static-website).</span><span class="sxs-lookup"><span data-stu-id="7ab15-198">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="7ab15-199">NGINX</span><span class="sxs-lookup"><span data-stu-id="7ab15-199">Nginx</span></span>

<span data-ttu-id="7ab15-200">Aşağıdaki *NGINX. conf* dosyası, NGINX 'in, disk üzerinde karşılık gelen bir dosyayı bulamadığı her seferinde *index. html* dosyasını göndermek üzere nasıl yapılandırılacağını gösterecek şekilde basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-200">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

<span data-ttu-id="7ab15-201">Üretim NGINX web sunucusu yapılandırması hakkında daha fazla bilgi için bkz. [NGINX Plus ve NGINX yapılandırma dosyaları oluşturma](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span><span class="sxs-lookup"><span data-stu-id="7ab15-201">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="7ab15-202">Docker 'da NGINX</span><span class="sxs-lookup"><span data-stu-id="7ab15-202">Nginx in Docker</span></span>

<span data-ttu-id="7ab15-203">NGINX kullanarak Docker 'da Blazor barındırmak için Dockerfile 'ı alp tabanlı NGINX görüntüsünü kullanacak şekilde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7ab15-203">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="7ab15-204">Dockerfile dosyasını, *NGINX. config* dosyasını kapsayıcıya kopyalamak için güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-204">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="7ab15-205">Aşağıdaki örnekte gösterildiği gibi Dockerfile dosyasına bir satır ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7ab15-205">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a><span data-ttu-id="7ab15-206">GitHub sayfaları</span><span class="sxs-lookup"><span data-stu-id="7ab15-206">GitHub Pages</span></span>

<span data-ttu-id="7ab15-207">URL yeniden işlemesini işlemek için, isteği *index. html* sayfasına yönlendirmeyi işleyen bir betiği olan bir *404. html* dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-207">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="7ab15-208">Topluluk tarafından sunulan örnek bir uygulama için bkz. [GitHub sayfaları Için tek sayfalı uygulamalar](https://spa-github-pages.rafrex.com/) ([GitHub üzerinde rafrex/Spa-GitHub-Pages](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="7ab15-208">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="7ab15-209">Topluluk yaklaşımını kullanan bir örnek, GitHub ([canlı site](https://blazor-demo.github.io/)) [üzerinde blazor-demo/blazor-demo. GitHub. IO](https://github.com/blazor-demo/blazor-demo.github.io) adresinde görülebilir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-209">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="7ab15-210">Bir kuruluş sitesi yerine bir proje sitesi kullanırken, *index. html*dosyasına `<base>` etiketi ekleyin veya güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-210">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="7ab15-211">@No__t-0 öznitelik değerini GitHub deposu adına sondaki eğik çizgiyle (örneğin, `my-repository/`) ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7ab15-211">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="7ab15-212">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="7ab15-212">Host configuration values</span></span>

<span data-ttu-id="7ab15-213">[Blazor WebAssembly Apps](xref:blazor/hosting-models#blazor-webassembly) , geliştirme ortamındaki çalışma zamanında aşağıdaki ana bilgisayar yapılandırma değerlerini komut satırı bağımsız değişkenleri olarak kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-213">[Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="7ab15-214">İçerik kökü</span><span class="sxs-lookup"><span data-stu-id="7ab15-214">Content root</span></span>

<span data-ttu-id="7ab15-215">@No__t-0 bağımsız değişkeni, uygulamanın içerik dosyalarını ([içerik kökü](xref:fundamentals/index#content-root)) içeren dizinin mutlak yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="7ab15-215">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files ([content root](xref:fundamentals/index#content-root)).</span></span> <span data-ttu-id="7ab15-216">Aşağıdaki örneklerde, `/content-root-path`, uygulamanın içerik kök yoludur.</span><span class="sxs-lookup"><span data-stu-id="7ab15-216">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="7ab15-217">Uygulamayı bir komut isteminde yerel olarak çalıştırırken bağımsız değişkenini geçirin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-217">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="7ab15-218">Uygulamanın dizininden şunu yürütün:</span><span class="sxs-lookup"><span data-stu-id="7ab15-218">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="7ab15-219">**IIS Express** profilindeki uygulamanın *launchsettings. JSON* dosyasına bir giriş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-219">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="7ab15-220">Bu ayar, uygulama Visual Studio hata ayıklayıcıyla ve `dotnet run` ile bir komut isteminden çalıştırıldığında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7ab15-220">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="7ab15-221">Visual Studio 'da  > **hata ayıklama** > **uygulama bağımsız değişkenlerinin** **Özellikler**bölümünde bağımsız değişkenini belirtin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-221">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="7ab15-222">Visual Studio özellik sayfasında bağımsız değişkeni ayarlama, bağımsız değişkenini *Launchsettings. JSON* dosyasına ekler.</span><span class="sxs-lookup"><span data-stu-id="7ab15-222">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="7ab15-223">Yol tabanı</span><span class="sxs-lookup"><span data-stu-id="7ab15-223">Path base</span></span>

<span data-ttu-id="7ab15-224">@No__t-0 bağımsız değişkeni, kök olmayan bir göreli URL yoluyla yerel olarak çalışan bir uygulamanın uygulama temel yolunu ayarlar (`<base>` etiketi `href` ' nin hazırlama ve üretim için @no__t 3 ' ten farklı bir yol olarak ayarlanır).</span><span class="sxs-lookup"><span data-stu-id="7ab15-224">The `--pathbase` argument sets the app base path for an app run locally with a non-root relative URL path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="7ab15-225">Aşağıdaki örneklerde, `/relative-URL-path`, uygulamanın yol tabanı olur.</span><span class="sxs-lookup"><span data-stu-id="7ab15-225">In the following examples, `/relative-URL-path` is the app's path base.</span></span> <span data-ttu-id="7ab15-226">Daha fazla bilgi için bkz. [uygulama temel yolu](xref:host-and-deploy/blazor/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="7ab15-226">For more information, see [App base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ab15-227">@No__t-1 etiketinin `href` ' a girilen yolun aksine, @no__t 3 bağımsız değişken değeri geçirilirken sondaki eğik çizgi (`/`) eklemeyin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-227">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="7ab15-228">Uygulama temel yolu `<base>` etiketinde `<base href="/CoolApp/">` (sondaki eğik çizgi içeriyorsa) olarak sağlanmışsa, komut satırı bağımsız değişken değerini `--pathbase=/CoolApp` (sondaki eğik çizgi olmadan) olarak geçirin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-228">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="7ab15-229">Uygulamayı bir komut isteminde yerel olarak çalıştırırken bağımsız değişkenini geçirin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-229">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="7ab15-230">Uygulamanın dizininden şunu yürütün:</span><span class="sxs-lookup"><span data-stu-id="7ab15-230">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* <span data-ttu-id="7ab15-231">**IIS Express** profilindeki uygulamanın *launchsettings. JSON* dosyasına bir giriş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-231">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="7ab15-232">Bu ayar, uygulamayı Visual Studio hata ayıklayıcıyla ve `dotnet run` ile bir komut isteminden çalıştırırken kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7ab15-232">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* <span data-ttu-id="7ab15-233">Visual Studio 'da  > **hata ayıklama** > **uygulama bağımsız değişkenlerinin** **Özellikler**bölümünde bağımsız değişkenini belirtin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-233">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="7ab15-234">Visual Studio özellik sayfasında bağımsız değişkeni ayarlama, bağımsız değişkenini *Launchsettings. JSON* dosyasına ekler.</span><span class="sxs-lookup"><span data-stu-id="7ab15-234">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a><span data-ttu-id="7ab15-235">URL'ler</span><span class="sxs-lookup"><span data-stu-id="7ab15-235">URLs</span></span>

<span data-ttu-id="7ab15-236">@No__t-0 bağımsız değişkeni, istekler için dinlemek üzere bağlantı noktaları ve protokollerle IP adreslerini veya konak adreslerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="7ab15-236">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="7ab15-237">Uygulamayı bir komut isteminde yerel olarak çalıştırırken bağımsız değişkenini geçirin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-237">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="7ab15-238">Uygulamanın dizininden şunu yürütün:</span><span class="sxs-lookup"><span data-stu-id="7ab15-238">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="7ab15-239">**IIS Express** profilindeki uygulamanın *launchsettings. JSON* dosyasına bir giriş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-239">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="7ab15-240">Bu ayar, uygulamayı Visual Studio hata ayıklayıcıyla ve `dotnet run` ile bir komut isteminden çalıştırırken kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7ab15-240">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="7ab15-241">Visual Studio 'da  > **hata ayıklama** > **uygulama bağımsız değişkenlerinin** **Özellikler**bölümünde bağımsız değişkenini belirtin.</span><span class="sxs-lookup"><span data-stu-id="7ab15-241">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="7ab15-242">Visual Studio özellik sayfasında bağımsız değişkeni ayarlama, bağımsız değişkenini *Launchsettings. JSON* dosyasına ekler.</span><span class="sxs-lookup"><span data-stu-id="7ab15-242">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a><span data-ttu-id="7ab15-243">Bağlayıcı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7ab15-243">Configure the Linker</span></span>

<span data-ttu-id="7ab15-244">Blazor, çıkış derlemelerinden gereksiz Il 'yi kaldırmak için her bir derlemede ara dil (IL) bağlamayı gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-244">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="7ab15-245">Derleme bağlama, derleme üzerinde denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="7ab15-245">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="7ab15-246">Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="7ab15-246">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>
