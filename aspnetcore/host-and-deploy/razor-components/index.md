---
title: Barındırma ve Razor bileşenleri dağıtma
author: guardrex
description: 'Ana bilgisayar ve ASP.NET Core, içerik teslim ağları (CDN), dosya sunucuları ve GitHub sayfaları kullanarak Razor bileşenleri ve Blazor uygulamaları dağıtmak nasıl keşfedin.'
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/razor-components/index
---
# <a name="host-and-deploy-razor-components"></a><span data-ttu-id="7bddd-103">Barındırma ve Razor bileşenleri dağıtma</span><span class="sxs-lookup"><span data-stu-id="7bddd-103">Host and deploy Razor Components</span></span>

<span data-ttu-id="7bddd-104">Tarafından [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), ve [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="7bddd-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="7bddd-105">Uygulamayı yayımlama</span><span class="sxs-lookup"><span data-stu-id="7bddd-105">Publish the app</span></span>

<span data-ttu-id="7bddd-106">Uygulamalar ile yayın Yapılandırması'nda dağıtım için yayımlanır [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu.</span><span class="sxs-lookup"><span data-stu-id="7bddd-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="7bddd-107">Bir IDE yürütme işleyebilir `dotnet publish` komutunu otomatik olarak yerleşik yayımlama özelliklerini kullanarak, bunu el ile gerekli olmayabilir bu nedenle kullanımda geliştirme araçları bağlı olarak bir komut isteminden komutu yürütün.</span><span class="sxs-lookup"><span data-stu-id="7bddd-107">An IDE may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="7bddd-108">`dotnet publish` Tetikleyiciler bir [geri](/dotnet/core/tools/dotnet-restore) proje bağımlılıklarının ve [yapılar](/dotnet/core/tools/dotnet-build) dağıtım için bir varlık oluşturmadan önce proje.</span><span class="sxs-lookup"><span data-stu-id="7bddd-108">`dotnet publish` triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="7bddd-109">Yapı işleminin bir parçası olarak, kullanılmayan yöntemleri ve derlemeleri, uygulama indirme boyutunu küçültmek ve yükleme süreleri için kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="7bddd-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="7bddd-110">Dağıtım oluşturulan */bin/yayın/&lt;hedef Framework'ü&gt;/ publish* klasör.</span><span class="sxs-lookup"><span data-stu-id="7bddd-110">The deployment is created in the */bin/Release/&lt;target-framework&gt;/publish* folder.</span></span>

<span data-ttu-id="7bddd-111">Varlıkları *yayımlama* klasör, web sunucusuna dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="7bddd-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="7bddd-112">Dağıtım işlemini kullanımda geliştirme araçları bağlı olarak otomatik veya el ile olabilir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="7bddd-113">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="7bddd-113">Host configuration values</span></span>

<span data-ttu-id="7bddd-114">Razor bileşenleri kullanan uygulamalar [sunucu tarafı barındırma modeli](xref:razor-components/hosting-models#server-side-hosting-model) kabul edebilir [Web ana bilgisayar yapılandırma değerlerini](xref:fundamentals/host/web-host#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="7bddd-114">Razor Components apps that use the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model) can accept [Web Host configuration values](xref:fundamentals/host/web-host#host-configuration-values).</span></span>

<span data-ttu-id="7bddd-115">Kullanan uygulamalar Blazor [istemci-tarafı barındırma modeli](xref:razor-components/hosting-models#client-side-hosting-model) geliştirme ortamında çalışma zamanında komut satırı bağımsız değişkenleri olarak aşağıdaki ana bilgisayar yapılandırma değerlerini kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-115">Blazor apps that use the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="7bddd-116">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="7bddd-116">Content Root</span></span>

<span data-ttu-id="7bddd-117">`--contentroot` Bağımsız değişkeni, uygulamanın içerik dosyaları içeren dizine mutlak yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="7bddd-117">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span>

* <span data-ttu-id="7bddd-118">Bağımsız değişken, uygulamayı bir komut isteminde yerel olarak çalıştırılırken geçirin.</span><span class="sxs-lookup"><span data-stu-id="7bddd-118">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="7bddd-119">Uygulamanın dizinden yürütün:</span><span class="sxs-lookup"><span data-stu-id="7bddd-119">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/<content-root>
  ```

* <span data-ttu-id="7bddd-120">Uygulamanın bir giriş ekleyin *launchSettings.json* dosyası **IIS Express** profili.</span><span class="sxs-lookup"><span data-stu-id="7bddd-120">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="7bddd-121">Bu ayar Visual Studio hata ayıklayıcısı ile uygulamayı çalıştırırken ve uygulama ile bir komut isteminden çalıştırırken seçilir `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="7bddd-121">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/<content-root>"
  ```

* <span data-ttu-id="7bddd-122">Visual Studio'da bağımsız değişkende belirtin **özellikleri** > **hata ayıklama** > **uygulamanın bağımsız değişkenleri**.</span><span class="sxs-lookup"><span data-stu-id="7bddd-122">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="7bddd-123">Ekler için bağımsız değişken bağımsız değişken Visual Studio özellik sayfasında ayarlama *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="7bddd-123">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/<content-root>
  ```

### <a name="path-base"></a><span data-ttu-id="7bddd-124">Temel yol</span><span class="sxs-lookup"><span data-stu-id="7bddd-124">Path base</span></span>

<span data-ttu-id="7bddd-125">`--pathbase` Bağımsız değişken ayarlar kök olmayan sanal yol ile yerel olarak çalıştırmak için bir uygulama için uygulama temel yolu ( `<base>` etiketi `href` yolu dışında ayarlı `/` hazırlama ve üretim için).</span><span class="sxs-lookup"><span data-stu-id="7bddd-125">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="7bddd-126">Daha fazla bilgi için [uygulama temel yolu](#app-base-path) bölümü.</span><span class="sxs-lookup"><span data-stu-id="7bddd-126">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7bddd-127">Sağlanan yol aksine `href` , `<base>` etiketinde, sonunda bir eğik çizgi eklemeyin (`/`) geçerken `--pathbase` bağımsız değişken değeri.</span><span class="sxs-lookup"><span data-stu-id="7bddd-127">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="7bddd-128">Uygulama temel yolu sağlanmazsa `<base>` olarak etiketleyin `<base href="/CoolApp/" />` (sonunda eğik çizgi içerir), komut satırı bağımsız değişkeni değer olarak geçirmeniz `--pathbase=/CoolApp` (Bitiş eğik).</span><span class="sxs-lookup"><span data-stu-id="7bddd-128">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/" />` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="7bddd-129">Bağımsız değişken, uygulamayı bir komut isteminde yerel olarak çalıştırılırken geçirin.</span><span class="sxs-lookup"><span data-stu-id="7bddd-129">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="7bddd-130">Uygulamanın dizinden yürütün:</span><span class="sxs-lookup"><span data-stu-id="7bddd-130">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/<virtual-path>
  ```

* <span data-ttu-id="7bddd-131">Uygulamanın bir giriş ekleyin *launchSettings.json* dosyası **IIS Express** profili.</span><span class="sxs-lookup"><span data-stu-id="7bddd-131">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="7bddd-132">Bu ayar Visual Studio hata ayıklayıcısı ile uygulamayı çalıştırırken ve uygulama ile bir komut isteminden çalıştırırken seçilir `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="7bddd-132">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/<virtual-path>"
  ```

* <span data-ttu-id="7bddd-133">Visual Studio'da bağımsız değişkende belirtin **özellikleri** > **hata ayıklama** > **uygulamanın bağımsız değişkenleri**.</span><span class="sxs-lookup"><span data-stu-id="7bddd-133">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="7bddd-134">Ekler için bağımsız değişken bağımsız değişken Visual Studio özellik sayfasında ayarlama *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="7bddd-134">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/<virtual-path>
  ```

### <a name="urls"></a><span data-ttu-id="7bddd-135">URL'ler</span><span class="sxs-lookup"><span data-stu-id="7bddd-135">URLs</span></span>

<span data-ttu-id="7bddd-136">`--urls` IP adresi veya konak adresleriyle bağlantı noktaları ve protokoller üzerinde isteklerini dinlemek için bağımsız değişken belirtir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-136">The `--urls` argument indicates the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="7bddd-137">Bağımsız değişken, uygulamayı bir komut isteminde yerel olarak çalıştırılırken geçirin.</span><span class="sxs-lookup"><span data-stu-id="7bddd-137">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="7bddd-138">Uygulamanın dizinden yürütün:</span><span class="sxs-lookup"><span data-stu-id="7bddd-138">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="7bddd-139">Uygulamanın bir giriş ekleyin *launchSettings.json* dosyası **IIS Express** profili.</span><span class="sxs-lookup"><span data-stu-id="7bddd-139">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="7bddd-140">Bu ayar Visual Studio hata ayıklayıcısı ile uygulamayı çalıştırırken ve uygulama ile bir komut isteminden çalıştırırken seçilir `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="7bddd-140">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="7bddd-141">Visual Studio'da bağımsız değişkende belirtin **özellikleri** > **hata ayıklama** > **uygulamanın bağımsız değişkenleri**.</span><span class="sxs-lookup"><span data-stu-id="7bddd-141">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="7bddd-142">Ekler için bağımsız değişken bağımsız değişken Visual Studio özellik sayfasında ayarlama *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="7bddd-142">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deploy-a-client-side-blazor-app"></a><span data-ttu-id="7bddd-143">Bir istemci-tarafı Blazor uygulaması dağıtma</span><span class="sxs-lookup"><span data-stu-id="7bddd-143">Deploy a client-side Blazor app</span></span>

<span data-ttu-id="7bddd-144">İle [istemci-tarafı barındırma modeli](xref:razor-components/hosting-models#client-side-hosting-model):</span><span class="sxs-lookup"><span data-stu-id="7bddd-144">With the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model):</span></span>

* <span data-ttu-id="7bddd-145">Tarayıcıya .NET çalışma zamanı Blazor uygulamayı ve bağımlılıkları indirilir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-145">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="7bddd-146">Uygulamayı doğrudan tarayıcıda kullanıcı Arabirimi iş parçacığında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="7bddd-146">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="7bddd-147">Aşağıdaki stratejilerin birini ya da desteklenir:</span><span class="sxs-lookup"><span data-stu-id="7bddd-147">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="7bddd-148">Blazor uygulama, ASP.NET Core uygulaması tarafından sunulur.</span><span class="sxs-lookup"><span data-stu-id="7bddd-148">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="7bddd-149">Ele [istemci-tarafı Blazor barındırılan ASP.NET Core ile dağıtım](#client-side-blazor-hosted-deployment-with-aspnet-core) bölümü.</span><span class="sxs-lookup"><span data-stu-id="7bddd-149">Covered in the [Client-side Blazor hosted deployment with ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="7bddd-150">Blazor uygulama bir statik barındırma web sunucusu veya hizmeti .NET Blazor uygulama sunmak için burada kullanılmayan yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-150">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="7bddd-151">Ele [istemci-tarafı Blazor tek başına dağıtımda](#client-side-blazor-standalone-deployment) bölümü.</span><span class="sxs-lookup"><span data-stu-id="7bddd-151">Covered in the [Client-side Blazor standalone deployment](#client-side-blazor-standalone-deployment) section.</span></span>

### <a name="configure-the-linker"></a><span data-ttu-id="7bddd-152">Bağlayıcıyı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7bddd-152">Configure the Linker</span></span>

<span data-ttu-id="7bddd-153">Gereksiz IL çıkış derlemeleri kaldırmak için her derlemede Ara dil (IL) bağlama Blazor gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-153">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="7bddd-154">Derleme üzerinde derleme bağlama denetleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7bddd-154">You can control assembly linking on build.</span></span> <span data-ttu-id="7bddd-155">Daha fazla bilgi için bkz. <xref:host-and-deploy/razor-components/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="7bddd-155">For more information, see <xref:host-and-deploy/razor-components/configure-linker>.</span></span>

### <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="7bddd-156">Doğru yönlendirme URL yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="7bddd-156">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="7bddd-157">İstekler sayfası bileşenler için bir istemci-tarafı uygulaması yönlendirme yönlendirme isteklerini bir sunucu tarafı, barındırılan uygulama kadar basit değildir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-157">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="7bddd-158">İki sayfa ile bir istemci-tarafı uygulaması göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="7bddd-158">Consider a client-side app with two pages:</span></span>

* <span data-ttu-id="7bddd-159">**_Main.cshtml_**  &ndash; uygulama köküne yükler ve hakkında sayfanın bağlantısını içeren (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="7bddd-159">**_Main.cshtml_** &ndash; Loads at the root of the app and contains a link to the About page (`href="About"`).</span></span>
* <span data-ttu-id="7bddd-160">**_About.cshtml_**  &ndash; hakkında sayfası.</span><span class="sxs-lookup"><span data-stu-id="7bddd-160">**_About.cshtml_** &ndash; About page.</span></span>

<span data-ttu-id="7bddd-161">Tarayıcı Adres çubuğunun kullanarak uygulamanın varsayılan belge istendiğinde (örneğin, `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="7bddd-161">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="7bddd-162">Tarayıcı, bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-162">The browser makes a request.</span></span>
1. <span data-ttu-id="7bddd-163">Varsayılan sayfayı döndürülen, genellikle olduğu *index.html*.</span><span class="sxs-lookup"><span data-stu-id="7bddd-163">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="7bddd-164">*index.HTML* uygulama bootstraps.</span><span class="sxs-lookup"><span data-stu-id="7bddd-164">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="7bddd-165">Blazor'ın yönlendirici yükler ve Razor ana sayfa (*Main.cshtml*) görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-165">Blazor's router loads and the Razor Main page (*Main.cshtml*) is displayed.</span></span>

<span data-ttu-id="7bddd-166">Ana sayfada hakkında sayfasının bağlantısını seçme hakkında sayfası yükler.</span><span class="sxs-lookup"><span data-stu-id="7bddd-166">On the Main page, selecting the link to the About page loads the About page.</span></span> <span data-ttu-id="7bddd-167">Hakkında sayfasının bağlantısını seçerek çalışan istemcide Internet üzerindeki bir isteği yapan tarayıcının Blazor yönlendirici durdurulur çünkü `www.contoso.com` için `About` ve hakkında sayfası görevi görür.</span><span class="sxs-lookup"><span data-stu-id="7bddd-167">Selecting the link to the About page works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the About page itself.</span></span> <span data-ttu-id="7bddd-168">Tüm istekler için iç sayfaları *istemci-tarafı uygulaması içinde* aynı şekilde çalışır: İstek sunucu tarafından barındırılan kaynaklara İnternet'e tarayıcı tabanlı istekleri tetikleyin yok.</span><span class="sxs-lookup"><span data-stu-id="7bddd-168">All of the requests for internal pages *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="7bddd-169">Yönlendirici isteklerini dahili olarak işler.</span><span class="sxs-lookup"><span data-stu-id="7bddd-169">The router handles the requests internally.</span></span>

<span data-ttu-id="7bddd-170">Bir istek için tarayıcının adres çubuğuna kullanarak denenirse `www.contoso.com/About`, istek başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="7bddd-170">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="7bddd-171">Böyle bir kaynak uygulamanın Internet konakta, bu nedenle mevcut bir *404 Bulunamadı Hatası* yanıt döndürülür.</span><span class="sxs-lookup"><span data-stu-id="7bddd-171">No such resource exists on the app's Internet host, so a *404 Not found* response is returned.</span></span>

<span data-ttu-id="7bddd-172">Tarayıcılar istemci-tarafı sayfaları için Internet tabanlı konakların isteklerde bulunmak için web sunucuları ve barındırma Hizmetleri sunucusuna fiziksel olarak şirket kaynakları için tüm istekleri yeniden yazmalısınız *index.html* sayfası.</span><span class="sxs-lookup"><span data-stu-id="7bddd-172">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="7bddd-173">Zaman *index.html* döndürülür, uygulamanın istemci-tarafı yönlendirici alır ve doğru kaynak ile yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-173">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

### <a name="app-base-path"></a><span data-ttu-id="7bddd-174">Uygulama temel yolu</span><span class="sxs-lookup"><span data-stu-id="7bddd-174">App base path</span></span>

<span data-ttu-id="7bddd-175">Uygulama temel yolu sunucuda sanal uygulama kök yoludur.</span><span class="sxs-lookup"><span data-stu-id="7bddd-175">The app base path is the virtual app root path on the server.</span></span> <span data-ttu-id="7bddd-176">Örneğin, Contoso sunucusunda bir sanal klasöründeki bulunan uygulama `/CoolApp/` en üst sınırına `https://www.contoso.com/CoolApp` ve sanal bir temel yolu `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="7bddd-176">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="7bddd-177">Uygulama temel yolu ayarlayarak `CoolApp/`, uygulamayı kullanan sanal sunucu üzerinde bulunduğu yapılır.</span><span class="sxs-lookup"><span data-stu-id="7bddd-177">By setting the app base path to `CoolApp/`, the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="7bddd-178">Uygulama, uygulama temel yolu kök dizininde değil bir bileşeni uygulama köküne URL'lerini oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7bddd-178">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="7bddd-179">Bu uygulamada konumlarda diğer kaynakların bağlantılarını oluşturmak için dizin yapısının farklı düzeylerde mevcut bileşenleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="7bddd-179">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="7bddd-180">Uygulama temel yolu da ele alınması için kullanılan köprüyü tıklattığında nerede `href` bağlantının hedefi olan uygulama temel yolu URI alanı içinde&mdash;iç Gezinti Blazor yönlendirici işler.</span><span class="sxs-lookup"><span data-stu-id="7bddd-180">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="7bddd-181">Birçok barındırma senaryolarında sunucu uygulamasının sanal yolu uygulamayı köküdür.</span><span class="sxs-lookup"><span data-stu-id="7bddd-181">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="7bddd-182">Bu gibi durumlarda, uygulama temel yolu bir eğik çizgi olan (`<base href="/" />`), bir uygulama için varsayılan yapılandırma olduğu.</span><span class="sxs-lookup"><span data-stu-id="7bddd-182">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="7bddd-183">GitHub sayfaları ve IIS sanal dizinler veya alt uygulamalar gibi diğer barındırma senaryolarında, uygulama temel yolu sunucu uygulamasının sanal yolu için ayarlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-183">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="7bddd-184">Uygulamanın temel yolunu ayarlamak için ekleme veya güncelleştirme `<base>` içindeki *index.html* içinde bulunan `<head>` öğeleri etiketleyin.</span><span class="sxs-lookup"><span data-stu-id="7bddd-184">To set the app's base path, add or update the `<base>` tag in *index.html* found within the `<head>` tag elements.</span></span> <span data-ttu-id="7bddd-185">Ayarlayın `href` öznitelik değerine `<virtual-path>/` (sonunda eğik çizgi gereklidir), burada `<virtual-path>/` uygulama için sunucuda tam sanal uygulama kök yolu.</span><span class="sxs-lookup"><span data-stu-id="7bddd-185">Set the `href` attribute value to `<virtual-path>/` (the trailing slash is required), where `<virtual-path>/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="7bddd-186">Önceki örnekte, sanal yol kümesi `CoolApp/`: `<base href="CoolApp/" />`.</span><span class="sxs-lookup"><span data-stu-id="7bddd-186">In the preceding example, the virtual path is set to `CoolApp/`: `<base href="CoolApp/" />`.</span></span>

<span data-ttu-id="7bddd-187">Yapılandırılmış bir kök olmayan sanal yol ile bir uygulama için (örneğin, `<base href="CoolApp/" />`), kaynaklarını bulmak uygulamanın başarısız *yerel olarak çalıştırdığınızda*.</span><span class="sxs-lookup"><span data-stu-id="7bddd-187">For an app with a non-root virtual path configured (for example, `<base href="CoolApp/" />`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="7bddd-188">Yerel geliştirme ve test sırasında bu sorunu çözmek için size sağlayabilir bir *yolu tabanı* eşleşen bağımsız değişken `href` değerini `<base>` zamanında etiketi.</span><span class="sxs-lookup"><span data-stu-id="7bddd-188">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="7bddd-189">Yol, kök yolu ile temel bağımsız değişken geçmek için (`/`) uygulamayı yerel olarak çalıştırırken, uygulamanın dizininden aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="7bddd-189">To pass the path base argument with the root path (`/`) when running the app locally, execute the following command from the app's directory:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="7bddd-190">Uygulama yanıt veren yerel olarak adresindeki `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="7bddd-190">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="7bddd-191">Daha fazla bilgi için [yolu temel konak yapılandırma değeri](#path-base) bölümü.</span><span class="sxs-lookup"><span data-stu-id="7bddd-191">For more information, see the [path base host configuration value](#path-base) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7bddd-192">Bir uygulama kullanıyorsa [istemci-tarafı barındırma modeli](xref:razor-components/hosting-models#client-side-hosting-model) (temel **Blazor** proje şablonu) ve barındırılan bir IIS alt uygulamada ASP.NET Core uygulaması, bu devralınan ASP.NET Core devre dışı bırakmanız önemlidir Modül işleyicisi veya kök (üst) uygulamanın emin `<handlers>` konusundaki *web.config* dosya alt uygulama tarafından devralınan değil.</span><span class="sxs-lookup"><span data-stu-id="7bddd-192">If an app uses the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) (based on the **Blazor** project template) and is hosted as an IIS sub-application in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>
>
> <span data-ttu-id="7bddd-193">Uygulama işleyicisinde yayımlanan Kaldır *web.config* dosyasını ekleyerek bir `<handlers>` dosya bölümüne:</span><span class="sxs-lookup"><span data-stu-id="7bddd-193">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>
>
> ```xml
> <handlers>
>   <remove name="aspNetCore" />
> </handlers>
> ```
>
> <span data-ttu-id="7bddd-194">Alternatif olarak, kök (üst) uygulamanın devralma devre dışı `<system.webServer>` kullanma bölümünde bir `<location>` öğeyle `inheritInChildApplications` kümesine `false`:</span><span class="sxs-lookup"><span data-stu-id="7bddd-194">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <configuration>
>  <location path="." inheritInChildApplications="false">
>    <system.webServer>
>      <handlers>
>        <add name="aspNetCore" ... />
>      </handlers>
>      <aspNetCore ... />
>    </system.webServer>
>  </location>
> </configuration>
> ```
>
> <span data-ttu-id="7bddd-195">İşleyici kaldırma veya devralmayı devre dışı bırakarak, uygulamanın taban yolu yapılandırmaya ek olarak, bu bölümde açıklanan şekilde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-195">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="7bddd-196">Uygulamanın uygulama temel yolu ayarla *index.html* IIS diğer IIS alt uygulama yapılandırma sırasında kullanılan dosya.</span><span class="sxs-lookup"><span data-stu-id="7bddd-196">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

### <a name="client-side-blazor-hosted-deployment-with-aspnet-core"></a><span data-ttu-id="7bddd-197">ASP.NET Core ile istemci tarafı Blazor barındırılan dağıtım</span><span class="sxs-lookup"><span data-stu-id="7bddd-197">Client-side Blazor hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="7bddd-198">A *dağıtım barındırılan* tarayıcılardan istemci-tarafı Blazor uygulama yarar bir [ASP.NET Core uygulaması](xref:index) bir sunucuda çalışır.</span><span class="sxs-lookup"><span data-stu-id="7bddd-198">A *hosted deployment* serves the client-side Blazor app to browsers from an [ASP.NET Core app](xref:index) that runs on a server.</span></span>

<span data-ttu-id="7bddd-199">Böylece iki uygulamanın birlikte dağıtılan Blazor uygulama yayımlanan çıktıda ASP.NET Core uygulaması ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-199">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="7bddd-200">ASP.NET Core uygulaması barındırma yeteneğine sahip bir web sunucusu gereklidir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-200">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="7bddd-201">Barındırılan dağıtım için Visual Studio içerir **Blazor (barındırılan ASP.NET Core)** proje şablonu (`blazorhosted` kullanırken şablon [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu).</span><span class="sxs-lookup"><span data-stu-id="7bddd-201">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="7bddd-202">ASP.NET Core uygulaması barındırma ve dağıtma hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="7bddd-202">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="7bddd-203">Azure App Service'e dağıtma hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="7bddd-203">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>

<span data-ttu-id="7bddd-204">Visual Studio kullanarak Azure App Service'e bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="7bddd-204">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

### <a name="client-side-blazor-standalone-deployment"></a><span data-ttu-id="7bddd-205">İstemci tarafı Blazor tek başına dağıtım</span><span class="sxs-lookup"><span data-stu-id="7bddd-205">Client-side Blazor standalone deployment</span></span>

<span data-ttu-id="7bddd-206">A *tek başına dağıtımda* doğrudan istemcileri tarafından istenen statik dosyalar bir dizi istemci-tarafı Blazor uygulama görür.</span><span class="sxs-lookup"><span data-stu-id="7bddd-206">A *standalone deployment* serves the client-side Blazor app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="7bddd-207">Bir web sunucusu Blazor uygulama sunmak için kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="7bddd-207">A web server isn't used to serve the Blazor app.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-iis"></a><span data-ttu-id="7bddd-208">İstemci tarafı Blazor tek başına ile IIS barındırma</span><span class="sxs-lookup"><span data-stu-id="7bddd-208">Client-side Blazor standalone hosting with IIS</span></span>

<span data-ttu-id="7bddd-209">IIS Blazor uygulamalar için bir statik özellikli dosya sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="7bddd-209">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="7bddd-210">Konağa Blazor IIS'yi yapılandırmak için bkz: [IIS üzerinde statik Web sitesi oluşturma](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="7bddd-210">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="7bddd-211">Yayımlanmış varlık oluşturulur  *\\bin\\yayın\\&lt;hedef Framework'ü&gt;\\yayımlama* klasör.</span><span class="sxs-lookup"><span data-stu-id="7bddd-211">Published assets are created in the *\\bin\\Release\\&lt;target-framework&gt;\\publish* folder.</span></span> <span data-ttu-id="7bddd-212">İçeriğini barındıran *yayımlama* web sunucusuna ya da barındırma hizmeti klasörü.</span><span class="sxs-lookup"><span data-stu-id="7bddd-212">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

<span data-ttu-id="7bddd-213">**Web.config**</span><span class="sxs-lookup"><span data-stu-id="7bddd-213">**web.config**</span></span>

<span data-ttu-id="7bddd-214">Blazor proje yayımlandığında bir *web.config* dosyası, aşağıdaki IIS yapılandırma ile oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="7bddd-214">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="7bddd-215">MIME türleri, aşağıdaki dosya uzantıları için ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="7bddd-215">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="7bddd-216">*\*.dll*: `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="7bddd-216">*\*.dll*: `application/octet-stream`</span></span>
  * <span data-ttu-id="7bddd-217">*\*.JSON*: `application/json`</span><span class="sxs-lookup"><span data-stu-id="7bddd-217">*\*.json*: `application/json`</span></span>
  * <span data-ttu-id="7bddd-218">*\*.wasm*: `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="7bddd-218">*\*.wasm*: `application/wasm`</span></span>
  * <span data-ttu-id="7bddd-219">*\*.woff*: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="7bddd-219">*\*.woff*: `application/font-woff`</span></span>
  * <span data-ttu-id="7bddd-220">*\*.woff2*: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="7bddd-220">*\*.woff2*: `application/font-woff`</span></span>
* <span data-ttu-id="7bddd-221">HTTP sıkıştırmasını aşağıdaki MIME türleri için etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="7bddd-221">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="7bddd-222">URL yeniden yazma modülü kuralları oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="7bddd-222">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="7bddd-223">Uygulamanın statik varlıklar bulunduğu alt dizini hizmet (*&lt;assembly_name&gt;\\dist\\&lt;path_requested&gt;*).</span><span class="sxs-lookup"><span data-stu-id="7bddd-223">Serve the sub-directory where the app's static assets reside (*&lt;assembly_name&gt;\\dist\\&lt;path_requested&gt;*).</span></span>
  * <span data-ttu-id="7bddd-224">Böylece uygulamanın varsayılan belge, statik varlıklar klasöründeki dosya olmayan varlıklar için istekleri yönlendirilirsiniz SPA geri dönüş yönlendirme oluşturma (*&lt;assembly_name&gt;\\dist\\index.html*).</span><span class="sxs-lookup"><span data-stu-id="7bddd-224">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*&lt;assembly_name&gt;\\dist\\index.html*).</span></span>

<span data-ttu-id="7bddd-225">**URL yeniden yazma modülünü yükleme**</span><span class="sxs-lookup"><span data-stu-id="7bddd-225">**Install the URL Rewrite Module**</span></span>

<span data-ttu-id="7bddd-226">[URL yeniden yazma Modülü](https://www.iis.net/downloads/microsoft/url-rewrite) URL yeniden yazma için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-226">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="7bddd-227">Modül varsayılan olarak yüklü değildir ve bir Web sunucusu (IIS) rol hizmeti özellik olarak yüklenmesi için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="7bddd-227">The module isn't installed by default, and it isn't available for install as an Web Server (IIS) role service feature.</span></span> <span data-ttu-id="7bddd-228">Modül IIS Web sitesinden yüklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-228">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="7bddd-229">Modülü yüklemek için Web Platformu Yükleyicisi'ni kullanın:</span><span class="sxs-lookup"><span data-stu-id="7bddd-229">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="7bddd-230">Yerel olarak gidin [URL yeniden yazma modülü indirmeler sayfasına](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="7bddd-230">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="7bddd-231">İngilizce sürümü için **Webpı** Webpı yükleyicisi indirilemedi.</span><span class="sxs-lookup"><span data-stu-id="7bddd-231">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="7bddd-232">Diğer diller için uygun mimarinin yükleyiciyi indirmek sunucu (x86/x64) seçin.</span><span class="sxs-lookup"><span data-stu-id="7bddd-232">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="7bddd-233">Yükleyici, sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="7bddd-233">Copy the installer to the server.</span></span> <span data-ttu-id="7bddd-234">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7bddd-234">Run the installer.</span></span> <span data-ttu-id="7bddd-235">Seçin **yükleme** düğmesi ve lisans koşullarını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="7bddd-235">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="7bddd-236">Yükleme tamamlandıktan sonra sunucunun yeniden başlatılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-236">A server restart isn't required after the install completes.</span></span>

<span data-ttu-id="7bddd-237">**Web sitesi yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="7bddd-237">**Configure the website**</span></span>

<span data-ttu-id="7bddd-238">Web sitesinin ayarlamak **fiziksel yolu** uygulamanın klasörüne.</span><span class="sxs-lookup"><span data-stu-id="7bddd-238">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="7bddd-239">Klasör içerir:</span><span class="sxs-lookup"><span data-stu-id="7bddd-239">The folder contains:</span></span>

* <span data-ttu-id="7bddd-240">*Web.config* dosyasını gerekli bir yeniden yönlendirme kuralları dahil olmak üzere Web sitesi yapılandırma ve dosya içerik türleri için IIS kullanır.</span><span class="sxs-lookup"><span data-stu-id="7bddd-240">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="7bddd-241">Uygulamanın statik varlık klasör.</span><span class="sxs-lookup"><span data-stu-id="7bddd-241">The app's static asset folder.</span></span>

<span data-ttu-id="7bddd-242">**Sorun giderme**</span><span class="sxs-lookup"><span data-stu-id="7bddd-242">**Troubleshooting**</span></span>

<span data-ttu-id="7bddd-243">Varsa bir *500 İç sunucu hatası* alınır ve IIS Yöneticisi'ni çalışırken Web sitenizin yapılandırmasına erişim, URL yeniden yazma Modülü yüklü olduğundan emin olmak hataları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7bddd-243">If a *500 Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="7bddd-244">Modül yüklenmediğinde *web.config* IIS tarafından dosya ayrıştırılamıyor.</span><span class="sxs-lookup"><span data-stu-id="7bddd-244">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="7bddd-245">Bu IIS Yöneticisi'ni Web sitesinin yapılandırması ve Web sitesi Blazor'ın statik dosyalar hizmetinden yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="7bddd-245">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="7bddd-246">IIS dağıtım sorunlarını giderme hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="7bddd-246">For more information on troubleshooting deployments to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx"></a><span data-ttu-id="7bddd-247">İstemci tarafı Blazor tek başına Ngınx ile barındırma</span><span class="sxs-lookup"><span data-stu-id="7bddd-247">Client-side Blazor standalone hosting with Nginx</span></span>

<span data-ttu-id="7bddd-248">Aşağıdaki *nginx.conf* dosya göndermek için Ngınx yapılandırma gösterecek şekilde basitleştirilir *Index.html* karşılık gelen bir dosya diskte bulunamıyor her dosya.</span><span class="sxs-lookup"><span data-stu-id="7bddd-248">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *Index.html* file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /Index.html =404;
        }
    }
}
```

<span data-ttu-id="7bddd-249">Üretim Ngınx web sunucusu yapılandırma hakkında daha fazla bilgi için bkz. [oluşturma NGINX Plus ve NGINX yapılandırma dosyalarını](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span><span class="sxs-lookup"><span data-stu-id="7bddd-249">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx-in-docker"></a><span data-ttu-id="7bddd-250">İstemci tarafı Blazor tek başına ile Ngınx docker'da barındırma</span><span class="sxs-lookup"><span data-stu-id="7bddd-250">Client-side Blazor standalone hosting with Nginx in Docker</span></span>

<span data-ttu-id="7bddd-251">Nginx kullanarak docker'da Blazor barındırmak için Dockerfile, Alpine tabanlı Nginx görüntüsü kullanmak için ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7bddd-251">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="7bddd-252">Dockerfile kopyalayın güncelleştirme *nginx.config* kapsayıcı içine bir dosya.</span><span class="sxs-lookup"><span data-stu-id="7bddd-252">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="7bddd-253">Aşağıdaki örnekte gösterildiği gibi bir satırı Dockerfile içine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7bddd-253">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

#### <a name="client-side-blazor-standalone-hosting-with-github-pages"></a><span data-ttu-id="7bddd-254">İstemci tarafı Blazor tek başına GitHub sayfalarıyla barındırma</span><span class="sxs-lookup"><span data-stu-id="7bddd-254">Client-side Blazor standalone hosting with GitHub Pages</span></span>

<span data-ttu-id="7bddd-255">URL yeniden yazma işlemleri işlemek için ekleme bir *404. html* yeniden yönlendirme isteği işleyen bir komut dosyasıyla *index.html* sayfası.</span><span class="sxs-lookup"><span data-stu-id="7bddd-255">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="7bddd-256">Topluluk tarafından sağlanan bir örnek için bkz: [GitHub sayfaları için tek sayfa uygulamaları](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-sayfaları github'da](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="7bddd-256">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="7bddd-257">Topluluk yaklaşımı kullanarak bir örnek, görülebilir [github'da blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([Canlı site](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="7bddd-257">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="7bddd-258">Proje sitesi yerine bir kuruluş site kullanırken, ekleme veya güncelleştirme `<base>` içindeki *index.html*.</span><span class="sxs-lookup"><span data-stu-id="7bddd-258">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="7bddd-259">Ayarlama `href` öznitelik değerine `<repository-name>/`burada `<repository-name>/` GitHub deposu adı.</span><span class="sxs-lookup"><span data-stu-id="7bddd-259">Set the `href` attribute value to `<repository-name>/`, where `<repository-name>/` is the GitHub repository name.</span></span>

## <a name="deploy-a-server-side-razor-components-app"></a><span data-ttu-id="7bddd-260">Bir sunucu tarafı Razor bileşenleri uygulaması dağıtma</span><span class="sxs-lookup"><span data-stu-id="7bddd-260">Deploy a server-side Razor Components app</span></span>

<span data-ttu-id="7bddd-261">İle [sunucu tarafı barındırma modeli](xref:razor-components/hosting-models#server-side-hosting-model), Razor bileşenleri sunucusunda bir ASP.NET Core uygulaması içinde yürütülür.</span><span class="sxs-lookup"><span data-stu-id="7bddd-261">With the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="7bddd-262">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrılarını bir SignalR bağlantısı üzerinden işlenir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-262">UI updates, event handling, and JavaScript calls are handled over a SignalR connection.</span></span>

<span data-ttu-id="7bddd-263">Böylece iki uygulamanın birlikte dağıtılan uygulamayı yayımlanan çıktıda ASP.NET Core uygulaması ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-263">The app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="7bddd-264">ASP.NET Core uygulaması barındırma yeteneğine sahip bir web sunucusu gereklidir.</span><span class="sxs-lookup"><span data-stu-id="7bddd-264">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="7bddd-265">Sunucu tarafı dağıtımı için Visual Studio içerir **Blazor (sunucu tarafı ASP.NET core'da)** proje şablonu (`blazorserver` kullanırken şablon [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu).</span><span class="sxs-lookup"><span data-stu-id="7bddd-265">For a server-side deployment, Visual Studio includes the **Blazor (Server-side in ASP.NET Core)** project template (`blazorserver` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="7bddd-266">ASP.NET Core uygulaması yayımlandığında, ASP.NET Core uygulaması ve Razor bileşenleri uygulama birlikte dağıtılabilir Razor bileşenleri uygulama yayımlanan çıktısında dahil olduğu.</span><span class="sxs-lookup"><span data-stu-id="7bddd-266">When the ASP.NET Core app is published, the Razor Components app is included in the published output so that the ASP.NET Core app and the Razor Components app can be deployed together.</span></span> <span data-ttu-id="7bddd-267">ASP.NET Core uygulaması barındırma ve dağıtma hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="7bddd-267">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="7bddd-268">Azure App Service'e dağıtma hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="7bddd-268">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>

<span data-ttu-id="7bddd-269">Visual Studio kullanarak Azure App Service'e bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="7bddd-269">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>
