---
title: Konak ve dağıtım ASP.NET Core Blazor istemci tarafı
author: guardrex
description: ASP.NET Core, Içerik teslim ağları (CDN), dosya sunucuları ve GitHub sayfalarını kullanarak bir Blazor uygulamasını nasıl barındırılacağını ve dağıtacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: host-and-deploy/blazor/client-side
ms.openlocfilehash: be6b6c245440cb085a1a6b115f4f087306f7cc83
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308082"
---
# <a name="host-and-deploy-aspnet-core-blazor-client-side"></a><span data-ttu-id="2c5dd-103">Konak ve dağıtım ASP.NET Core Blazor istemci tarafı</span><span class="sxs-lookup"><span data-stu-id="2c5dd-103">Host and deploy ASP.NET Core Blazor client-side</span></span>

<span data-ttu-id="2c5dd-104">, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="2c5dd-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="2c5dd-105">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="2c5dd-105">Host configuration values</span></span>

<span data-ttu-id="2c5dd-106">[İstemci tarafı barındırma modelini](xref:blazor/hosting-models#client-side) kullanan Blazor uygulamalar, geliştirme ortamındaki çalışma zamanında aşağıdaki ana bilgisayar yapılandırma değerlerini komut satırı bağımsız değişkenleri olarak kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-106">Blazor apps that use the [client-side hosting model](xref:blazor/hosting-models#client-side) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="2c5dd-107">İçerik kökü</span><span class="sxs-lookup"><span data-stu-id="2c5dd-107">Content Root</span></span>

<span data-ttu-id="2c5dd-108">`--contentroot` Bağımsız değişkeni, uygulamanın içerik dosyalarını içeren dizinin mutlak yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-108">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span> <span data-ttu-id="2c5dd-109">Aşağıdaki örneklerde, `/content-root-path` uygulamanın içerik kök yolu bulunur.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-109">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="2c5dd-110">Uygulamayı bir komut isteminde yerel olarak çalıştırırken bağımsız değişkenini geçirin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-110">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="2c5dd-111">Uygulamanın dizininden şunu yürütün:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-111">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="2c5dd-112">**IIS Express** profilindeki uygulamanın *launchsettings. JSON* dosyasına bir giriş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-112">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="2c5dd-113">Bu ayar, uygulama Visual Studio hata ayıklayıcısı ve ile `dotnet run`bir komut isteminden çalıştırıldığında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-113">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="2c5dd-114">Visual Studio 'da, **Özellikler** > **hata ayıklama** > **uygulama bağımsız değişkenlerinde**bağımsız değişkenini belirtin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-114">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="2c5dd-115">Visual Studio özellik sayfasında bağımsız değişkeni ayarlama, bağımsız değişkenini *Launchsettings. JSON* dosyasına ekler.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-115">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="2c5dd-116">Yol tabanı</span><span class="sxs-lookup"><span data-stu-id="2c5dd-116">Path base</span></span>

<span data-ttu-id="2c5dd-117">Bağımsız değişkeni, kök olmayan bir sanal yol ile yerel olarak çalıştırılan bir uygulamanın uygulama temeli yolunu ayarlar `<base>` (etiket `href` , hazırlama ve üretim `/` için dışında bir yola ayarlanır). `--pathbase`</span><span class="sxs-lookup"><span data-stu-id="2c5dd-117">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="2c5dd-118">Aşağıdaki örneklerde, `/virtual-path` uygulamanın yol tabanı bulunur.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-118">In the following examples, `/virtual-path` is the app's path base.</span></span> <span data-ttu-id="2c5dd-119">Daha fazla bilgi için [uygulama temel yolu](#app-base-path) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-119">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2c5dd-120">Etiketinde belirtilen `href` `/`yolun aksine, `--pathbase` bağımsız değişken değeri geçirilirken sondaki eğik çizgi () eklemeyin. `<base>`</span><span class="sxs-lookup"><span data-stu-id="2c5dd-120">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="2c5dd-121">`<base>` Etikette uygulama temel yolu (sondaki eğik çizgi içeriyorsa) olarak `<base href="/CoolApp/">` sağlanmışsa, komut satırı bağımsız değişken değerini (sondaki eğik çizgi yok) olarak `--pathbase=/CoolApp` geçirin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-121">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="2c5dd-122">Uygulamayı bir komut isteminde yerel olarak çalıştırırken bağımsız değişkenini geçirin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-122">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="2c5dd-123">Uygulamanın dizininden şunu yürütün:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-123">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/virtual-path
  ```

* <span data-ttu-id="2c5dd-124">**IIS Express** profilindeki uygulamanın *launchsettings. JSON* dosyasına bir giriş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-124">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="2c5dd-125">Bu ayar, uygulamayı Visual Studio hata ayıklayıcıyla ve ile `dotnet run`bir komut isteminden çalıştırırken kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-125">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/virtual-path"
  ```

* <span data-ttu-id="2c5dd-126">Visual Studio 'da, **Özellikler** > **hata ayıklama** > **uygulama bağımsız değişkenlerinde**bağımsız değişkenini belirtin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-126">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="2c5dd-127">Visual Studio özellik sayfasında bağımsız değişkeni ayarlama, bağımsız değişkenini *Launchsettings. JSON* dosyasına ekler.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-127">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/virtual-path
  ```

### <a name="urls"></a><span data-ttu-id="2c5dd-128">URL'ler</span><span class="sxs-lookup"><span data-stu-id="2c5dd-128">URLs</span></span>

<span data-ttu-id="2c5dd-129">`--urls` Bağımsız değişkeni, istekler için dinlemek üzere bağlantı noktaları ve protokollerle IP adreslerini veya konak adreslerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-129">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="2c5dd-130">Uygulamayı bir komut isteminde yerel olarak çalıştırırken bağımsız değişkenini geçirin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-130">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="2c5dd-131">Uygulamanın dizininden şunu yürütün:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-131">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="2c5dd-132">**IIS Express** profilindeki uygulamanın *launchsettings. JSON* dosyasına bir giriş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-132">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="2c5dd-133">Bu ayar, uygulamayı Visual Studio hata ayıklayıcıyla ve ile `dotnet run`bir komut isteminden çalıştırırken kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-133">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="2c5dd-134">Visual Studio 'da, **Özellikler** > **hata ayıklama** > **uygulama bağımsız değişkenlerinde**bağımsız değişkenini belirtin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-134">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="2c5dd-135">Visual Studio özellik sayfasında bağımsız değişkeni ayarlama, bağımsız değişkenini *Launchsettings. JSON* dosyasına ekler.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-135">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deployment"></a><span data-ttu-id="2c5dd-136">Dağıtım</span><span class="sxs-lookup"><span data-stu-id="2c5dd-136">Deployment</span></span>

<span data-ttu-id="2c5dd-137">[İstemci tarafı barındırma modeliyle](xref:blazor/hosting-models#client-side):</span><span class="sxs-lookup"><span data-stu-id="2c5dd-137">With the [client-side hosting model](xref:blazor/hosting-models#client-side):</span></span>

* <span data-ttu-id="2c5dd-138">Blazor uygulaması, bağımlılıkları ve .NET çalışma zamanı tarayıcıya indirilir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-138">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="2c5dd-139">Uygulama doğrudan tarayıcı kullanıcı arabirimi iş parçacığında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-139">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="2c5dd-140">Aşağıdaki stratejilerden biri desteklenir:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-140">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="2c5dd-141">Blazor uygulaması, bir ASP.NET Core uygulaması tarafından sunulur.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-141">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="2c5dd-142">Bu strateji [ASP.NET Core bölümünde barındırılan dağıtımda](#hosted-deployment-with-aspnet-core) ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-142">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="2c5dd-143">Blazor uygulaması, .NET, Blazor uygulamasına hizmet vermek için kullanılmayan bir statik barındırma Web sunucusuna veya hizmetine yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-143">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="2c5dd-144">Bu strateji, [tek başına dağıtım](#standalone-deployment) bölümünde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-144">This strategy is covered in the [Standalone deployment](#standalone-deployment) section.</span></span>

## <a name="configure-the-linker"></a><span data-ttu-id="2c5dd-145">Bağlayıcıyı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2c5dd-145">Configure the Linker</span></span>

<span data-ttu-id="2c5dd-146">Blazor, çıkış derlemelerinden gereksiz Il 'yi kaldırmak için her bir derlemede ara dil (IL) bağlamayı gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-146">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="2c5dd-147">Derleme bağlama, derleme üzerinde denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-147">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="2c5dd-148">Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-148">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="2c5dd-149">Doğru yönlendirme için URL 'Leri yeniden yazın</span><span class="sxs-lookup"><span data-stu-id="2c5dd-149">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="2c5dd-150">İstemci tarafı uygulamadaki sayfa bileşenlerine yönelik yönlendirme istekleri, sunucu tarafı barındırılan bir uygulamaya yönlendirme istekleri kadar basit değildir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-150">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="2c5dd-151">İki bileşeni olan bir istemci tarafı uygulamayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-151">Consider a client-side app with two components:</span></span>

* <span data-ttu-id="2c5dd-152">*Main. Razor* &ndash; , uygulamanın köküne yüklenir ve `About` bileşene (`href="About"`) bir bağlantı içerir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-152">*Main.razor* &ndash; Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="2c5dd-153">*.* &ndash; Razor`About` bileşeni hakkında.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-153">*About.razor* &ndash; `About` component.</span></span>

<span data-ttu-id="2c5dd-154">Uygulamanın varsayılan belgesi, tarayıcının adres çubuğu kullanılarak istendiğinde (örneğin, `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="2c5dd-154">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="2c5dd-155">Tarayıcı bir istek yapar.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-155">The browser makes a request.</span></span>
1. <span data-ttu-id="2c5dd-156">Varsayılan sayfa döndürülür, bu genellikle *index. html*'dir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-156">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="2c5dd-157">uygulamanın *index. html* önyükleme.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-157">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="2c5dd-158">Blazor 'in yönlendirici yükleri ve Razor `Main` bileşeni işlenir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-158">Blazor's router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="2c5dd-159">Ana sayfada `About` , Blazor yönlendiricisi tarayıcıyı `www.contoso.com` Internet `About` 'te için bir istek yapmadan ve işlenmiş `About` bileşenin kendisini görecek olduğundan, bileşen bağlantısını seçtiğinizde istemci üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-159">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="2c5dd-160">*İstemci tarafı uygulama içindeki* iç uç noktalara yönelik tüm istekler aynı şekilde çalışır: İstekler tarayıcı tabanlı istekleri Internet 'teki sunucu tarafından barındırılan kaynaklara tetiklemez.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-160">All of the requests for internal endpoints *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="2c5dd-161">Yönlendirici istekleri dahili olarak işler.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-161">The router handles the requests internally.</span></span>

<span data-ttu-id="2c5dd-162">Tarayıcının adres çubuğu `www.contoso.com/About`kullanılarak bir istek yapılırsa, istek başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-162">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="2c5dd-163">Uygulamanın Internet ana bilgisayarında böyle bir kaynak yok, bu nedenle *404-bulunamayan* bir yanıt döndürülür.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-163">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="2c5dd-164">Tarayıcılar, istemci tarafı sayfaları için Internet tabanlı konaklara istek yaptığından, Web sunucuları ve barındırma hizmetleri, fiziksel olarak sunucu üzerinde olmayan kaynakların tüm isteklerini *Dizin. html* sayfasına yeniden yazmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-164">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="2c5dd-165">*İndex. html* döndürüldüğünde, uygulamanın istemci tarafı yönlendiricisi, doğru kaynakla sürer ve yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-165">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="2c5dd-166">Uygulama temel yolu</span><span class="sxs-lookup"><span data-stu-id="2c5dd-166">App base path</span></span>

<span data-ttu-id="2c5dd-167">*Uygulama temel yolu* , sunucusundaki sanal uygulama kök yoludur.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-167">The *app base path* is the virtual app root path on the server.</span></span> <span data-ttu-id="2c5dd-168">Örneğin, ' de bir sanal klasördeki `/CoolApp/` `https://www.contoso.com/CoolApp` contoso sunucusunda bulunan bir uygulamaya, ve sanal temel yolu `/CoolApp/`vardır.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-168">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="2c5dd-169">Uygulama taban yolunu sanal yola (`<base href="/CoolApp/">`) ayarlayarak, uygulama sunucuda neredeyse bulunduğu yerden haberdar olur.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-169">By setting the app base path to the virtual path (`<base href="/CoolApp/">`), the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="2c5dd-170">Uygulama, kök dizinde olmayan bir bileşenden uygulama köküne göre URL 'Ler oluşturmak için uygulama temel yolunu kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-170">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="2c5dd-171">Bu, farklı düzeylerde dizin yapısında bulunan bileşenlerin, uygulama genelindeki konumlarda diğer kaynakların bağlantılarını oluşturmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-171">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="2c5dd-172">Uygulama temel yolu Ayrıca, bağlantının `href` hedefinin uygulama temel yolu URI alanı&mdash;içinde olduğu yerde köprü tıklamalarını, Blazor yönlendiricisinin iç gezintiyi işlemesini sağlamak için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-172">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="2c5dd-173">Birçok barındırma senaryosunda, sunucunun uygulamanın sanal yolu uygulamanın köküdür.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-173">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="2c5dd-174">Bu durumlarda, uygulama temel yolu, bir uygulamanın varsayılan yapılandırması olan bir`<base href="/" />`eğik çizgi () olur.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-174">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="2c5dd-175">GitHub sayfaları ve IIS sanal dizinleri ya da alt uygulamalar gibi diğer barındırma senaryolarında, uygulama temel yolu sunucunun uygulamanın sanal yoluna ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-175">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="2c5dd-176">Uygulamanın temel yolunu ayarlamak için `<base>` *Wwwroot/index.html* dosyasının `<head>` etiket öğeleri içindeki etiketi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-176">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *wwwroot/index.html* file.</span></span> <span data-ttu-id="2c5dd-177">Öznitelik değerini olarak `/virtual-path/` ayarlayın (sondaki eğik çizgi gereklidir); burada `/virtual-path/` , uygulamanın sunucusundaki tam sanal uygulama kök yoludur. `href`</span><span class="sxs-lookup"><span data-stu-id="2c5dd-177">Set the `href` attribute value to `/virtual-path/` (the trailing slash is required), where `/virtual-path/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="2c5dd-178">Yukarıdaki örnekte, sanal yol: `/CoolApp/` `<base href="/CoolApp/">`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-178">In the preceding example, the virtual path is set to `/CoolApp/`: `<base href="/CoolApp/">`.</span></span>

<span data-ttu-id="2c5dd-179">Kök olmayan bir sanal yol (örneğin, `<base href="/CoolApp/">`) yapılandırılmış bir uygulama için, uygulama *yerel olarak çalıştırıldığında*kaynaklarını bulamaz.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-179">For an app with a non-root virtual path configured (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="2c5dd-180">Yerel geliştirme ve test sırasında bu sorunu aşmak için, çalışma zamanında `<base>` etiketinin `href` değeriyle eşleşen bir *yol temel* bağımsız değişkeni sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-180">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="2c5dd-181">Uygulamayı yerel olarak çalıştırırken yol temel bağımsız değişkenini kök yol (`/`) ile geçirmek için, `dotnet run` komutunu `--pathbase` uygulama dizininden çalıştırın, seçeneği:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-181">To pass the path base argument with the root path (`/`) when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```console
dotnet run --pathbase=/{Virtual Path (no trailing slash)}
```

<span data-ttu-id="2c5dd-182">`/CoolApp/` (`<base href="/CoolApp/">`) Sanal taban yoluna sahip bir uygulama için, komut şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-182">For an app with a virtual base path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="2c5dd-183">Uygulama üzerinde `http://localhost:port/CoolApp`yerel olarak yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-183">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="2c5dd-184">Daha fazla bilgi için, [yol temel ana bilgisayar yapılandırma değerindeki](#path-base)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-184">For more information, see the section on the [path base host configuration value](#path-base).</span></span>

<span data-ttu-id="2c5dd-185">Bir uygulama, [istemci tarafı barındırma modelini](xref:blazor/hosting-models#client-side) ( **Blazor (istemci tarafı)** proje `blazor` şablonunu temel alan, [DotNet yeni](/dotnet/core/tools/dotnet-new) komut kullanılırken şablon) kullanıyorsa ve bir ASP.NET Core uygulamasında IIS alt uygulaması olarak barındırılıyorsa, devralınan ASP.NET Core modülü işleyicisini devre dışı bırakın veya *Web. config* dosyasındaki kök (üst) `<handlers>` uygulamanın bölümünün alt uygulama tarafından devralınamayacağını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-185">If an app uses the [client-side hosting model](xref:blazor/hosting-models#client-side) (based on the **Blazor (client-side)** project template, the `blazor` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) and is hosted as an IIS sub-app in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>

<span data-ttu-id="2c5dd-186">Dosyaya bir `<handlers>` bölüm ekleyerek uygulamanın yayınlanan *Web. config* dosyasındaki işleyiciyi kaldırın:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-186">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

```xml
<handlers>
  <remove name="aspNetCore" />
</handlers>
```

<span data-ttu-id="2c5dd-187">Alternatif olarak, `<system.webServer>` şu şekilde `<location>` `inheritInChildApplications` ayarlanmışbiröğekullanarakkök(üst)uygulamanındevralınmasınıdevredışıbırakın:`false`</span><span class="sxs-lookup"><span data-stu-id="2c5dd-187">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

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

<span data-ttu-id="2c5dd-188">İşleyicinin kaldırılması veya devralmayı devre dışı bırakmak, bu bölümde açıklandığı gibi uygulamanın temel yolunu yapılandırmaya ek olarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-188">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="2c5dd-189">Uygulamanın *index. html* dosyasındaki uygulama temel yolunu, IIS 'de alt uygulamayı YAPıLANDıRıRKEN kullanılan IIS diğer adına ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-189">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="2c5dd-190">ASP.NET Core ile barındırılan dağıtım</span><span class="sxs-lookup"><span data-stu-id="2c5dd-190">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="2c5dd-191">*Barındırılan bir dağıtım* , Blazor istemci tarafı uygulamasını Web sunucusunda çalışan [ASP.NET Core bir uygulamadan](xref:index) tarayıcılara sunar.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-191">A *hosted deployment* serves the Blazor client-side app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="2c5dd-192">Blazor uygulaması, yayımlanan çıktıda ASP.NET Core uygulamasına dahildir, böylece iki uygulama birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-192">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="2c5dd-193">ASP.NET Core uygulamasını barındırabilen bir Web sunucusu gereklidir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-193">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="2c5dd-194">Barındırılan bir dağıtım için, Visual Studio **Blazor (ASP.NET Core hosted)** proje şablonunu (`blazorhosted` [DotNet New](/dotnet/core/tools/dotnet-new) komutu kullanılırken şablon) içerir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-194">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="2c5dd-195">Uygulama barındırma ve dağıtım ASP.NET Core hakkında daha fazla bilgi için bkz <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-195">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="2c5dd-196">Azure App Service dağıtma hakkında daha fazla bilgi için bkz <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-196">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="2c5dd-197">Tek başına dağıtım</span><span class="sxs-lookup"><span data-stu-id="2c5dd-197">Standalone deployment</span></span>

<span data-ttu-id="2c5dd-198">*Tek başına dağıtım* , Blazor istemci tarafı uygulamasına doğrudan istemciler tarafından istenen statik dosyalar kümesi olarak hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-198">A *standalone deployment* serves the Blazor client-side app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="2c5dd-199">Herhangi bir statik dosya sunucusu Blazor uygulamasını sunabilir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-199">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="2c5dd-200">Tek başına dağıtım varlıkları *bin/Release/{Target Framework}/publish/{ASSEMBLY Name}/Dist* klasöründe yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-200">Standalone deployment assets are published to the *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span>

### <a name="iis"></a><span data-ttu-id="2c5dd-201">IIS</span><span class="sxs-lookup"><span data-stu-id="2c5dd-201">IIS</span></span>

<span data-ttu-id="2c5dd-202">IIS, Blazor uygulamaları için özellikli bir statik dosya sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-202">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="2c5dd-203">IIS 'yi Blazor barındıracak şekilde yapılandırmak için bkz. [IIS 'de statik Web sitesi oluşturma](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="2c5dd-203">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="2c5dd-204">Yayımlanan varlıklar */BIN/Release/{Target Framework}/Publish* klasöründe oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-204">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="2c5dd-205">Web sunucusunda veya barındırma hizmetinde *Yayımlama* klasörünün içeriğini barındırın.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-205">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="2c5dd-206">Web. config</span><span class="sxs-lookup"><span data-stu-id="2c5dd-206">web.config</span></span>

<span data-ttu-id="2c5dd-207">Bir Blazor projesi yayımlandığında, aşağıdaki IIS yapılandırmasıyla bir *Web. config* dosyası oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-207">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="2c5dd-208">MIME türleri aşağıdaki dosya uzantıları için ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-208">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="2c5dd-209">*. dll* &ndash;`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="2c5dd-209">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="2c5dd-210">*. JSON* &ndash;`application/json`</span><span class="sxs-lookup"><span data-stu-id="2c5dd-210">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="2c5dd-211">*.* &ndash;`application/wasm`</span><span class="sxs-lookup"><span data-stu-id="2c5dd-211">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="2c5dd-212">*. WOFF* &ndash;`application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="2c5dd-212">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="2c5dd-213">*. woff2* &ndash;`application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="2c5dd-213">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="2c5dd-214">Aşağıdaki MIME türleri için HTTP sıkıştırması etkindir:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-214">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="2c5dd-215">URL yeniden yazma modülü kuralları oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-215">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="2c5dd-216">Uygulamanın statik varlıklarının bulunduğu alt dizini ( *{Assembly Name}/dist/{PATH istenen}* ) sunar.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-216">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="2c5dd-217">Dosya olmayan varlıklar için isteklerin statik varlıklar klasöründe ( *{Assembly Name}/Dist/index.html*) uygulamanın varsayılan belgesine yeniden YÖNLENDIRILMESI için Spa geri dönüş yönlendirmesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-217">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="2c5dd-218">URL yeniden yazma modülünü yükler</span><span class="sxs-lookup"><span data-stu-id="2c5dd-218">Install the URL Rewrite Module</span></span>

<span data-ttu-id="2c5dd-219">[URL yeniden yazma modülü](https://www.iis.net/downloads/microsoft/url-rewrite) , URL 'leri yeniden yazmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-219">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="2c5dd-220">Modül varsayılan olarak yüklenmez ve bir Web sunucusu (IIS) rol hizmeti özelliği olarak yükleme için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-220">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="2c5dd-221">Modül IIS Web sitesinden indirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-221">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="2c5dd-222">Modülünü yüklemek için Web Platformu Yükleyicisi 'ni kullanın:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-222">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="2c5dd-223">Yerel olarak, [URL yeniden yazma modülü İndirmeleri sayfasına](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)gidin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-223">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="2c5dd-224">Ingilizce sürümünde, WebPI yükleyicisini indirmek için **WebPI** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-224">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="2c5dd-225">Diğer diller için, yükleyiciyi indirmek üzere sunucu için uygun mimariyi (x86/x64) seçin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-225">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="2c5dd-226">Yükleyiciyi sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-226">Copy the installer to the server.</span></span> <span data-ttu-id="2c5dd-227">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-227">Run the installer.</span></span> <span data-ttu-id="2c5dd-228">, **Install** düğmesini seçin ve lisans koşullarını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-228">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="2c5dd-229">Yüklemesi tamamlandıktan sonra sunucu yeniden başlatması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-229">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="2c5dd-230">Web sitesini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2c5dd-230">Configure the website</span></span>

<span data-ttu-id="2c5dd-231">Web sitesinin **fiziksel yolunu** uygulamanın klasörüne ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-231">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="2c5dd-232">Klasör şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-232">The folder contains:</span></span>

* <span data-ttu-id="2c5dd-233">Gerekli yeniden yönlendirme kuralları ve dosya içerik türleri dahil olmak üzere IIS 'nin Web sitesini yapılandırmak için kullandığı *Web. config* dosyası.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-233">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="2c5dd-234">Uygulamanın statik varlık klasörü.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-234">The app's static asset folder.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="2c5dd-235">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="2c5dd-235">Troubleshooting</span></span>

<span data-ttu-id="2c5dd-236">*500-Iç sunucu hatası* ALıNMıŞSA ve IIS Yöneticisi Web sitesinin yapılandırmasına erişmeye çalışırken hatalar OLUŞTURURSA, URL yeniden yazma modülünün yüklü olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-236">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="2c5dd-237">Modül yüklü olmadığında, *Web. config* dosyası IIS tarafından ayrıştırılamaz.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-237">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="2c5dd-238">Bu, IIS yöneticisinin Web sitesinin yapılandırmasını ve Web sitesinin Blazor 'in statik dosyalarına hizmet etmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-238">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="2c5dd-239">IIS ile dağıtım sorunlarını giderme hakkında daha fazla bilgi için <xref:test/troubleshoot-azure-iis>bkz.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-239">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="2c5dd-240">Azure Depolama</span><span class="sxs-lookup"><span data-stu-id="2c5dd-240">Azure Storage</span></span>

<span data-ttu-id="2c5dd-241">Azure depolama statik dosya barındırma, sunucusuz Blazor uygulamasının barındırılmasına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-241">Azure Storage static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="2c5dd-242">Özel etki alanı adları, Azure Content Delivery Network (CDN) ve HTTPS desteklenir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-242">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="2c5dd-243">Blob hizmeti bir depolama hesabında barındırılan statik Web sitesi için etkinleştirildiğinde:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-243">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="2c5dd-244">**Dizin belgesi adını** olarak `index.html`ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-244">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="2c5dd-245">**Hata belge yolunu** olarak `index.html`ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-245">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="2c5dd-246">Razor bileşenleri ve diğer dosya olmayan uç noktaları, blob hizmeti tarafından depolanan statik içerikte fiziksel yollarda yer vermez.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-246">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="2c5dd-247">Blazor yönlendiricisinin işlemesi gereken bu kaynaklardan birine yönelik bir istek alındığında, blob hizmeti tarafından oluşturulan *404-bulunamayan* hata, isteği **hata belge yoluna**yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-247">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="2c5dd-248">*İndex. html* blobu döndürülür ve Blazor yönlendiricisi yolu yükler ve işler.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-248">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="2c5dd-249">Daha fazla bilgi için bkz. [Azure Storage 'Da statik Web sitesi barındırma](/azure/storage/blobs/storage-blob-static-website).</span><span class="sxs-lookup"><span data-stu-id="2c5dd-249">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="2c5dd-250">NGINX</span><span class="sxs-lookup"><span data-stu-id="2c5dd-250">Nginx</span></span>

<span data-ttu-id="2c5dd-251">Aşağıdaki *NGINX. conf* dosyası, NGINX 'in, disk üzerinde karşılık gelen bir dosyayı bulamadığı her seferinde *index. html* dosyasını göndermek üzere nasıl yapılandırılacağını gösterecek şekilde basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-251">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="2c5dd-252">Üretim NGINX web sunucusu yapılandırması hakkında daha fazla bilgi için bkz. [NGINX Plus ve NGINX yapılandırma dosyaları oluşturma](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span><span class="sxs-lookup"><span data-stu-id="2c5dd-252">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="2c5dd-253">Docker 'da NGINX</span><span class="sxs-lookup"><span data-stu-id="2c5dd-253">Nginx in Docker</span></span>

<span data-ttu-id="2c5dd-254">NGINX kullanarak Docker 'da Blazor barındırmak için Dockerfile 'ı alp tabanlı NGINX görüntüsünü kullanacak şekilde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-254">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="2c5dd-255">Dockerfile dosyasını, *NGINX. config* dosyasını kapsayıcıya kopyalamak için güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-255">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="2c5dd-256">Aşağıdaki örnekte gösterildiği gibi Dockerfile dosyasına bir satır ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2c5dd-256">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a><span data-ttu-id="2c5dd-257">GitHub sayfaları</span><span class="sxs-lookup"><span data-stu-id="2c5dd-257">GitHub Pages</span></span>

<span data-ttu-id="2c5dd-258">URL yeniden işlemesini işlemek için, isteği *index. html* sayfasına yönlendirmeyi işleyen bir betiği olan bir *404. html* dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-258">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="2c5dd-259">Topluluk tarafından sunulan örnek bir uygulama için bkz. [GitHub sayfaları Için tek sayfalı uygulamalar](https://spa-github-pages.rafrex.com/) ([GitHub üzerinde rafrex/Spa-GitHub-Pages](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="2c5dd-259">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="2c5dd-260">Topluluk yaklaşımını kullanan bir örnek, GitHub ([canlı site](https://blazor-demo.github.io/)) [üzerinde blazor-demo/blazor-demo. GitHub. IO](https://github.com/blazor-demo/blazor-demo.github.io) adresinde görülebilir.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-260">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="2c5dd-261">Bir kuruluş sitesi yerine bir proje sitesi kullanırken `<base>` etiketi *index. html*dosyasına ekleyin veya güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="2c5dd-261">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="2c5dd-262">Öznitelik değerini GitHub deposu adına sondaki eğik çizgiyle (örneğin, `my-repository/`) ayarlayın. `href`</span><span class="sxs-lookup"><span data-stu-id="2c5dd-262">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>
