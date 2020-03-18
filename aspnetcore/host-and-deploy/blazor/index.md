---
title: ASP.NET Core Blazor barındırma ve dağıtma
author: guardrex
description: Blazor uygulamalarının nasıl barındırılacağını ve dağıtılacağını öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/index
ms.openlocfilehash: ddf70da29a82d462422c1bdf74ff45b92bb10b56
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434271"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="eaf8e-103">ASP.NET Core Blazor barındırma ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="eaf8e-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="eaf8e-104">, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="eaf8e-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="eaf8e-105">Uygulamayı yayımlama</span><span class="sxs-lookup"><span data-stu-id="eaf8e-105">Publish the app</span></span>

<span data-ttu-id="eaf8e-106">Uygulamalar yayın yapılandırmasında dağıtım için yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="eaf8e-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eaf8e-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="eaf8e-108">Gezinti çubuğundan **derleme** >  **{APPLICATION} Yayımla** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="eaf8e-109">*Yayımla hedefini*seçin.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-109">Select the *publish target*.</span></span> <span data-ttu-id="eaf8e-110">Yerel olarak yayımlamak için **klasör**' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="eaf8e-111">**Klasör seçin** alanında varsayılan konumu kabul edin veya farklı bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="eaf8e-112">**Yayımla** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-112">Select the **Publish** button.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="eaf8e-113">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="eaf8e-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="eaf8e-114">Uygulamayı bir sürüm yapılandırmasıyla yayımlamak için [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="eaf8e-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```dotnetcli
dotnet publish -c Release
```

---

<span data-ttu-id="eaf8e-115">Uygulamanın yayımlanması, projenin bağımlılıklarını [geri yüklemeyi](/dotnet/core/tools/dotnet-restore) tetikler ve dağıtım için varlıkları oluşturmadan önce projeyi [oluşturur](/dotnet/core/tools/dotnet-build) .</span><span class="sxs-lookup"><span data-stu-id="eaf8e-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="eaf8e-116">Yapı işleminin bir parçası olarak, uygulama indirme boyutunu ve yükleme sürelerini azaltmak için kullanılmayan Yöntemler ve derlemeler kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="eaf8e-117">Yayımlama konumları:</span><span class="sxs-lookup"><span data-stu-id="eaf8e-117">Publish locations:</span></span>

* Blazor<span data-ttu-id="eaf8e-118"> WebAssembly</span><span class="sxs-lookup"><span data-stu-id="eaf8e-118"> WebAssembly</span></span>
  * <span data-ttu-id="eaf8e-119">Uygulamanın tek başına &ndash; */BIN/Release/{Target Framework}/Publish/Wwwroot* klasöründe yayımlanması.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-119">Standalone &ndash; The app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder.</span></span> <span data-ttu-id="eaf8e-120">Uygulamayı statik bir site olarak dağıtmak için *Wwwroot* klasörünün içeriğini statik site konağına kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-120">To deploy the app as a static site, copy the contents of the *wwwroot* folder to the static site host.</span></span>
  * <span data-ttu-id="eaf8e-121">İstemci Blazor WebAssembly uygulamasının barındırılan &ndash;, sunucu uygulamasının diğer statik Web varlıklarıyla birlikte sunucu uygulamasının */BIN/Release/{Target Framework}/Publish/Wwwroot* klasöründe yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-121">Hosted &ndash; The client Blazor WebAssembly app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder of the server app, along with any other static web assets of the server app.</span></span> <span data-ttu-id="eaf8e-122">*Yayımla* klasörünün içeriğini konağa dağıtın.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-122">Deploy the contents of the *publish* folder to the host.</span></span>
* Blazor<span data-ttu-id="eaf8e-123"> sunucu &ndash; *Framework}/Publish* klasöründe yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-123"> Server &ndash; The app is published into the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="eaf8e-124">*Yayımla* klasörünün içeriğini konağa dağıtın.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-124">Deploy the contents of the *publish* folder to the host.</span></span>

<span data-ttu-id="eaf8e-125">Klasördeki varlıklar Web sunucusuna dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-125">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="eaf8e-126">Dağıtım, kullanımdaki geliştirme araçlarına bağlı olarak el ile veya otomatik bir süreç olabilir.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-126">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="eaf8e-127">Uygulama temel yolu</span><span class="sxs-lookup"><span data-stu-id="eaf8e-127">App base path</span></span>

<span data-ttu-id="eaf8e-128">*Uygulama temel yolu* , UYGULAMANıN kök URL yoludur.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-128">The *app base path* is the app's root URL path.</span></span> <span data-ttu-id="eaf8e-129">Aşağıdaki ASP.NET Core uygulamayı ve Blazor alt uygulamayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="eaf8e-129">Consider the following ASP.NET Core app and Blazor sub-app:</span></span>

* <span data-ttu-id="eaf8e-130">ASP.NET Core uygulamasının adı `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="eaf8e-130">The ASP.NET Core app is named `MyApp`:</span></span>
  * <span data-ttu-id="eaf8e-131">Uygulama fiziksel olarak *d:/MyApp*konumunda bulunur.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-131">The app physically resides at *d:/MyApp*.</span></span>
  * <span data-ttu-id="eaf8e-132">İstekler `https://www.contoso.com/{MYAPP RESOURCE}`alındı.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-132">Requests are received at `https://www.contoso.com/{MYAPP RESOURCE}`.</span></span>
* <span data-ttu-id="eaf8e-133">`CoolApp` adlı bir Blazor uygulaması, `MyApp`alt uygulamasıdır:</span><span class="sxs-lookup"><span data-stu-id="eaf8e-133">A Blazor app named `CoolApp` is a sub-app of `MyApp`:</span></span>
  * <span data-ttu-id="eaf8e-134">Alt uygulama fiziksel olarak *d:/MyApp/CoolApp*konumunda bulunur.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-134">The sub-app physically resides at *d:/MyApp/CoolApp*.</span></span>
  * <span data-ttu-id="eaf8e-135">İstekler `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`alındı.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-135">Requests are received at `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span></span>

<span data-ttu-id="eaf8e-136">`CoolApp`için ek yapılandırma belirtmeden, Bu senaryodaki alt uygulama, sunucuda nerede bulunduğu konusunda bilgi sahibi değildir.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-136">Without specifying additional configuration for `CoolApp`, the sub-app in this scenario has no knowledge of where it resides on the server.</span></span> <span data-ttu-id="eaf8e-137">Örneğin, uygulama, `/CoolApp/`göreli URL yolunda bulunduğunu bilmeden kaynaklarına doğru göreli URL 'Ler oluşturamıyoruz.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-137">For example, the app can't construct correct relative URLs to its resources without knowing that it resides at the relative URL path `/CoolApp/`.</span></span>

<span data-ttu-id="eaf8e-138">Blazor uygulamasının temel `https://www.contoso.com/CoolApp/`yolu için yapılandırma sağlamak üzere `<base>` etiketinin `href` özniteliği *Pages/_Host. cshtml* dosyasında (Blazor Server) veya *wwwroot/index.html* dosyasında (Blazor webassembly) göreli kök yolu olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="eaf8e-138">To provide configuration for the Blazor app's base path of `https://www.contoso.com/CoolApp/`, the `<base>` tag's `href` attribute is set to the relative root path in the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly):</span></span>

```html
<base href="/CoolApp/">
```

Blazor<span data-ttu-id="eaf8e-139"> sunucu uygulamaları, uygulamanın `Startup.Configure`istek ardışık düzeninde <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> çağırarak sunucu tarafı taban yolunu da ayarlar:</span><span class="sxs-lookup"><span data-stu-id="eaf8e-139"> Server apps additionally set the server-side base path by calling <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> in the app's request pipeline of `Startup.Configure`:</span></span>

```csharp
app.UsePathBase("/CoolApp");
```

<span data-ttu-id="eaf8e-140">Göreli URL yolunu sağlayarak, kök dizinde olmayan bir bileşen, uygulamanın kök yoluna göre URL 'Ler oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-140">By providing the relative URL path, a component that isn't in the root directory can construct URLs relative to the app's root path.</span></span> <span data-ttu-id="eaf8e-141">Farklı dizin yapısı düzeylerindeki bileşenler, uygulama genelinde konumlardaki diğer kaynakların bağlantılarını oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-141">Components at different levels of the directory structure can build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="eaf8e-142">Uygulama temel yolu Ayrıca, bağlantının `href` hedefinin uygulama temel yolu URI alanı içinde olduğu seçili köprüleri ele almak için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-142">The app base path is also used to intercept selected hyperlinks where the `href` target of the link is within the app base path URI space.</span></span> <span data-ttu-id="eaf8e-143">Blazor yönlendirici iç gezintiyi işler.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-143">The Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="eaf8e-144">Birçok barındırma senaryosunda, uygulamanın göreli URL yolu uygulamanın köküdür.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-144">In many hosting scenarios, the relative URL path to the app is the root of the app.</span></span> <span data-ttu-id="eaf8e-145">Bu durumlarda, uygulamanın göreli URL taban yolu bir Blazor uygulamasının varsayılan yapılandırması olan eğik çizgi (`<base href="/" />`) olur.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-145">In these cases, the app's relative URL base path is a forward slash (`<base href="/" />`), which is the default configuration for a Blazor app.</span></span> <span data-ttu-id="eaf8e-146">GitHub sayfaları ve IIS alt uygulamaları gibi diğer barındırma senaryolarında, uygulama temel yolu, sunucunun uygulamanın göreli URL 'SI yolu olarak ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-146">In other hosting scenarios, such as GitHub Pages and IIS sub-apps, the app base path must be set to the server's relative URL path of the app.</span></span>

<span data-ttu-id="eaf8e-147">Uygulamanın temel yolunu ayarlamak için, *Pages/_Host. cshtml* dosyasının (Blazor Server) veya *wwwroot/index.html* dosyasının (Blazor webassembly) `<head>` etiketi öğeleri içindeki `<base>` etiketini güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-147">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly).</span></span> <span data-ttu-id="eaf8e-148">`href` öznitelik değerini `/{RELATIVE URL PATH}/` olarak ayarlayın (sondaki eğik çizgi gereklidir), burada `{RELATIVE URL PATH}` uygulamanın tam göreli URL yoludur.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-148">Set the `href` attribute value to `/{RELATIVE URL PATH}/` (the trailing slash is required), where `{RELATIVE URL PATH}` is the app's full relative URL path.</span></span>

<span data-ttu-id="eaf8e-149">Kök olmayan göreli URL yolu (örneğin, `<base href="/CoolApp/">`) olan bir Blazor WebAssembly uygulaması için, uygulama *yerel olarak çalıştırıldığında*kaynaklarını bulamaz.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-149">For an Blazor WebAssembly app with a non-root relative URL path (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="eaf8e-150">Yerel geliştirme ve test sırasında bu sorunu aşmak için, çalışma zamanında `<base>` etiketinin `href` değeriyle eşleşen bir *yol temel* bağımsız değişkeni sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-150">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span> <span data-ttu-id="eaf8e-151">Sondaki eğik çizgi eklemeyin.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-151">Don't include a trailing slash.</span></span> <span data-ttu-id="eaf8e-152">Uygulamayı yerel olarak çalıştırırken yol temel bağımsız değişkenini geçirmek için, `--pathbase` seçeneğiyle uygulamanın dizininden `dotnet run` komutunu yürütün:</span><span class="sxs-lookup"><span data-stu-id="eaf8e-152">To pass the path base argument when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

<span data-ttu-id="eaf8e-153">Göreli URL yolu `/CoolApp/` (`<base href="/CoolApp/">`) olan bir Blazor WebAssembly uygulaması için, komut şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="eaf8e-153">For a Blazor WebAssembly app with a relative URL path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```dotnetcli
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="eaf8e-154">Blazor WebAssembly uygulaması, `http://localhost:port/CoolApp`yerel olarak yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="eaf8e-154">The Blazor WebAssembly app responds locally at `http://localhost:port/CoolApp`.</span></span>

## <a name="deployment"></a><span data-ttu-id="eaf8e-155">Dağıtım</span><span class="sxs-lookup"><span data-stu-id="eaf8e-155">Deployment</span></span>

<span data-ttu-id="eaf8e-156">Dağıtım Kılavuzu için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="eaf8e-156">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
