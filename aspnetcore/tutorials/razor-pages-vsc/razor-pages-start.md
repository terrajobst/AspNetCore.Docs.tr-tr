---
title: Visual Studio code'da ASP.NET Core Razor sayfaları kullanmaya başlama
author: rick-anderson
description: Bir Visual Studio Code ile ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 9ea66134c524a6a1a670d55bae4e66cf38a45274
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089858"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="92822-103">Visual Studio code'da ASP.NET Core Razor sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="92822-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="92822-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="92822-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="92822-105">Bu öğreticide bir ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="92822-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="92822-106">Tamamlamanız önerilir [Razor sayfaları giriş](xref:razor-pages/index) bu öğreticiye başlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="92822-106">We recommend you complete [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="92822-107">Razor sayfaları, ASP.NET Core web uygulamaları için kullanıcı arabirimini derlemek için önerilen yoludur.</span><span class="sxs-lookup"><span data-stu-id="92822-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="92822-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="92822-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="92822-109">Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="92822-109">Create a Razor web app</span></span>

<span data-ttu-id="92822-110">Bir terminalde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="92822-110">From a terminal, run the following commands:</span></span>

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

<span data-ttu-id="92822-111">Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) oluşturup bir Razor sayfaları projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="92822-111">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="92822-112">Bir tarayıcıda http://localhost:5000 uygulamayı görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="92822-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Giriş ya da dizin sayfası](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="92822-114">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="92822-114">Open the project</span></span>

<span data-ttu-id="92822-115">Uygulamayı kapatmak için CTRL + C tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="92822-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="92822-116">Visual Studio Code'dan (VS Code), seçin **Dosya > Klasör Aç**ve ardından *RazorPagesMovie* klasör.</span><span class="sxs-lookup"><span data-stu-id="92822-116">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="92822-117">Seçin **Evet** için **uyar** ileti "derleme ve hata ayıklamak için gerekli varlıkları 'RazorPagesMovie' eksik.</span><span class="sxs-lookup"><span data-stu-id="92822-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="92822-118">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="92822-118">Add them?"</span></span>
- <span data-ttu-id="92822-119">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıklar vardır" iletisi.</span><span class="sxs-lookup"><span data-stu-id="92822-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="92822-120">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="92822-120">Launch the app</span></span>

<span data-ttu-id="92822-121">Hata ayıklama olmadan uygulamayı başlatmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="92822-121">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="92822-122">Alternatif olarak, gelen **hata ayıklama** menüsünde **hata ayıklama olmadan Başlat**.</span><span class="sxs-lookup"><span data-stu-id="92822-122">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="92822-123">Sonraki öğreticide, projeye bir model ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="92822-123">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="92822-124">Sonraki: model ekleme</span><span class="sxs-lookup"><span data-stu-id="92822-124">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
