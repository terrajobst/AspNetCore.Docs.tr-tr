---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: ASP.NET MVC 3 (C#) giriş | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 9b965df6175051a084de35627160161c116be42d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="1a3eb-103">Giriş ASP.NET MVC 3 (C#)</span><span class="sxs-lookup"><span data-stu-id="1a3eb-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="1a3eb-104">tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="1a3eb-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="1a3eb-105">Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="1a3eb-106">Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="1a3eb-107">Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="1a3eb-108">Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="1a3eb-109">Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="1a3eb-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="1a3eb-110">Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1a3eb-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="1a3eb-111">Visual Studio Web Developer Express SP1 önkoşulları</span><span class="sxs-lookup"><span data-stu-id="1a3eb-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="1a3eb-112">ASP.NET MVC 3 araçları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="1a3eb-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="1a3eb-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)</span><span class="sxs-lookup"><span data-stu-id="1a3eb-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="1a3eb-114">Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="1a3eb-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="1a3eb-115">C# kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="1a3eb-116">[C# sürümü](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="1a3eb-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="1a3eb-117">Visual Basic tercih ederseniz, geçiş [Visual Basic sürüm](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="1a3eb-118">Ne oluşturacağınız</span><span class="sxs-lookup"><span data-stu-id="1a3eb-118">What You'll Build</span></span>

<span data-ttu-id="1a3eb-119">Oluşturma, düzenleme ve veritabanından filmler listeleme destekleyen basit bir film listesi uygulaması uygulamanız.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="1a3eb-120">Aşağıda oluşturacağınız uygulamasının iki ekran görüntüleri verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="1a3eb-121">Veritabanından filmler listesini görüntüleyen bir sayfa içerir:</span><span class="sxs-lookup"><span data-stu-id="1a3eb-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="1a3eb-123">Uygulaması, ekleme, düzenleme ve tek tek olanları hakkında ayrıntılara bakın yanı sıra, filmler silme sağlar.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="1a3eb-124">Tüm veri girişi senaryolar veritabanında depolanan verileri doğru olduğundan emin olmak için doğrulama içerir.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="1a3eb-125">Bilgi edineceksiniz yetenekleri</span><span class="sxs-lookup"><span data-stu-id="1a3eb-125">Skills You'll Learn</span></span>

<span data-ttu-id="1a3eb-126">Öğrenecekleriniz aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="1a3eb-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="1a3eb-127">Yeni bir ASP.NET MVC projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="1a3eb-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="1a3eb-128">ASP.NET MVC denetleyicileri ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="1a3eb-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="1a3eb-129">Entity Framework Code First modeli kullanarak yeni bir veritabanı oluşturmak nasıl.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="1a3eb-130">Nasıl alınacağını ve verileri görüntüle.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="1a3eb-131">Verileri düzenlemek ve veri doğrulamasını etkinleştirmek nasıl.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="1a3eb-132">Başlarken</span><span class="sxs-lookup"><span data-stu-id="1a3eb-132">Getting Started</span></span>

<span data-ttu-id="1a3eb-133">Visual Web Developer 2010 Express ("Visual Web Developer" kısaca) çalıştırarak ve seçin **yeni proje** gelen **Başlat** sayfası.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="1a3eb-134">Visual Web Developer bir IDE veya tümleşik geliştirme ortamını değil.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="1a3eb-135">Belgeleri yazmak için Microsoft Word kullanma gibi uygulamaları oluşturmak için bir IDE kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="1a3eb-136">Visual Web Developer ile bir araç çubuğu size çeşitli seçenekleri gösteren üstünde yoktur.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="1a3eb-137">IDE içinde görevleri gerçekleştirmek için başka bir yol sağlayan bir menüsünü yoktur.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="1a3eb-138">(Örneğin, seçmek yerine **yeni proje** gelen **Başlat** sayfasında, menüsünü kullanın ve seçin **dosya** &gt; **yeni proje**.)</span><span class="sxs-lookup"><span data-stu-id="1a3eb-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="1a3eb-139">İlk uygulamanızı oluşturma</span><span class="sxs-lookup"><span data-stu-id="1a3eb-139">Creating Your First Application</span></span>

<span data-ttu-id="1a3eb-140">Visual Basic veya Visual C# programlama dili olarak kullanan uygulamalar oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="1a3eb-141">Visual C# sol tarafta'i seçin ve ardından **ASP.NET MVC 3 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="1a3eb-142">Projeniz "MvcMovie" olarak adlandırın ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="1a3eb-143">(Visual Basic tercih ederseniz, geçiş [Visual Basic sürüm](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.)</span><span class="sxs-lookup"><span data-stu-id="1a3eb-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="1a3eb-144">İçinde **yeni ASP.NET MVC 3 projesinin** iletişim kutusunda **Internet uygulama**.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="1a3eb-145">Denetleme **kullanımı HTML5 biçimlendirme** bırakıp **Razor** varsayılan görünüm altyapısı olarak.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="1a3eb-146">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-146">Click **OK**.</span></span> <span data-ttu-id="1a3eb-147">Visual Web Developer varsayılan bir şablon, yeni oluşturduğunuz, ASP.NET MVC proje için kullanılan çalışan bir uygulama şu anda herhangi bir şey yapmadan elinizde!</span><span class="sxs-lookup"><span data-stu-id="1a3eb-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="1a3eb-148">Bu bir basit "Hello World!" olur</span><span class="sxs-lookup"><span data-stu-id="1a3eb-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="1a3eb-149">Proje ve kullanıcının uygulamanızı başlatmak için uygun bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="1a3eb-150">Gelen **hata ayıklama** menüsünde, select **hata ayıklamayı Başlat**.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="1a3eb-151">Klavye kısayolu hata ayıklamayı başlatmak için F5 olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="1a3eb-152">F5 geliştirme web sunucusu başlatmak ve web uygulamanızı çalıştırmak Visual Web Developer neden olur.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="1a3eb-153">Visual Web Developer bir tarayıcı başlatılır ve uygulamanın giriş sayfasını açar.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="1a3eb-154">Tarayıcının adres çubuğunda diyor bildirimi `localhost` bir şey yok gibi ve `example.com`.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="1a3eb-155">Çünkü `localhost` her zaman, bu durumda yalnızca yerleşik uygulama çalışan kendi yerel bilgisayara gösterir.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="1a3eb-156">Visual Web Developer bir web projesi çalıştığında, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="1a3eb-157">Aşağıdaki resimde 43246 rastgele bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="1a3eb-158">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası büyük olasılıkla görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="1a3eb-159">Kullanıma hazır bu varsayılan şablonu ziyaret etmek için iki sayfa ve temel oturum açma sayfasına sunar.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="1a3eb-160">Bu uygulama şeklini değiştirmek ve biraz işleminde ASP.NET MVC hakkında bilgi almak için sonraki adımdır bakın.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="1a3eb-161">Tarayıcınızı kapatın ve biraz kod değiştirelim.</span><span class="sxs-lookup"><span data-stu-id="1a3eb-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1a3eb-162">Next</span><span class="sxs-lookup"><span data-stu-id="1a3eb-162">Next</span></span>](adding-a-controller.md)
