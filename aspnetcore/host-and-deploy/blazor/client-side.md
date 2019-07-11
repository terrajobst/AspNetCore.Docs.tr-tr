---
title: Barındırma ve ASP.NET Core Blazor istemci-tarafı dağıtma
author: guardrex
description: Ana bilgisayar ve ASP.NET Core, içerik teslim ağları (CDN), dosya sunucuları ve GitHub sayfaları kullanarak Blazor uygulamasını dağıtma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: host-and-deploy/blazor/client-side
ms.openlocfilehash: 60fe45626efef70adbf6204e67d011e01b4bc7cb
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815247"
---
# <a name="host-and-deploy-aspnet-core-blazor-client-side"></a><span data-ttu-id="f6851-103">Barındırma ve ASP.NET Core Blazor istemci-tarafı dağıtma</span><span class="sxs-lookup"><span data-stu-id="f6851-103">Host and deploy ASP.NET Core Blazor client-side</span></span>

<span data-ttu-id="f6851-104">Tarafından [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), ve [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f6851-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="f6851-105">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="f6851-105">Host configuration values</span></span>

<span data-ttu-id="f6851-106">Kullanan uygulamalar Blazor [istemci-tarafı barındırma modeli](xref:blazor/hosting-models#client-side) geliştirme ortamında çalışma zamanında komut satırı bağımsız değişkenleri olarak aşağıdaki ana bilgisayar yapılandırma değerlerini kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="f6851-106">Blazor apps that use the [client-side hosting model](xref:blazor/hosting-models#client-side) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="f6851-107">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="f6851-107">Content Root</span></span>

<span data-ttu-id="f6851-108">`--contentroot` Bağımsız değişkeni, uygulamanın içerik dosyaları içeren dizine mutlak yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f6851-108">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span> <span data-ttu-id="f6851-109">Aşağıdaki örneklerde, `/content-root-path` uygulamanın içerik kök yolu.</span><span class="sxs-lookup"><span data-stu-id="f6851-109">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="f6851-110">Bağımsız değişken, uygulamayı bir komut isteminde yerel olarak çalıştırılırken geçirin.</span><span class="sxs-lookup"><span data-stu-id="f6851-110">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="f6851-111">Uygulamanın dizinden yürütün:</span><span class="sxs-lookup"><span data-stu-id="f6851-111">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="f6851-112">Uygulamanın bir giriş ekleyin *launchSettings.json* dosyası **IIS Express** profili.</span><span class="sxs-lookup"><span data-stu-id="f6851-112">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="f6851-113">Bu ayar ile bir komut isteminden Visual Studio hata ayıklayıcı ile uygulama çalıştırıldığında kullanılan `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="f6851-113">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="f6851-114">Visual Studio'da bağımsız değişkende belirtin **özellikleri** > **hata ayıklama** > **uygulamanın bağımsız değişkenleri**.</span><span class="sxs-lookup"><span data-stu-id="f6851-114">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="f6851-115">Ekler için bağımsız değişken bağımsız değişken Visual Studio özellik sayfasında ayarlama *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="f6851-115">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="f6851-116">Temel yol</span><span class="sxs-lookup"><span data-stu-id="f6851-116">Path base</span></span>

<span data-ttu-id="f6851-117">`--pathbase` Bağımsız değişken ayarlar kök olmayan sanal yol ile yerel olarak çalıştırmak için bir uygulama için uygulama temel yolu ( `<base>` etiketi `href` yolu dışında ayarlı `/` hazırlama ve üretim için).</span><span class="sxs-lookup"><span data-stu-id="f6851-117">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="f6851-118">Aşağıdaki örneklerde, `/virtual-path` uygulamanın temel yoludur.</span><span class="sxs-lookup"><span data-stu-id="f6851-118">In the following examples, `/virtual-path` is the app's path base.</span></span> <span data-ttu-id="f6851-119">Daha fazla bilgi için [uygulama temel yolu](#app-base-path) bölümü.</span><span class="sxs-lookup"><span data-stu-id="f6851-119">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f6851-120">Sağlanan yol aksine `href` , `<base>` etiketinde, sonunda bir eğik çizgi eklemeyin (`/`) geçerken `--pathbase` bağımsız değişken değeri.</span><span class="sxs-lookup"><span data-stu-id="f6851-120">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="f6851-121">Uygulama temel yolu sağlanmazsa `<base>` olarak etiketleyin `<base href="/CoolApp/">` (sonunda eğik çizgi içerir), komut satırı bağımsız değişkeni değer olarak geçirmeniz `--pathbase=/CoolApp` (Bitiş eğik).</span><span class="sxs-lookup"><span data-stu-id="f6851-121">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="f6851-122">Bağımsız değişken, uygulamayı bir komut isteminde yerel olarak çalıştırılırken geçirin.</span><span class="sxs-lookup"><span data-stu-id="f6851-122">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="f6851-123">Uygulamanın dizinden yürütün:</span><span class="sxs-lookup"><span data-stu-id="f6851-123">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/virtual-path
  ```

* <span data-ttu-id="f6851-124">Uygulamanın bir giriş ekleyin *launchSettings.json* dosyası **IIS Express** profili.</span><span class="sxs-lookup"><span data-stu-id="f6851-124">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="f6851-125">Bu ayar, uygulama ile bir komut isteminden Visual Studio hata ayıklayıcı ile çalıştırırken kullanılır `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="f6851-125">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/virtual-path"
  ```

* <span data-ttu-id="f6851-126">Visual Studio'da bağımsız değişkende belirtin **özellikleri** > **hata ayıklama** > **uygulamanın bağımsız değişkenleri**.</span><span class="sxs-lookup"><span data-stu-id="f6851-126">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="f6851-127">Ekler için bağımsız değişken bağımsız değişken Visual Studio özellik sayfasında ayarlama *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="f6851-127">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/virtual-path
  ```

### <a name="urls"></a><span data-ttu-id="f6851-128">URL'ler</span><span class="sxs-lookup"><span data-stu-id="f6851-128">URLs</span></span>

<span data-ttu-id="f6851-129">`--urls` IP adresi veya konak adresleriyle bağlantı noktaları ve protokoller üzerinde isteklerini dinlemek için bağımsız değişken ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f6851-129">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="f6851-130">Bağımsız değişken, uygulamayı bir komut isteminde yerel olarak çalıştırılırken geçirin.</span><span class="sxs-lookup"><span data-stu-id="f6851-130">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="f6851-131">Uygulamanın dizinden yürütün:</span><span class="sxs-lookup"><span data-stu-id="f6851-131">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="f6851-132">Uygulamanın bir giriş ekleyin *launchSettings.json* dosyası **IIS Express** profili.</span><span class="sxs-lookup"><span data-stu-id="f6851-132">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="f6851-133">Bu ayar, uygulama ile bir komut isteminden Visual Studio hata ayıklayıcı ile çalıştırırken kullanılır `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="f6851-133">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="f6851-134">Visual Studio'da bağımsız değişkende belirtin **özellikleri** > **hata ayıklama** > **uygulamanın bağımsız değişkenleri**.</span><span class="sxs-lookup"><span data-stu-id="f6851-134">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="f6851-135">Ekler için bağımsız değişken bağımsız değişken Visual Studio özellik sayfasında ayarlama *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="f6851-135">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deployment"></a><span data-ttu-id="f6851-136">Dağıtım</span><span class="sxs-lookup"><span data-stu-id="f6851-136">Deployment</span></span>

<span data-ttu-id="f6851-137">İle [istemci-tarafı barındırma modeli](xref:blazor/hosting-models#client-side):</span><span class="sxs-lookup"><span data-stu-id="f6851-137">With the [client-side hosting model](xref:blazor/hosting-models#client-side):</span></span>

* <span data-ttu-id="f6851-138">Tarayıcıya .NET çalışma zamanı Blazor uygulamayı ve bağımlılıkları indirilir.</span><span class="sxs-lookup"><span data-stu-id="f6851-138">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="f6851-139">Uygulamayı doğrudan tarayıcıda kullanıcı Arabirimi iş parçacığında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="f6851-139">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="f6851-140">Aşağıdaki stratejilerin birini ya da desteklenir:</span><span class="sxs-lookup"><span data-stu-id="f6851-140">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="f6851-141">Blazor uygulama, ASP.NET Core uygulaması tarafından sunulur.</span><span class="sxs-lookup"><span data-stu-id="f6851-141">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="f6851-142">Bu strateji ele alınmıştır [barındırılan ASP.NET Core ile dağıtım](#hosted-deployment-with-aspnet-core) bölümü.</span><span class="sxs-lookup"><span data-stu-id="f6851-142">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="f6851-143">Blazor uygulama bir statik barındırma web sunucusu veya hizmeti .NET Blazor uygulama sunmak için burada kullanılmayan yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f6851-143">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="f6851-144">Bu strateji ele alınmıştır [tek başına dağıtımda](#standalone-deployment) bölümü.</span><span class="sxs-lookup"><span data-stu-id="f6851-144">This strategy is covered in the [Standalone deployment](#standalone-deployment) section.</span></span>

## <a name="configure-the-linker"></a><span data-ttu-id="f6851-145">Bağlayıcıyı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f6851-145">Configure the Linker</span></span>

<span data-ttu-id="f6851-146">Gereksiz IL çıkış derlemeleri kaldırmak için her derlemede Ara dil (IL) bağlama Blazor gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="f6851-146">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="f6851-147">Derleme bağlama üzerinde derleme denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="f6851-147">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="f6851-148">Daha fazla bilgi için bkz. <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="f6851-148">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="f6851-149">Doğru yönlendirme URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="f6851-149">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="f6851-150">İstekler sayfası bileşenler için bir istemci-tarafı uygulaması yönlendirme yönlendirme isteklerini bir sunucu tarafı, barındırılan uygulama kadar basit değildir.</span><span class="sxs-lookup"><span data-stu-id="f6851-150">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="f6851-151">İki bileşeni ile bir istemci-tarafı uygulaması göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="f6851-151">Consider a client-side app with two components:</span></span>

* <span data-ttu-id="f6851-152">*Main.Razor* &ndash; uygulama köküne yükler ve bir bağlantı içeren `About` bileşeni (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="f6851-152">*Main.razor* &ndash; Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="f6851-153">*About.Razor* &ndash; `About` bileşeni.</span><span class="sxs-lookup"><span data-stu-id="f6851-153">*About.razor* &ndash; `About` component.</span></span>

<span data-ttu-id="f6851-154">Tarayıcı Adres çubuğunun kullanarak uygulamanın varsayılan belge istendiğinde (örneğin, `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="f6851-154">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="f6851-155">Tarayıcı, bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="f6851-155">The browser makes a request.</span></span>
1. <span data-ttu-id="f6851-156">Varsayılan sayfayı döndürülen, genellikle olduğu *index.html*.</span><span class="sxs-lookup"><span data-stu-id="f6851-156">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="f6851-157">*index.HTML* uygulama bootstraps.</span><span class="sxs-lookup"><span data-stu-id="f6851-157">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="f6851-158">Blazor'ın yönlendirici yükler ve Razor `Main` bileşen işlenir.</span><span class="sxs-lookup"><span data-stu-id="f6851-158">Blazor's router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="f6851-159">Ana sayfanın bağlantısını seçerek `About` bileşeni çalışır istemcide Internet üzerindeki bir isteği yapan tarayıcının Blazor yönlendirici durdurulur çünkü `www.contoso.com` için `About` ve işlenen `About` bileşene.</span><span class="sxs-lookup"><span data-stu-id="f6851-159">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="f6851-160">Tüm istekler için iç uç nokta *istemci-tarafı uygulaması içinde* aynı şekilde çalışır: İstek sunucu tarafından barındırılan kaynaklara İnternet'e tarayıcı tabanlı istekleri tetikleyin yok.</span><span class="sxs-lookup"><span data-stu-id="f6851-160">All of the requests for internal endpoints *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="f6851-161">Yönlendirici isteklerini dahili olarak işler.</span><span class="sxs-lookup"><span data-stu-id="f6851-161">The router handles the requests internally.</span></span>

<span data-ttu-id="f6851-162">Bir istek için tarayıcının adres çubuğuna kullanarak denenirse `www.contoso.com/About`, istek başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="f6851-162">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="f6851-163">Böyle bir kaynak uygulamanın Internet konakta, bu nedenle mevcut bir *404 - Bulunamadı* yanıt döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f6851-163">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="f6851-164">Tarayıcılar istemci-tarafı sayfaları için Internet tabanlı konakların isteklerde bulunmak için web sunucuları ve barındırma Hizmetleri sunucusuna fiziksel olarak şirket kaynakları için tüm istekleri yeniden yazmalısınız *index.html* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f6851-164">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="f6851-165">Zaman *index.html* döndürülür, uygulamanın istemci-tarafı yönlendirici alır ve doğru kaynak ile yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="f6851-165">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="f6851-166">Uygulama temel yolu</span><span class="sxs-lookup"><span data-stu-id="f6851-166">App base path</span></span>

<span data-ttu-id="f6851-167">*Uygulama temel yolu* sunucusundaki sanal uygulama kök yolu.</span><span class="sxs-lookup"><span data-stu-id="f6851-167">The *app base path* is the virtual app root path on the server.</span></span> <span data-ttu-id="f6851-168">Örneğin, Contoso sunucusunda bir sanal klasöründeki bulunan uygulama `/CoolApp/` en üst sınırına `https://www.contoso.com/CoolApp` ve sanal bir temel yolu `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="f6851-168">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="f6851-169">Uygulama temel yolu sanal yola ayarlayarak (`<base href="/CoolApp/">`), uygulamayı kullanan sanal sunucu üzerinde bulunduğu yapılır.</span><span class="sxs-lookup"><span data-stu-id="f6851-169">By setting the app base path to the virtual path (`<base href="/CoolApp/">`), the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="f6851-170">Uygulama, uygulama temel yolu kök dizininde değil bir bileşeni uygulama köküne URL'lerini oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f6851-170">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="f6851-171">Bu uygulamada konumlarda diğer kaynakların bağlantılarını oluşturmak için dizin yapısının farklı düzeylerde mevcut bileşenleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="f6851-171">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="f6851-172">Uygulama temel yolu da ele alınması için kullanılan köprüyü tıklattığında nerede `href` bağlantının hedefi olan uygulama temel yolu URI alanı içinde&mdash;iç Gezinti Blazor yönlendirici işler.</span><span class="sxs-lookup"><span data-stu-id="f6851-172">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="f6851-173">Birçok barındırma senaryolarında sunucu uygulamasının sanal yolu uygulamayı köküdür.</span><span class="sxs-lookup"><span data-stu-id="f6851-173">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="f6851-174">Bu gibi durumlarda, uygulama temel yolu bir eğik çizgi olan (`<base href="/" />`), bir uygulama için varsayılan yapılandırma olduğu.</span><span class="sxs-lookup"><span data-stu-id="f6851-174">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="f6851-175">GitHub sayfaları ve IIS sanal dizinler veya alt uygulamalar gibi diğer barındırma senaryolarında, uygulama temel yolu sunucu uygulamasının sanal yolu için ayarlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f6851-175">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="f6851-176">Uygulamanın taban yolu belirlemek için güncelleştirme `<base>` içinde etiketi `<head>` etiketi öğeleri *wwwroot/index.html* dosya.</span><span class="sxs-lookup"><span data-stu-id="f6851-176">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *wwwroot/index.html* file.</span></span> <span data-ttu-id="f6851-177">Ayarlayın `href` öznitelik değerine `/virtual-path/` (sonunda eğik çizgi gereklidir), burada `/virtual-path/` uygulama için sunucuda tam sanal uygulama kök yolu.</span><span class="sxs-lookup"><span data-stu-id="f6851-177">Set the `href` attribute value to `/virtual-path/` (the trailing slash is required), where `/virtual-path/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="f6851-178">Önceki örnekte, sanal yol kümesi `/CoolApp/`: `<base href="/CoolApp/">`.</span><span class="sxs-lookup"><span data-stu-id="f6851-178">In the preceding example, the virtual path is set to `/CoolApp/`: `<base href="/CoolApp/">`.</span></span>

<span data-ttu-id="f6851-179">Yapılandırılmış bir kök olmayan sanal yol ile bir uygulama için (örneğin, `<base href="/CoolApp/">`), kaynaklarını bulmak uygulamanın başarısız *yerel olarak çalıştırdığınızda*.</span><span class="sxs-lookup"><span data-stu-id="f6851-179">For an app with a non-root virtual path configured (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="f6851-180">Yerel geliştirme ve test sırasında bu sorunu çözmek için size sağlayabilir bir *yolu tabanı* eşleşen bağımsız değişken `href` değerini `<base>` zamanında etiketi.</span><span class="sxs-lookup"><span data-stu-id="f6851-180">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="f6851-181">Yol, kök yolu ile temel bağımsız değişken geçmek için (`/`) uygulamayı yerel olarak çalışırken, yürütme `dotnet run` ile uygulamanın dizinine komutunu `--pathbase` seçeneği:</span><span class="sxs-lookup"><span data-stu-id="f6851-181">To pass the path base argument with the root path (`/`) when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```console
dotnet run --pathbase=/{Virtual Path (no trailing slash)}
```

<span data-ttu-id="f6851-182">Bir sanal taban yolu ile bir uygulama için `/CoolApp/` (`<base href="/CoolApp/">`), komut:</span><span class="sxs-lookup"><span data-stu-id="f6851-182">For an app with a virtual base path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="f6851-183">Uygulama yanıt veren yerel olarak adresindeki `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="f6851-183">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="f6851-184">Daha fazla bilgi için üzerinde bölümüne bakın. [yolu temel konak yapılandırma değeri](#path-base).</span><span class="sxs-lookup"><span data-stu-id="f6851-184">For more information, see the section on the [path base host configuration value](#path-base).</span></span>

<span data-ttu-id="f6851-185">Bir uygulama kullanıyorsa [istemci-tarafı barındırma modeli](xref:blazor/hosting-models#client-side) (temel **Blazor (istemci-tarafı)** proje şablonu, `blazor` kullanırken şablon [yeni dotnet](/dotnet/core/tools/dotnet-new) Komut) ve barındırılan bir IIS alt uygulama ASP.NET Core uygulaması olarak, devralınan ASP.NET Core modülü işleyici devre dışı bırakın veya kök (üst) uygulamanın emin olmak önemlidir `<handlers>` konusundaki *web.config* dosyası değil alt uygulama tarafından devralınır.</span><span class="sxs-lookup"><span data-stu-id="f6851-185">If an app uses the [client-side hosting model](xref:blazor/hosting-models#client-side) (based on the **Blazor (client-side)** project template, the `blazor` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) and is hosted as an IIS sub-app in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>

<span data-ttu-id="f6851-186">Uygulama işleyicisinde yayımlanan Kaldır *web.config* dosyasını ekleyerek bir `<handlers>` dosya bölümüne:</span><span class="sxs-lookup"><span data-stu-id="f6851-186">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

```xml
<handlers>
  <remove name="aspNetCore" />
</handlers>
```

<span data-ttu-id="f6851-187">Alternatif olarak, kök (üst) uygulamanın devralma devre dışı `<system.webServer>` kullanma bölümünde bir `<location>` öğeyle `inheritInChildApplications` kümesine `false`:</span><span class="sxs-lookup"><span data-stu-id="f6851-187">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

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

<span data-ttu-id="f6851-188">İşleyici kaldırma veya devralmayı devre dışı bırakarak, uygulamanın taban yolu yapılandırmaya ek olarak, bu bölümde açıklanan şekilde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f6851-188">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="f6851-189">Uygulamanın uygulama temel yolu ayarla *index.html* IIS diğer IIS alt uygulama yapılandırma sırasında kullanılan dosya.</span><span class="sxs-lookup"><span data-stu-id="f6851-189">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="f6851-190">ASP.NET Core ile barındırılan dağıtım</span><span class="sxs-lookup"><span data-stu-id="f6851-190">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="f6851-191">A *dağıtım barındırılan* Blazor istemci-tarafı uygulaması, tarayıcılardan hizmet veren bir [ASP.NET Core uygulaması](xref:index) bir web sunucusunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="f6851-191">A *hosted deployment* serves the Blazor client-side app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="f6851-192">Böylece iki uygulamanın birlikte dağıtılan Blazor uygulama yayımlanan çıktıda ASP.NET Core uygulaması ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="f6851-192">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="f6851-193">ASP.NET Core uygulaması barındırma yeteneğine sahip bir web sunucusu gereklidir.</span><span class="sxs-lookup"><span data-stu-id="f6851-193">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="f6851-194">Barındırılan dağıtım için Visual Studio içerir **Blazor (barındırılan ASP.NET Core)** proje şablonu (`blazorhosted` kullanırken şablon [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu).</span><span class="sxs-lookup"><span data-stu-id="f6851-194">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="f6851-195">ASP.NET Core uygulaması barındırma ve dağıtma hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="f6851-195">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="f6851-196">Azure App Service'e dağıtma hakkında daha fazla bilgi için bkz. <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="f6851-196">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="f6851-197">Tek başına dağıtım</span><span class="sxs-lookup"><span data-stu-id="f6851-197">Standalone deployment</span></span>

<span data-ttu-id="f6851-198">A *tek başına dağıtımda* doğrudan istemcileri tarafından istenen statik dosyaları kümesini Blazor istemci-tarafı uygulaması görür.</span><span class="sxs-lookup"><span data-stu-id="f6851-198">A *standalone deployment* serves the Blazor client-side app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="f6851-199">Herhangi bir statik dosya sunucusu Blazor uygulama hizmet kuramıyor.</span><span class="sxs-lookup"><span data-stu-id="f6851-199">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="f6851-200">Tek başına dağıtım varlıklar yayımlanır *bin/bırakma / {TARGET FRAMEWORK} /publish/ {derleme adı} / dist* klasör.</span><span class="sxs-lookup"><span data-stu-id="f6851-200">Standalone deployment assets are published to the *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span>

### <a name="iis"></a><span data-ttu-id="f6851-201">IIS</span><span class="sxs-lookup"><span data-stu-id="f6851-201">IIS</span></span>

<span data-ttu-id="f6851-202">IIS Blazor uygulamalar için bir statik özellikli dosya sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="f6851-202">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="f6851-203">Konağa Blazor IIS'yi yapılandırmak için bkz: [IIS üzerinde statik Web sitesi oluşturma](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="f6851-203">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="f6851-204">Yayımlanmış varlık oluşturulan */bin/yayın / {hedef çerçeve} / publish* klasör.</span><span class="sxs-lookup"><span data-stu-id="f6851-204">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="f6851-205">İçeriğini barındıran *yayımlama* web sunucusuna ya da barındırma hizmeti klasörü.</span><span class="sxs-lookup"><span data-stu-id="f6851-205">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="f6851-206">Web.config</span><span class="sxs-lookup"><span data-stu-id="f6851-206">web.config</span></span>

<span data-ttu-id="f6851-207">Blazor proje yayımlandığında bir *web.config* dosyası, aşağıdaki IIS yapılandırma ile oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="f6851-207">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="f6851-208">MIME türleri, aşağıdaki dosya uzantıları için ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="f6851-208">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="f6851-209">*.dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="f6851-209">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="f6851-210">*.json* &ndash; `application/json`</span><span class="sxs-lookup"><span data-stu-id="f6851-210">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="f6851-211">*.wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="f6851-211">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="f6851-212">*.woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="f6851-212">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="f6851-213">*.woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="f6851-213">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="f6851-214">HTTP sıkıştırmasını aşağıdaki MIME türleri için etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="f6851-214">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="f6851-215">URL yeniden yazma modülü kuralları oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="f6851-215">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="f6851-216">Uygulamanın statik varlıklar bulunduğu alt dizini hizmet ( *{derleme adı} /dist/ {yolu İSTENEN}* ).</span><span class="sxs-lookup"><span data-stu-id="f6851-216">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="f6851-217">Böylece uygulamanın varsayılan belge, statik varlıklar klasöründeki dosya olmayan varlıklar için istekleri yönlendirilirsiniz SPA geri dönüş yönlendirme oluşturma ( *{derleme NAME}/dist/index.html*).</span><span class="sxs-lookup"><span data-stu-id="f6851-217">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="f6851-218">URL yeniden yazma modülünü yükleme</span><span class="sxs-lookup"><span data-stu-id="f6851-218">Install the URL Rewrite Module</span></span>

<span data-ttu-id="f6851-219">[URL yeniden yazma Modülü](https://www.iis.net/downloads/microsoft/url-rewrite) URL yeniden yazma için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="f6851-219">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="f6851-220">Modül varsayılan olarak yüklü değildir ve bir Web sunucusu (IIS) rolü hizmeti özellik olarak yüklenmesi için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="f6851-220">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="f6851-221">Modül IIS Web sitesinden yüklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="f6851-221">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="f6851-222">Modülü yüklemek için Web Platformu Yükleyicisi'ni kullanın:</span><span class="sxs-lookup"><span data-stu-id="f6851-222">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="f6851-223">Yerel olarak gidin [URL yeniden yazma modülü indirmeler sayfasına](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="f6851-223">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="f6851-224">İngilizce sürümü için **Webpı** Webpı yükleyicisi indirilemedi.</span><span class="sxs-lookup"><span data-stu-id="f6851-224">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="f6851-225">Diğer diller için uygun mimarinin yükleyiciyi indirmek sunucu (x86/x64) seçin.</span><span class="sxs-lookup"><span data-stu-id="f6851-225">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="f6851-226">Yükleyici, sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="f6851-226">Copy the installer to the server.</span></span> <span data-ttu-id="f6851-227">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f6851-227">Run the installer.</span></span> <span data-ttu-id="f6851-228">Seçin **yükleme** düğmesi ve lisans koşullarını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="f6851-228">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="f6851-229">Yükleme tamamlandıktan sonra sunucunun yeniden başlatılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="f6851-229">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="f6851-230">Web sitesi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f6851-230">Configure the website</span></span>

<span data-ttu-id="f6851-231">Web sitesinin ayarlamak **fiziksel yolu** uygulamanın klasörüne.</span><span class="sxs-lookup"><span data-stu-id="f6851-231">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="f6851-232">Klasör içerir:</span><span class="sxs-lookup"><span data-stu-id="f6851-232">The folder contains:</span></span>

* <span data-ttu-id="f6851-233">*Web.config* dosyasını gerekli bir yeniden yönlendirme kuralları dahil olmak üzere Web sitesi yapılandırma ve dosya içerik türleri için IIS kullanır.</span><span class="sxs-lookup"><span data-stu-id="f6851-233">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="f6851-234">Uygulamanın statik varlık klasör.</span><span class="sxs-lookup"><span data-stu-id="f6851-234">The app's static asset folder.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="f6851-235">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="f6851-235">Troubleshooting</span></span>

<span data-ttu-id="f6851-236">Varsa bir *500 - İç sunucu hatası* alınır ve IIS Yöneticisi'ni çalışırken Web sitenizin yapılandırmasına erişim, URL yeniden yazma Modülü yüklü olduğundan emin olmak hataları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f6851-236">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="f6851-237">Modül yüklenmediğinde *web.config* IIS tarafından dosya ayrıştırılamıyor.</span><span class="sxs-lookup"><span data-stu-id="f6851-237">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="f6851-238">Bu IIS Yöneticisi'ni Web sitesinin yapılandırması ve Web sitesi Blazor'ın statik dosyalar hizmetinden yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="f6851-238">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="f6851-239">IIS dağıtım sorunlarını giderme hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="f6851-239">For more information on troubleshooting deployments to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="f6851-240">Azure Depolama</span><span class="sxs-lookup"><span data-stu-id="f6851-240">Azure Storage</span></span>

<span data-ttu-id="f6851-241">Azure depolama statik dosya barındırma sunucusuz Blazor uygulama barındırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="f6851-241">Azure Storage static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="f6851-242">Özel etki alanı adları, Azure Content Delivery Network (CDN) ve HTTPS desteklenir.</span><span class="sxs-lookup"><span data-stu-id="f6851-242">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="f6851-243">Blob hizmetini statik Web sitesi bir depolama hesabında barındırmak için etkinleştirildiğinde:</span><span class="sxs-lookup"><span data-stu-id="f6851-243">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="f6851-244">Ayarlama **dizin belgesi adı** için `index.html`.</span><span class="sxs-lookup"><span data-stu-id="f6851-244">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="f6851-245">Ayarlama **hata belgesi yolu** için `index.html`.</span><span class="sxs-lookup"><span data-stu-id="f6851-245">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="f6851-246">Razor bileşenleri ve diğer dosya olmayan uç noktaları fiziksel yollarda blob hizmeti tarafından depolanan statik içeriğin yer yok.</span><span class="sxs-lookup"><span data-stu-id="f6851-246">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="f6851-247">Blazor yönlendirici işlemesi gerekir, bu kaynakları biri için bir istek alındığında *404 - Bulunamadı* blob hizmeti tarafından oluşturulan hata isteğe yönlendiren **hata belgesi yolu**.</span><span class="sxs-lookup"><span data-stu-id="f6851-247">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="f6851-248">*İndex.html* blob döndürülür ve Blazor yönlendirici yükler ve yolun işler.</span><span class="sxs-lookup"><span data-stu-id="f6851-248">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="f6851-249">Daha fazla bilgi için [Azure Depolama'da statik Web sitesi barındırma](/azure/storage/blobs/storage-blob-static-website).</span><span class="sxs-lookup"><span data-stu-id="f6851-249">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="f6851-250">Nginx</span><span class="sxs-lookup"><span data-stu-id="f6851-250">Nginx</span></span>

<span data-ttu-id="f6851-251">Aşağıdaki *nginx.conf* dosya göndermek için Ngınx yapılandırma gösterecek şekilde basitleştirilir *index.html* karşılık gelen bir dosya diskte bulunamıyor her dosya.</span><span class="sxs-lookup"><span data-stu-id="f6851-251">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="f6851-252">Üretim Ngınx web sunucusu yapılandırma hakkında daha fazla bilgi için bkz. [oluşturma NGINX Plus ve NGINX yapılandırma dosyalarını](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span><span class="sxs-lookup"><span data-stu-id="f6851-252">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="f6851-253">Docker'da Nginx</span><span class="sxs-lookup"><span data-stu-id="f6851-253">Nginx in Docker</span></span>

<span data-ttu-id="f6851-254">Nginx kullanarak docker'da Blazor barındırmak için Dockerfile, Alpine tabanlı Nginx görüntüsü kullanmak için ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="f6851-254">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="f6851-255">Dockerfile kopyalayın güncelleştirme *nginx.config* kapsayıcı içine bir dosya.</span><span class="sxs-lookup"><span data-stu-id="f6851-255">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="f6851-256">Aşağıdaki örnekte gösterildiği gibi bir satırı Dockerfile içine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f6851-256">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a><span data-ttu-id="f6851-257">GitHub sayfaları</span><span class="sxs-lookup"><span data-stu-id="f6851-257">GitHub Pages</span></span>

<span data-ttu-id="f6851-258">URL yeniden yazma işlemleri işlemek için ekleme bir *404. html* yeniden yönlendirme isteği işleyen bir komut dosyasıyla *index.html* sayfası.</span><span class="sxs-lookup"><span data-stu-id="f6851-258">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="f6851-259">Topluluk tarafından sağlanan bir örnek için bkz: [GitHub sayfaları için tek sayfa uygulamaları](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-sayfaları github'da](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="f6851-259">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="f6851-260">Topluluk yaklaşımı kullanarak bir örnek, görülebilir [github'da blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([Canlı site](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="f6851-260">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="f6851-261">Proje sitesi yerine bir kuruluş site kullanırken, ekleme veya güncelleştirme `<base>` içindeki *index.html*.</span><span class="sxs-lookup"><span data-stu-id="f6851-261">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="f6851-262">Ayarlama `href` sonunda eğik çizgi ile GitHub deposu adı için öznitelik değeri (örneğin, `my-repository/`.</span><span class="sxs-lookup"><span data-stu-id="f6851-262">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>
