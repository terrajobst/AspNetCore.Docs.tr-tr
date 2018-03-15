---
title: "Mac için ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama"
author: rick-anderson
description: "ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama öğrenin"
manager: wpickett
ms.author: riande
ms.date: 8/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 7d2729969a65cf2050d0eac390169898a4102de1
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="e6efd-103">Mac için ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="e6efd-103">Get started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="e6efd-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e6efd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e6efd-105">Bu öğretici bir ASP.NET Core MVC web app kullanarak oluşturma temelleri öğretilmektedir [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="e6efd-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="e6efd-106">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="e6efd-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="e6efd-107">macOS: [Mac için Visual Studio ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e6efd-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="e6efd-108">Windows: [Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e6efd-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="e6efd-109">MacOS, Linux ve Windows: [Visual Studio Code ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e6efd-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6efd-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="e6efd-110">Prerequisites</span></span>

<span data-ttu-id="e6efd-111">Bu öğretici gerektirir [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="e6efd-111">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

<span data-ttu-id="e6efd-112">Aşağıdaki yükleyin:</span><span class="sxs-lookup"><span data-stu-id="e6efd-112">Install the following:</span></span>

- <span data-ttu-id="e6efd-113">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya daha yenisi</span><span class="sxs-lookup"><span data-stu-id="e6efd-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="e6efd-114">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e6efd-114">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="e6efd-115">Bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="e6efd-115">Create a web app</span></span>

<span data-ttu-id="e6efd-116">Visual Studio'dan seçin **Dosya > Yeni bir çözüm**.</span><span class="sxs-lookup"><span data-stu-id="e6efd-116">From Visual Studio, select **File > New Solution**.</span></span>

![macOS yeni çözüm](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="e6efd-118">Seçin **.NET Core uygulaması > ASP.NET Core > Web uygulaması > sonraki**.</span><span class="sxs-lookup"><span data-stu-id="e6efd-118">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![macOS yeni proje iletişim kutusu](start-mvc/1.png)

<span data-ttu-id="e6efd-120">Proje adı **MvcMovie**ve ardından **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="e6efd-120">Name the project **MvcMovie**, and then select **Create**.</span></span>

![macOS yeni proje iletişim kutusu](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="e6efd-122">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="e6efd-122">Launch the app</span></span>

<span data-ttu-id="e6efd-123">Visual Studio'da seçin **Çalıştır > hata ayıklama olmadan Başlat** uygulamayı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="e6efd-123">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="e6efd-124">Visual Studio başlatır [Kestrel](xref:fundamentals/servers/index#kestrel), bir tarayıcı başlatılır ve gider `http://localhost:port`, burada *bağlantı noktası* rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="e6efd-124">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Yeni Proje tarayıcıyla](start-mvc/b1.png)

* <span data-ttu-id="e6efd-126">Adres çubuğunu gösterir `localhost:port#` bir şey yok gibi ve `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e6efd-126">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e6efd-127">Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="e6efd-127">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e6efd-128">Visual Studio web projesini oluşturduğunda, rastgele bir bağlantı noktası web sunucusu için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e6efd-128">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e6efd-129">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="e6efd-129">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e6efd-130">Hata ayıklama veya hata ayıklama olmayan modundan uygulamada başlatabilirsiniz **çalıştırmak** menüsü.</span><span class="sxs-lookup"><span data-stu-id="e6efd-130">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="e6efd-131">Varsayılan şablonu verir **hakkında giriş** ve **kişi** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="e6efd-131">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="e6efd-132">Yukarıdaki tarayıcı resimde bu bağlantıları göstermez.</span><span class="sxs-lookup"><span data-stu-id="e6efd-132">The browser image above doesn't show these links.</span></span> <span data-ttu-id="e6efd-133">Tarayıcınız boyutuna bağlı olarak, bunları göstermek için Gezinti simgesini gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="e6efd-133">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Yeni Proje tarayıcıyla](start-mvc/b2.png)

<span data-ttu-id="e6efd-135">Bu öğreticinin sonraki bölümünde, MVC konusunda bilgi edinmek ve bazı kod yazmayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="e6efd-135">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e6efd-136">Next</span><span class="sxs-lookup"><span data-stu-id="e6efd-136">Next</span></span>](adding-controller.md)  
