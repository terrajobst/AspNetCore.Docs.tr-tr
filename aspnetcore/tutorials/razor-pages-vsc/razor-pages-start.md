---
title: "Visual Studio Code ile ASP.NET Core Razor sayfalarında ile çalışmaya başlama"
author: rick-anderson
description: "Razor sayfalarında ASP.NET Core kullanarak Visual Studio Code ile çalışmaya başlama"
keywords: "ASP.NET, iskele kurma Entity Framework Çekirdek, EF, EF çekirdek, veritabanı, mac, macOS çekirdek, Razor sayfalarının, Visual Studio Code, kod"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 1b9dff14fa98314601fa44aa229aef6b73bb79d0
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="06818-104">Visual Studio Code ile ASP.NET Core Razor sayfalarında ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="06818-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="06818-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="06818-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="06818-106">Bu öğretici, bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temel öğretir.</span><span class="sxs-lookup"><span data-stu-id="06818-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="06818-107">Tamamlamanız önerilir [Razor sayfalarının giriş](xref:mvc/razor-pages/index) bu öğreticiye başlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="06818-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="06818-108">Razor sayfalarının ASP.NET Core web uygulamaları için kullanıcı Arabirimi oluşturmak için önerilen yöntem olduğu.</span><span class="sxs-lookup"><span data-stu-id="06818-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="06818-109">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="06818-109">Prerequisites</span></span>

<span data-ttu-id="06818-110">Aşağıdaki yükleyin:</span><span class="sxs-lookup"><span data-stu-id="06818-110">Install the following:</span></span>

* <span data-ttu-id="06818-111">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya daha yenisi</span><span class="sxs-lookup"><span data-stu-id="06818-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="06818-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="06818-112">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="06818-113">VS Code [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="06818-113">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="06818-114">Bir Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="06818-114">Create a Razor web app</span></span>

<span data-ttu-id="06818-115">Terminal durumundan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="06818-115">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="06818-116">Yukarıdaki komutları kullanım [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) oluşturun ve Razor sayfalarının proje çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="06818-116">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="06818-117">Http://localhost: 5000 uygulamayı görüntülemek için bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="06818-117">Open a browser to http://localhost:5000 to view the application.</span></span>

![Giriş veya dizin sayfası](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="06818-119">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="06818-119">Open the project</span></span>

<span data-ttu-id="06818-120">Uygulamayı kapatın CTRL + C tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="06818-120">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="06818-121">Visual Studio kodundan (VS Code) seçin **Dosya > Klasör Aç**ve ardından *RazorPagesMovie* klasör.</span><span class="sxs-lookup"><span data-stu-id="06818-121">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="06818-122">Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'RazorPagesMovie' eksik. ileti</span><span class="sxs-lookup"><span data-stu-id="06818-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="06818-123">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="06818-123">Add them?"</span></span>
- <span data-ttu-id="06818-124">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.</span><span class="sxs-lookup"><span data-stu-id="06818-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="06818-125">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="06818-125">Launch the app</span></span>

<span data-ttu-id="06818-126">Hata ayıklama olmadan uygulamayı başlatmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="06818-126">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="06818-127">Alternatif olarak, gelen **hata ayıklama** menüsünde, select **hata ayıklama olmadan Başlat**.</span><span class="sxs-lookup"><span data-stu-id="06818-127">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="06818-128">Sonraki öğreticide biz projenize bir model ekleyin.</span><span class="sxs-lookup"><span data-stu-id="06818-128">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="06818-129">Sonraki: bir modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="06818-129">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
