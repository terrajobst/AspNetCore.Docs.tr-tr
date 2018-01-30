---
title: "ASP.NET Core Razor sayfalarında ile çalışmaya başlama"
author: rick-anderson
description: "ASP.NET Core Razor sayfalarında ile çalışmaya başlama"
manager: wpickett
ms.author: riande
ms.date: 12/22/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 54fa970d136de3903ae08b710b55f15f96f9a012
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="3b1cf-103">ASP.NET Core Razor sayfalarında kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="3b1cf-103">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="3b1cf-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3b1cf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3b1cf-105">Bu öğretici, bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temel öğretir.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="3b1cf-106">Razor sayfalarının ASP.NET Core web uygulamaları için kullanıcı Arabirimi oluşturmak için önerilen yöntem olduğu.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-106">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="3b1cf-107">Bu öğretici için üç sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="3b1cf-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="3b1cf-108">Windows: Bu öğretici</span><span class="sxs-lookup"><span data-stu-id="3b1cf-108">Windows: This tutorial</span></span>
* <span data-ttu-id="3b1cf-109">MacOS: [Mac için Visual Studio ile Razor sayfaları ile Başlarken](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="3b1cf-109">MacOS: [Getting started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="3b1cf-110">macOS, Linux ve Windows: [Visual Studio Code ile ASP.NET Core Razor sayfalarında ile çalışmaya başlama](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="3b1cf-110">macOS, Linux, and Windows: [Getting started with Razor Pages in ASP.NET Core with Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="3b1cf-111">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3b1cf-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b1cf-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="3b1cf-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="3b1cf-113">Bir Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="3b1cf-113">Create a Razor web app</span></span>

* <span data-ttu-id="3b1cf-114">Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="3b1cf-115">Yeni bir ASP.NET çekirdek Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-115">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="3b1cf-116">Proje adı **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-116">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="3b1cf-117">Proje adı önemlidir *RazorPagesMovie* ad alanları, kopyala/yapıştır kod zaman eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-117">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="3b1cf-118">![Yeni ASP.NET çekirdek Web uygulaması](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="3b1cf-118">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="3b1cf-119">Seçin **ASP.NET Core 2.0** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-119">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE[install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="3b1cf-120">Visual Studio şablon bir başlangıç projesi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="3b1cf-120">The Visual Studio template creates a starter project:</span></span>

![Çözüm Gezgini](razor-pages-start/_static/se.png)

<span data-ttu-id="3b1cf-122">Tuşuna **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** hata ayıklayıcı eklemeden çalıştırmak için</span><span class="sxs-lookup"><span data-stu-id="3b1cf-122">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Giriş veya dizin sayfası](razor-pages-start/_static/home.png)

* <span data-ttu-id="3b1cf-124">Visual Studio başlatır [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-124">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="3b1cf-125">Adres çubuğunu gösterir `localhost:port#` bir şey yok gibi ve `example.com`.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-125">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="3b1cf-126">Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-126">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="3b1cf-127">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-127">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="3b1cf-128">Visual Studio web projesini oluşturduğunda, rastgele bir bağlantı noktası web sunucusu için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-128">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="3b1cf-129">Önceki görüntüde 5000 bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-129">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="3b1cf-130">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-130">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="3b1cf-131">Uygulama başlatma **Ctrl + F5** (olmayan hata ayıklama modu), kod değişiklikleri yapabilir, dosyayı kaydedin, tarayıcıyı yenilemek ve kod değişiklikleri görmek olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-131">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="3b1cf-132">Çoğu geliştirici, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için olmayan hata ayıklama modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="3b1cf-132">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="3b1cf-133">Sonraki: bir modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="3b1cf-133">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)

>[!div class="step-by-step"]
[<span data-ttu-id="3b1cf-134">Sonraki: bir modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="3b1cf-134">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
