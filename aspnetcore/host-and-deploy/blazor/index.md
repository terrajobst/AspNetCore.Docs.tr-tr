---
title: ASP.NET Core Blazor barındırma ve dağıtma
author: guardrex
description: Blazor uygulamalarının nasıl barındırılacağını ve dağıtılacağını öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: d18abbf33c71dca5130bfc6b503b46c1d5bce537
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68913933"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="c0f50-103">ASP.NET Core Blazor barındırma ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="c0f50-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="c0f50-104">, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="c0f50-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="c0f50-105">Uygulamayı yayımlama</span><span class="sxs-lookup"><span data-stu-id="c0f50-105">Publish the app</span></span>

<span data-ttu-id="c0f50-106">Uygulamalar yayın yapılandırmasında dağıtım için yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="c0f50-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c0f50-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0f50-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="c0f50-108">Gezinti çubuğundan **Build** > **Publish {APPLICATION}** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="c0f50-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="c0f50-109">*Yayımla hedefini*seçin.</span><span class="sxs-lookup"><span data-stu-id="c0f50-109">Select the *publish target*.</span></span> <span data-ttu-id="c0f50-110">Yerel olarak yayımlamak için **klasör**' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="c0f50-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="c0f50-111">**Klasör seçin** alanında varsayılan konumu kabul edin veya farklı bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="c0f50-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="c0f50-112">**Yayımla** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="c0f50-112">Select the **Publish** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c0f50-113">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c0f50-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c0f50-114">Uygulamayı bir sürüm yapılandırmasıyla yayımlamak için [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="c0f50-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```console
dotnet publish -c Release
```

---

<span data-ttu-id="c0f50-115">Uygulamanın yayımlanması, projenin bağımlılıklarını [geri yüklemeyi](/dotnet/core/tools/dotnet-restore) tetikler ve dağıtım için varlıkları oluşturmadan önce projeyi [oluşturur](/dotnet/core/tools/dotnet-build) .</span><span class="sxs-lookup"><span data-stu-id="c0f50-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="c0f50-116">Yapı işleminin bir parçası olarak, uygulama indirme boyutunu ve yükleme sürelerini azaltmak için kullanılmayan Yöntemler ve derlemeler kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="c0f50-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="c0f50-117">Blazor istemci tarafı uygulaması */BIN/Release/{Target Framework}/publish/{ASSEMBLY Name}/Dist* klasörüne yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="c0f50-117">A Blazor client-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span> <span data-ttu-id="c0f50-118">Blazor sunucu tarafı bir uygulama */BIN/Release/{Target Framework}/Publish* klasörüne yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="c0f50-118">A Blazor server-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="c0f50-119">Klasördeki varlıklar Web sunucusuna dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="c0f50-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="c0f50-120">Dağıtım, kullanımdaki geliştirme araçlarına bağlı olarak el ile veya otomatik bir süreç olabilir.</span><span class="sxs-lookup"><span data-stu-id="c0f50-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="c0f50-121">Dağıtım</span><span class="sxs-lookup"><span data-stu-id="c0f50-121">Deployment</span></span>

<span data-ttu-id="c0f50-122">Dağıtım Kılavuzu için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="c0f50-122">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a><span data-ttu-id="c0f50-123">Azure depolama ile Blazor sunucusuz barındırma</span><span class="sxs-lookup"><span data-stu-id="c0f50-123">Blazor serverless hosting with Azure Storage</span></span>

<span data-ttu-id="c0f50-124">Blazor istemci tarafı uygulamalar, doğrudan bir depolama kapsayıcısından statik içerik olarak [Azure depolama](https://azure.microsoft.com/services/storage/) 'dan sunulabilir.</span><span class="sxs-lookup"><span data-stu-id="c0f50-124">Blazor client-side apps can be served from [Azure Storage](https://azure.microsoft.com/services/storage/) as static content directly from a storage container.</span></span>

<span data-ttu-id="c0f50-125">Daha fazla bilgi için bkz [. konak ve dağıtım ASP.NET Core Blazor istemci tarafı (tek başına dağıtım): Azure depolama](xref:host-and-deploy/blazor/client-side#azure-storage).</span><span class="sxs-lookup"><span data-stu-id="c0f50-125">For more information, see [Host and deploy ASP.NET Core Blazor client-side (Standalone deployment): Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage).</span></span>
