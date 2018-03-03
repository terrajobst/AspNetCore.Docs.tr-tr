---
title: "ASP.NET Core Razor sayfalarında, Visual Studio Code ile çalışmaya başlama"
author: rick-anderson
description: "Visual Studio Code ile bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temellerini öğrenin."
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: f65395f2b7b4f37bb5e191de4a2669ac6d26461f
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="getting-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="8229a-103">ASP.NET Core Razor sayfalarında, Visual Studio Code ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="8229a-103">Getting started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="8229a-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8229a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8229a-105">Bu öğretici, bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temel öğretir.</span><span class="sxs-lookup"><span data-stu-id="8229a-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="8229a-106">Tamamlamanız önerilir [Razor sayfalarının giriş](xref:mvc/razor-pages/index) bu öğreticiye başlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="8229a-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="8229a-107">Razor sayfalarının ASP.NET Core web uygulamaları için kullanıcı Arabirimi oluşturmak için önerilen yöntem olduğu.</span><span class="sxs-lookup"><span data-stu-id="8229a-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8229a-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="8229a-108">Prerequisites</span></span>

<span data-ttu-id="8229a-109">Aşağıdaki yükleyin:</span><span class="sxs-lookup"><span data-stu-id="8229a-109">Install the following:</span></span>

* <span data-ttu-id="8229a-110">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya daha yenisi</span><span class="sxs-lookup"><span data-stu-id="8229a-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="8229a-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8229a-111">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="8229a-112">VS Code [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="8229a-112">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="8229a-113">Bir Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="8229a-113">Create a Razor web app</span></span>

<span data-ttu-id="8229a-114">Terminal durumundan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="8229a-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="8229a-115">Yukarıdaki komutları kullanım [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) oluşturun ve Razor sayfalarının proje çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8229a-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="8229a-116">Http://localhost: 5000 uygulamayı görüntülemek için bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="8229a-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![Giriş veya dizin sayfası](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="8229a-118">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="8229a-118">Open the project</span></span>

<span data-ttu-id="8229a-119">Uygulamayı kapatın CTRL + C tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="8229a-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="8229a-120">Visual Studio kodundan (VS Code) seçin **Dosya > Klasör Aç**ve ardından *RazorPagesMovie* klasör.</span><span class="sxs-lookup"><span data-stu-id="8229a-120">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="8229a-121">Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'RazorPagesMovie' eksik. ileti</span><span class="sxs-lookup"><span data-stu-id="8229a-121">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="8229a-122">Bunları eklensin mi?"</span><span class="sxs-lookup"><span data-stu-id="8229a-122">Add them?"</span></span>
- <span data-ttu-id="8229a-123">Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.</span><span class="sxs-lookup"><span data-stu-id="8229a-123">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="8229a-124">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="8229a-124">Launch the app</span></span>

<span data-ttu-id="8229a-125">Hata ayıklama olmadan uygulamayı başlatmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="8229a-125">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="8229a-126">Alternatif olarak, gelen **hata ayıklama** menüsünde, select **hata ayıklama olmadan Başlat**.</span><span class="sxs-lookup"><span data-stu-id="8229a-126">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="8229a-127">Sonraki öğreticide biz projenize bir model ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8229a-127">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="8229a-128">Sonraki: bir modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="8229a-128">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
