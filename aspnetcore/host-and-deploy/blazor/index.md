---
title: ASP.NET Core Blazor barındırma ve dağıtma
author: guardrex
description: Blazor uygulamalarının nasıl barındırılacağını ve dağıtılacağını öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 238e7fc8f8d64c7847dc8847fb66e22442a3c8e0
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160268"
---
# <a name="host-and-deploy-aspnet-core-opno-locblazor"></a><span data-ttu-id="225fa-103">ASP.NET Core Blazor barındırma ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="225fa-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="225fa-104">, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="225fa-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="225fa-105">Uygulamayı yayımlama</span><span class="sxs-lookup"><span data-stu-id="225fa-105">Publish the app</span></span>

<span data-ttu-id="225fa-106">Uygulamalar yayın yapılandırmasında dağıtım için yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="225fa-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="225fa-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="225fa-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="225fa-108">Gezinti çubuğundan **derleme** >  **{APPLICATION} Yayımla** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="225fa-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="225fa-109">*Yayımla hedefini*seçin.</span><span class="sxs-lookup"><span data-stu-id="225fa-109">Select the *publish target*.</span></span> <span data-ttu-id="225fa-110">Yerel olarak yayımlamak için **klasör**' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="225fa-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="225fa-111">**Klasör seçin** alanında varsayılan konumu kabul edin veya farklı bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="225fa-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="225fa-112">**Yayımla** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="225fa-112">Select the **Publish** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="225fa-113">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="225fa-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="225fa-114">Uygulamayı bir sürüm yapılandırmasıyla yayımlamak için [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="225fa-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```dotnetcli
dotnet publish -c Release
```

---

<span data-ttu-id="225fa-115">Uygulamanın yayımlanması, projenin bağımlılıklarını [geri yüklemeyi](/dotnet/core/tools/dotnet-restore) tetikler ve dağıtım için varlıkları oluşturmadan önce projeyi [oluşturur](/dotnet/core/tools/dotnet-build) .</span><span class="sxs-lookup"><span data-stu-id="225fa-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="225fa-116">Yapı işleminin bir parçası olarak, uygulama indirme boyutunu ve yükleme sürelerini azaltmak için kullanılmayan Yöntemler ve derlemeler kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="225fa-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="225fa-117">Blazor WebAssembly uygulaması */BIN/Release/{Target Framework}/publish/{ASSEMBLY Name}/Dist* klasörüne yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="225fa-117">A Blazor WebAssembly app is published to the */bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span> <span data-ttu-id="225fa-118">Bir Blazor sunucusu uygulaması */BIN/Release/{Target Framework}/Publish* klasörüne yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="225fa-118">A Blazor Server app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="225fa-119">Klasördeki varlıklar Web sunucusuna dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="225fa-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="225fa-120">Dağıtım, kullanımdaki geliştirme araçlarına bağlı olarak el ile veya otomatik bir süreç olabilir.</span><span class="sxs-lookup"><span data-stu-id="225fa-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="225fa-121">Uygulama temel yolu</span><span class="sxs-lookup"><span data-stu-id="225fa-121">App base path</span></span>

<span data-ttu-id="225fa-122">*Uygulama temel yolu* , UYGULAMANıN kök URL yoludur.</span><span class="sxs-lookup"><span data-stu-id="225fa-122">The *app base path* is the app's root URL path.</span></span> <span data-ttu-id="225fa-123">Aşağıdaki ASP.NET Core uygulamayı ve Blazor alt uygulamayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="225fa-123">Consider the following ASP.NET Core app and Blazor sub-app:</span></span>

* <span data-ttu-id="225fa-124">ASP.NET Core uygulamasının adı `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="225fa-124">The ASP.NET Core app is named `MyApp`:</span></span>
  * <span data-ttu-id="225fa-125">Uygulama fiziksel olarak *d:/MyApp*konumunda bulunur.</span><span class="sxs-lookup"><span data-stu-id="225fa-125">The app physically resides at *d:/MyApp*.</span></span>
  * <span data-ttu-id="225fa-126">İstekler `https://www.contoso.com/{MYAPP RESOURCE}`alındı.</span><span class="sxs-lookup"><span data-stu-id="225fa-126">Requests are received at `https://www.contoso.com/{MYAPP RESOURCE}`.</span></span>
* <span data-ttu-id="225fa-127">`CoolApp` adlı bir Blazor uygulaması, `MyApp`alt uygulamasıdır:</span><span class="sxs-lookup"><span data-stu-id="225fa-127">A Blazor app named `CoolApp` is a sub-app of `MyApp`:</span></span>
  * <span data-ttu-id="225fa-128">Alt uygulama fiziksel olarak *d:/MyApp/CoolApp*konumunda bulunur.</span><span class="sxs-lookup"><span data-stu-id="225fa-128">The sub-app physically resides at *d:/MyApp/CoolApp*.</span></span>
  * <span data-ttu-id="225fa-129">İstekler `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`alındı.</span><span class="sxs-lookup"><span data-stu-id="225fa-129">Requests are received at `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span></span>

<span data-ttu-id="225fa-130">`CoolApp`için ek yapılandırma belirtmeden, Bu senaryodaki alt uygulama, sunucuda nerede bulunduğu konusunda bilgi sahibi değildir.</span><span class="sxs-lookup"><span data-stu-id="225fa-130">Without specifying additional configuration for `CoolApp`, the sub-app in this scenario has no knowledge of where it resides on the server.</span></span> <span data-ttu-id="225fa-131">Örneğin, uygulama, `/CoolApp/`göreli URL yolunda bulunduğunu bilmeden kaynaklarına doğru göreli URL 'Ler oluşturamıyoruz.</span><span class="sxs-lookup"><span data-stu-id="225fa-131">For example, the app can't construct correct relative URLs to its resources without knowing that it resides at the relative URL path `/CoolApp/`.</span></span>

<span data-ttu-id="225fa-132">Blazor uygulamasının temel `https://www.contoso.com/CoolApp/`yolu için yapılandırma sağlamak üzere `<base>` etiketinin `href` özniteliği *Pages/_Host. cshtml* dosyasında (Blazor Server) veya *wwwroot/index.html* dosyasında (Blazor webassembly) göreli kök yolu olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="225fa-132">To provide configuration for the Blazor app's base path of `https://www.contoso.com/CoolApp/`, the `<base>` tag's `href` attribute is set to the relative root path in the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly):</span></span>

```html
<base href="/CoolApp/">
```

Blazor<span data-ttu-id="225fa-133"> sunucu uygulamaları, uygulamanın `Startup.Configure`istek ardışık düzeninde <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> çağırarak sunucu tarafı taban yolunu da ayarlar:</span><span class="sxs-lookup"><span data-stu-id="225fa-133"> Server apps additionally set the server-side base path by calling <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> in the app's request pipeline of `Startup.Configure`:</span></span>

```csharp
app.UsePathBase("/CoolApp");
```

<span data-ttu-id="225fa-134">Göreli URL yolunu sağlayarak, kök dizinde olmayan bir bileşen, uygulamanın kök yoluna göre URL 'Ler oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="225fa-134">By providing the relative URL path, a component that isn't in the root directory can construct URLs relative to the app's root path.</span></span> <span data-ttu-id="225fa-135">Farklı dizin yapısı düzeylerindeki bileşenler, uygulama genelinde konumlardaki diğer kaynakların bağlantılarını oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="225fa-135">Components at different levels of the directory structure can build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="225fa-136">Uygulama temel yolu Ayrıca, bağlantının `href` hedefinin uygulama temel yolu URI alanı içinde olduğu seçili köprüleri ele almak için de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="225fa-136">The app base path is also used to intercept selected hyperlinks where the `href` target of the link is within the app base path URI space.</span></span> <span data-ttu-id="225fa-137">Blazor yönlendirici iç gezintiyi işler.</span><span class="sxs-lookup"><span data-stu-id="225fa-137">The Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="225fa-138">Birçok barındırma senaryosunda, uygulamanın göreli URL yolu uygulamanın köküdür.</span><span class="sxs-lookup"><span data-stu-id="225fa-138">In many hosting scenarios, the relative URL path to the app is the root of the app.</span></span> <span data-ttu-id="225fa-139">Bu durumlarda, uygulamanın göreli URL taban yolu bir Blazor uygulamasının varsayılan yapılandırması olan eğik çizgi (`<base href="/" />`) olur.</span><span class="sxs-lookup"><span data-stu-id="225fa-139">In these cases, the app's relative URL base path is a forward slash (`<base href="/" />`), which is the default configuration for a Blazor app.</span></span> <span data-ttu-id="225fa-140">GitHub sayfaları ve IIS alt uygulamaları gibi diğer barındırma senaryolarında, uygulama temel yolu, sunucunun uygulamanın göreli URL 'SI yolu olarak ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="225fa-140">In other hosting scenarios, such as GitHub Pages and IIS sub-apps, the app base path must be set to the server's relative URL path of the app.</span></span>

<span data-ttu-id="225fa-141">Uygulamanın temel yolunu ayarlamak için, *Pages/_Host. cshtml* dosyasının (Blazor Server) veya *wwwroot/index.html* dosyasının (Blazor webassembly) `<head>` etiketi öğeleri içindeki `<base>` etiketini güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="225fa-141">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly).</span></span> <span data-ttu-id="225fa-142">`href` öznitelik değerini `/{RELATIVE URL PATH}/` olarak ayarlayın (sondaki eğik çizgi gereklidir), burada `{RELATIVE URL PATH}` uygulamanın tam göreli URL yoludur.</span><span class="sxs-lookup"><span data-stu-id="225fa-142">Set the `href` attribute value to `/{RELATIVE URL PATH}/` (the trailing slash is required), where `{RELATIVE URL PATH}` is the app's full relative URL path.</span></span>

<span data-ttu-id="225fa-143">Kök olmayan göreli URL yolu (örneğin, `<base href="/CoolApp/">`) olan bir Blazor WebAssembly uygulaması için, uygulama *yerel olarak çalıştırıldığında*kaynaklarını bulamaz.</span><span class="sxs-lookup"><span data-stu-id="225fa-143">For an Blazor WebAssembly app with a non-root relative URL path (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="225fa-144">Yerel geliştirme ve test sırasında bu sorunu aşmak için, çalışma zamanında `<base>` etiketinin `href` değeriyle eşleşen bir *yol temel* bağımsız değişkeni sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="225fa-144">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span> <span data-ttu-id="225fa-145">Sondaki eğik çizgi eklemeyin.</span><span class="sxs-lookup"><span data-stu-id="225fa-145">Don't include a trailing slash.</span></span> <span data-ttu-id="225fa-146">Uygulamayı yerel olarak çalıştırırken yol temel bağımsız değişkenini geçirmek için, `--pathbase` seçeneğiyle uygulamanın dizininden `dotnet run` komutunu yürütün:</span><span class="sxs-lookup"><span data-stu-id="225fa-146">To pass the path base argument when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

<span data-ttu-id="225fa-147">Göreli URL yolu `/CoolApp/` (`<base href="/CoolApp/">`) olan bir Blazor WebAssembly uygulaması için, komut şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="225fa-147">For a Blazor WebAssembly app with a relative URL path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```dotnetcli
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="225fa-148">Blazor WebAssembly uygulaması, `http://localhost:port/CoolApp`yerel olarak yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="225fa-148">The Blazor WebAssembly app responds locally at `http://localhost:port/CoolApp`.</span></span>

## <a name="deployment"></a><span data-ttu-id="225fa-149">Dağıtım</span><span class="sxs-lookup"><span data-stu-id="225fa-149">Deployment</span></span>

<span data-ttu-id="225fa-150">Dağıtım Kılavuzu için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="225fa-150">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
