---
title: ASP.NET Core Razor sayfaları Visual Studio ile macOS üzerinde Mac için kullanmaya başlama
author: rick-anderson
description: Mac için Visual Studio kullanarak ASP.NET Core Razor sayfaları kullanmaya başlama öğrenin
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 0e1c2a9ab436968e2c24aa5f306ec69674fb025b
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523278"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a><span data-ttu-id="504a2-103">ASP.NET Core Razor sayfaları Visual Studio ile macOS üzerinde Mac için kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="504a2-103">Get started with Razor Pages in ASP.NET Core on macOS with Visual Studio for Mac</span></span>

<span data-ttu-id="504a2-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="504a2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="504a2-105">Bu öğreticide bir ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="504a2-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="504a2-106">Gözden geçirmenizi öneririz [Razor sayfaları giriş](xref:razor-pages/index) bu öğreticiye başlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="504a2-106">We recommend you review [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="504a2-107">Razor sayfaları, ASP.NET Core web uygulamaları için kullanıcı arabirimini derlemek için önerilen yoludur.</span><span class="sxs-lookup"><span data-stu-id="504a2-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="504a2-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="504a2-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="504a2-109">Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="504a2-109">Create a Razor web app</span></span>

<span data-ttu-id="504a2-110">Bir terminalde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="504a2-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="504a2-111">Kullanım komutları önceki [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) oluşturup bir Razor sayfaları projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="504a2-111">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="504a2-112">Bir tarayıcıda http://localhost:5000 uygulamayı görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="504a2-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Giriş ya da dizin sayfası](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="504a2-114">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="504a2-114">Open the project</span></span>

<span data-ttu-id="504a2-115">Uygulamayı kapatmak için CTRL + C tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="504a2-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="504a2-116">Visual Studio'dan seçin **Dosya > Aç**ve ardından *RazorPagesMovie.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="504a2-116">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="504a2-117">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="504a2-117">Launch the app</span></span>

<span data-ttu-id="504a2-118">Visual Studio'da **çalıştırın > hata ayıklama olmadan Başlat** uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="504a2-118">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="504a2-119">Visual Studio başlatır [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="504a2-119">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="504a2-120">Sonraki öğreticide, projeye bir model ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="504a2-120">In the next tutorial, we add a model to the project.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="504a2-121">Sonraki: model ekleme</span><span class="sxs-lookup"><span data-stu-id="504a2-121">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
