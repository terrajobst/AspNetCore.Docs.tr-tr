---
title: "ASP.NET Core Mac üzerinde Razor sayfalarında ile çalışmaya başlama"
author: rick-anderson
description: "Mac için Visual Studio kullanarak ASP.NET Core Razor sayfalarında ile çalışmaya başlama"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: ca9fca507096f3f09f19fe0a7f1dc023003649d7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="edf14-103">Mac için Visual Studio ile ASP.NET Core Razor sayfalarında ile Başlarken</span><span class="sxs-lookup"><span data-stu-id="edf14-103">Getting started with Razor Pages in ASP.NET Core with Visual Studio for Mac</span></span>

<span data-ttu-id="edf14-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="edf14-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="edf14-105">Bu öğretici, bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temel öğretir.</span><span class="sxs-lookup"><span data-stu-id="edf14-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="edf14-106">Tamamlamanız önerilir [Razor sayfalarının giriş](xref:mvc/razor-pages/index) bu öğreticiye başlamadan önce.</span><span class="sxs-lookup"><span data-stu-id="edf14-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="edf14-107">Razor sayfalarının ASP.NET Core web uygulamaları için kullanıcı Arabirimi oluşturmak için önerilen yöntem olduğu.</span><span class="sxs-lookup"><span data-stu-id="edf14-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="edf14-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="edf14-108">Prerequisites</span></span>

<span data-ttu-id="edf14-109">Aşağıdaki yükleyin:</span><span class="sxs-lookup"><span data-stu-id="edf14-109">Install the following:</span></span>

* <span data-ttu-id="edf14-110">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya daha yenisi</span><span class="sxs-lookup"><span data-stu-id="edf14-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="edf14-111">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="edf14-111">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a><span data-ttu-id="edf14-112">Bir Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="edf14-112">Create a Razor web app</span></span>

<span data-ttu-id="edf14-113">Terminal durumundan, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="edf14-113">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="edf14-114">Yukarıdaki komutları kullanım [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) oluşturun ve Razor sayfalarının proje çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="edf14-114">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="edf14-115">Http://localhost: 5000 uygulamayı görüntülemek için bir tarayıcı açın.</span><span class="sxs-lookup"><span data-stu-id="edf14-115">Open a browser to http://localhost:5000 to view the application.</span></span>

![Giriş veya dizin sayfası](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="edf14-117">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="edf14-117">Open the project</span></span>

<span data-ttu-id="edf14-118">Uygulamayı kapatın CTRL + C tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="edf14-118">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="edf14-119">Visual Studio'dan seçin **Dosya > Aç**ve ardından *RazorPagesMovie.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="edf14-119">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="edf14-120">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="edf14-120">Launch the app</span></span>

<span data-ttu-id="edf14-121">Visual Studio'da seçin **Çalıştır > hata ayıklama olmadan Başlat** uygulamayı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="edf14-121">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="edf14-122">Visual Studio başlatır [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), bir tarayıcı başlatılır ve gider `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="edf14-122">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="edf14-123">Sonraki öğreticide biz projenize bir model ekleyin.</span><span class="sxs-lookup"><span data-stu-id="edf14-123">In the next tutorial, we add a model to the project.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="edf14-124">Sonraki: bir modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="edf14-124">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
