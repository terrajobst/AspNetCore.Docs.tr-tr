---
title: Barındırma ve Blazor dağıtma
author: guardrex
description: Ana bilgisayar ve Blazor uygulamaları dağıtmak nasıl keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/23/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 5def0356d13975211dd234f6a6a9f5a993d003b7
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223175"
---
# <a name="host-and-deploy-blazor"></a><span data-ttu-id="f59d8-103">Barındırma ve Blazor dağıtma</span><span class="sxs-lookup"><span data-stu-id="f59d8-103">Host and deploy Blazor</span></span>

<span data-ttu-id="f59d8-104">Tarafından [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), ve [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f59d8-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="f59d8-105">Uygulamayı yayımlama</span><span class="sxs-lookup"><span data-stu-id="f59d8-105">Publish the app</span></span>

<span data-ttu-id="f59d8-106">Uygulamalar, yayın Yapılandırması'nda dağıtım için yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="f59d8-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f59d8-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f59d8-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f59d8-108">Seçin **derleme** > **yayımlama {uygulama}** gezinti çubuğundan.</span><span class="sxs-lookup"><span data-stu-id="f59d8-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="f59d8-109">Seçin *hedef yayımlama*.</span><span class="sxs-lookup"><span data-stu-id="f59d8-109">Select the *publish target*.</span></span> <span data-ttu-id="f59d8-110">Yerel olarak yayımlamak için seçin **klasör**.</span><span class="sxs-lookup"><span data-stu-id="f59d8-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="f59d8-111">Varsayılan konumu kabul **bir klasör seçin** alanına veya farklı bir konum belirtin.</span><span class="sxs-lookup"><span data-stu-id="f59d8-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="f59d8-112">**Yayımla** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="f59d8-112">Select the **Publish** button.</span></span>


# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="f59d8-113">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f59d8-113">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="f59d8-114">Kullanım [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu bir sürüm yapılandırması ile uygulama yayımlama:</span><span class="sxs-lookup"><span data-stu-id="f59d8-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```console
dotnet publish -c Release
```

---

<span data-ttu-id="f59d8-115">Uygulama Tetikleyicileri yayımlama bir [geri](/dotnet/core/tools/dotnet-restore) proje bağımlılıklarının ve [yapılar](/dotnet/core/tools/dotnet-build) dağıtım için bir varlık oluşturmadan önce proje.</span><span class="sxs-lookup"><span data-stu-id="f59d8-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="f59d8-116">Yapı işleminin bir parçası olarak, kullanılmayan yöntemleri ve derlemeleri, uygulama indirme boyutunu küçültmek ve yükleme süreleri için kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="f59d8-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="f59d8-117">Bir Blazor istemci-tarafı uygulaması yayımlanan */bin/yayın / {hedef çerçeve} / dist* klasör.</span><span class="sxs-lookup"><span data-stu-id="f59d8-117">A Blazor client-side app is published to the */bin/Release/{TARGET FRAMEWORK}/dist* folder.</span></span> <span data-ttu-id="f59d8-118">İçin yayımlanan bir Blazor sunucu tarafı uygulamayı */bin/yayın / {hedef çerçeve} / publish* klasör.</span><span class="sxs-lookup"><span data-stu-id="f59d8-118">A Blazor server-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="f59d8-119">Varlıkları klasöründe, web sunucusuna dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="f59d8-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="f59d8-120">Dağıtım işlemini kullanımda geliştirme araçları bağlı olarak otomatik veya el ile olabilir.</span><span class="sxs-lookup"><span data-stu-id="f59d8-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="f59d8-121">Dağıtım</span><span class="sxs-lookup"><span data-stu-id="f59d8-121">Deployment</span></span>

<span data-ttu-id="f59d8-122">Dağıtım yönergeleri için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="f59d8-122">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a><span data-ttu-id="f59d8-123">Azure Depolama'yı barındıran sunucusuz Blazor</span><span class="sxs-lookup"><span data-stu-id="f59d8-123">Blazor serverless hosting with Azure Storage</span></span>

<span data-ttu-id="f59d8-124">Gelen istemci tarafı uygulamalar Blazor sunulduğundan [Azure depolama](https://azure.microsoft.com/services/storage/) statik içeriği doğrudan bir depolama kapsayıcısı olarak.</span><span class="sxs-lookup"><span data-stu-id="f59d8-124">Blazor client-side apps can be served from [Azure Storage](https://azure.microsoft.com/services/storage/) as static content directly from a storage container.</span></span>

<span data-ttu-id="f59d8-125">Daha fazla bilgi için [konak ve Blazor istemci-tarafı (tek başına dağıtımı) dağıtın: Azure depolama](xref:host-and-deploy/blazor/client-side#azure-storage).</span><span class="sxs-lookup"><span data-stu-id="f59d8-125">For more information, see [Host and deploy Blazor client-side (Standalone deployment): Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage).</span></span>
