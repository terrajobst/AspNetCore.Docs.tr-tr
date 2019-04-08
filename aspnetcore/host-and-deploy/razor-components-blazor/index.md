---
title: Barındırma ve Razor bileşenleri ve Blazor dağıtma
author: guardrex
description: Ana bilgisayar ve Razor bileşenleri ve Blazor uygulamaları dağıtmak nasıl keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/index
ms.openlocfilehash: eeaa27bef584523b77d8b47000b310fe0cac7341
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59069779"
---
# <a name="host-and-deploy-razor-components-and-blazor"></a><span data-ttu-id="f1145-103">Barındırma ve Razor bileşenleri ve Blazor dağıtma</span><span class="sxs-lookup"><span data-stu-id="f1145-103">Host and deploy Razor Components and Blazor</span></span>

<span data-ttu-id="f1145-104">Tarafından [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), ve [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f1145-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="f1145-105">Uygulamayı yayımlama</span><span class="sxs-lookup"><span data-stu-id="f1145-105">Publish the app</span></span>

<span data-ttu-id="f1145-106">Uygulamalar ile yayın Yapılandırması'nda dağıtım için yayımlanır [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu.</span><span class="sxs-lookup"><span data-stu-id="f1145-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="f1145-107">Yürütülen bir tümleşik geliştirme ortamı (IDE) işleyebilir `dotnet publish` komutunu otomatik olarak yerleşik yayımlama özelliklerini kullanarak, bunu el ile gerekli olmayabilir bu nedenle geliştirme bağlı olarak bir komut isteminden komutu yürütün Araçlar kullanılıyor.</span><span class="sxs-lookup"><span data-stu-id="f1145-107">An integrated development environment (IDE) may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

`dotnet publish` <span data-ttu-id="f1145-108">Tetikleyiciler bir [geri](/dotnet/core/tools/dotnet-restore) proje bağımlılıklarının ve [yapılar](/dotnet/core/tools/dotnet-build) dağıtım için bir varlık oluşturmadan önce proje.</span><span class="sxs-lookup"><span data-stu-id="f1145-108">triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="f1145-109">Yapı işleminin bir parçası olarak, kullanılmayan yöntemleri ve derlemeleri, uygulama indirme boyutunu küçültmek ve yükleme süreleri için kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="f1145-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="f1145-110">Dağıtım oluşturulan */bin/yayın / {hedef çerçeve} / publish* klasör.</span><span class="sxs-lookup"><span data-stu-id="f1145-110">The deployment is created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="f1145-111">Varlıkları *yayımlama* klasör, web sunucusuna dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="f1145-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="f1145-112">Dağıtım işlemini kullanımda geliştirme araçları bağlı olarak otomatik veya el ile olabilir.</span><span class="sxs-lookup"><span data-stu-id="f1145-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="f1145-113">Dağıtım</span><span class="sxs-lookup"><span data-stu-id="f1145-113">Deployment</span></span>

<span data-ttu-id="f1145-114">Dağıtım yönergeleri için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="f1145-114">For deployment guidance, see the following topics:</span></span>

* [<span data-ttu-id="f1145-115">İstemci tarafı Blazor</span><span class="sxs-lookup"><span data-stu-id="f1145-115">Client-side Blazor</span></span>](xref:host-and-deploy/razor-components-blazor/blazor)
* [<span data-ttu-id="f1145-116">Sunucu tarafı ASP.NET Core Razor bileşenleri</span><span class="sxs-lookup"><span data-stu-id="f1145-116">Server-side ASP.NET Core Razor Components</span></span>](xref:host-and-deploy/razor-components-blazor/razor-components)
