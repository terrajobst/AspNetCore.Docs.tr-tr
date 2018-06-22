---
title: Mac için ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama
author: rick-anderson
description: ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama öğrenin
ms.author: riande
ms.date: 8/23/2017
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: e94b9aa6b6c594ae407792387788410f776d4c1d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272299"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="16ec2-103">Mac için ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="16ec2-103">Get started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="16ec2-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="16ec2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="16ec2-105">Bu öğretici bir ASP.NET Core MVC web app kullanarak oluşturma temelleri öğretilmektedir [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="16ec2-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="16ec2-106">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="16ec2-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="16ec2-107">macOS: [Mac için Visual Studio ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="16ec2-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="16ec2-108">Windows: [Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="16ec2-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="16ec2-109">MacOS, Linux ve Windows: [Visual Studio Code ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="16ec2-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16ec2-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="16ec2-110">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="16ec2-111">Bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="16ec2-111">Create a web app</span></span>

<span data-ttu-id="16ec2-112">Visual Studio'dan seçin **Dosya > Yeni bir çözüm**.</span><span class="sxs-lookup"><span data-stu-id="16ec2-112">From Visual Studio, select **File > New Solution**.</span></span>

![macOS yeni çözüm](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="16ec2-114">Seçin **.NET Core uygulaması > ASP.NET Core > Web uygulaması > sonraki**.</span><span class="sxs-lookup"><span data-stu-id="16ec2-114">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![macOS yeni proje iletişim kutusu](start-mvc/1.png)

<span data-ttu-id="16ec2-116">Proje adı **MvcMovie**ve ardından **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="16ec2-116">Name the project **MvcMovie**, and then select **Create**.</span></span>

![macOS yeni proje iletişim kutusu](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="16ec2-118">Uygulamayı başlatın</span><span class="sxs-lookup"><span data-stu-id="16ec2-118">Launch the app</span></span>

<span data-ttu-id="16ec2-119">Visual Studio'da seçin **Çalıştır > hata ayıklama olmadan Başlat** uygulamayı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="16ec2-119">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="16ec2-120">Visual Studio başlatır [Kestrel](xref:fundamentals/servers/index#kestrel), bir tarayıcı başlatılır ve gider `http://localhost:port`, burada *bağlantı noktası* rastgele seçilen bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="16ec2-120">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Yeni Proje tarayıcıyla](start-mvc/b1.png)

* <span data-ttu-id="16ec2-122">Adres çubuğunu gösterir `localhost:port#` bir şey yok gibi ve `example.com`.</span><span class="sxs-lookup"><span data-stu-id="16ec2-122">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="16ec2-123">Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="16ec2-123">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="16ec2-124">Visual Studio web projesini oluşturduğunda, rastgele bir bağlantı noktası web sunucusu için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="16ec2-124">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="16ec2-125">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="16ec2-125">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="16ec2-126">Hata ayıklama veya hata ayıklama olmayan modundan uygulamada başlatabilirsiniz **çalıştırmak** menüsü.</span><span class="sxs-lookup"><span data-stu-id="16ec2-126">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="16ec2-127">Varsayılan şablonu verir **hakkında giriş** ve **kişi** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="16ec2-127">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="16ec2-128">Yukarıdaki tarayıcı resimde bu bağlantıları göstermez.</span><span class="sxs-lookup"><span data-stu-id="16ec2-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="16ec2-129">Tarayıcınız boyutuna bağlı olarak, bunları göstermek için Gezinti simgesini gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="16ec2-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Yeni Proje tarayıcıyla](start-mvc/b2.png)

<span data-ttu-id="16ec2-131">Bu öğreticinin sonraki bölümünde, MVC konusunda bilgi edinmek ve bazı kod yazmayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="16ec2-131">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="16ec2-132">Next</span><span class="sxs-lookup"><span data-stu-id="16ec2-132">Next</span></span>](adding-controller.md)  
