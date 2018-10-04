---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: ASP.NET MVC 3 (C#) giriş | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack, 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 53306318ab1a782d3605876aac53ec903d053269
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576293"
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="73c8d-103">Giriş ASP.NET MVC 3 (C#)</span><span class="sxs-lookup"><span data-stu-id="73c8d-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="73c8d-104">Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="73c8d-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> > [!NOTE]
> > <span data-ttu-id="73c8d-105">Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır.</span><span class="sxs-lookup"><span data-stu-id="73c8d-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="73c8d-106">Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="73c8d-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="73c8d-107">Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır.</span><span class="sxs-lookup"><span data-stu-id="73c8d-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="73c8d-108">Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun.</span><span class="sxs-lookup"><span data-stu-id="73c8d-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="73c8d-109">Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="73c8d-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="73c8d-110">Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="73c8d-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="73c8d-111">Visual Studio Web Developer Express SP1 önkoşulları</span><span class="sxs-lookup"><span data-stu-id="73c8d-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="73c8d-112">ASP.NET MVC 3 araçları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="73c8d-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="73c8d-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)</span><span class="sxs-lookup"><span data-stu-id="73c8d-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="73c8d-114">Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="73c8d-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="73c8d-115">C# kaynak kodu içeren bir Visual Web Developer proje, bu konuya eşlik etmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="73c8d-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="73c8d-116">[C# sürümü indirme](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="73c8d-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="73c8d-117">Visual Basic tercih ederseniz, geçiş [Visual Basic sürümü](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.</span><span class="sxs-lookup"><span data-stu-id="73c8d-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="73c8d-118">Ne oluşturacaksınız</span><span class="sxs-lookup"><span data-stu-id="73c8d-118">What You'll Build</span></span>

<span data-ttu-id="73c8d-119">Oluşturma, düzenleme ve veritabanı filmler listeleme destekleyen basit bir film listeleme uygulama uygulayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="73c8d-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="73c8d-120">Aşağıda oluşturacağınız uygulama iki ekran görüntüleri verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="73c8d-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="73c8d-121">Film veritabanı listesini görüntüleyen bir sayfa içerir:</span><span class="sxs-lookup"><span data-stu-id="73c8d-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="73c8d-123">Uygulama, ekleme, düzenleme ve tek tek olanları hakkında ayrıntılara bakın yanı sıra, filmler Sil da sağlar.</span><span class="sxs-lookup"><span data-stu-id="73c8d-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="73c8d-124">Tüm veri girişi senaryolar veritabanında depolanan verileri doğru olduğundan emin olmak için doğrulama içerir.</span><span class="sxs-lookup"><span data-stu-id="73c8d-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="73c8d-125">Beceriler hakkında bilgi edineceksiniz</span><span class="sxs-lookup"><span data-stu-id="73c8d-125">Skills You'll Learn</span></span>

<span data-ttu-id="73c8d-126">Öğrenecekleriniz aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="73c8d-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="73c8d-127">Yeni bir ASP.NET MVC projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="73c8d-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="73c8d-128">ASP.NET MVC denetleyicileri ve görünümleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="73c8d-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="73c8d-129">Entity Framework Code First paradigması kullanarak yeni bir veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="73c8d-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="73c8d-130">Nasıl alınacağını ve verileri görüntüle.</span><span class="sxs-lookup"><span data-stu-id="73c8d-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="73c8d-131">Verileri düzenleme ve veri doğrulama etkinleştirme hakkında.</span><span class="sxs-lookup"><span data-stu-id="73c8d-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="73c8d-132">Başlarken</span><span class="sxs-lookup"><span data-stu-id="73c8d-132">Getting Started</span></span>

<span data-ttu-id="73c8d-133">Visual Web Developer 2010 Express'in ("Visual Web Developer" kısaca) çalıştırarak ve seçin **yeni proje** gelen **Başlat** sayfası.</span><span class="sxs-lookup"><span data-stu-id="73c8d-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="73c8d-134">Visual Web Developer, bir IDE ya da tümleşik geliştirme ortamı yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="73c8d-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="73c8d-135">Belgeler yazmak için Microsoft Word kullanma gibi bir IDE uygulamalar oluşturmak için kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="73c8d-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="73c8d-136">Visual Web Developer kullanarak bir araç çubuğu için çeşitli seçenekler kullanılabilir gösteren üstünde yoktur.</span><span class="sxs-lookup"><span data-stu-id="73c8d-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="73c8d-137">IDE'de görevleri gerçekleştirmek için başka bir yol sağlayan bir menüsünde de mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="73c8d-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="73c8d-138">(Örneğin seçmek yerine **yeni proje** gelen **Başlat** sayfasında menüsünü kullanın ve seçin **dosya** &gt; **YeniProje**.)</span><span class="sxs-lookup"><span data-stu-id="73c8d-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="73c8d-139">İlk uygulamanızı oluşturma</span><span class="sxs-lookup"><span data-stu-id="73c8d-139">Creating Your First Application</span></span>

<span data-ttu-id="73c8d-140">Programlama dili olarak Visual Basic veya Visual C# kullanarak uygulamalar oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="73c8d-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="73c8d-141">Visual C# sol tarafta'i seçin ve ardından **ASP.NET MVC 3, Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="73c8d-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="73c8d-142">"MvcMovie" projenizi adlandırın ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="73c8d-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="73c8d-143">(Visual Basic tercih ederseniz, geçiş [Visual Basic sürümü](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.)</span><span class="sxs-lookup"><span data-stu-id="73c8d-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="73c8d-144">İçinde **yeni ASP.NET MVC 3 projesini** iletişim kutusunda **Internet uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="73c8d-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="73c8d-145">Denetleme **kullanımı HTML5 biçimlendirme** bırakıp **Razor** varsayılan görünüm altyapısı olarak.</span><span class="sxs-lookup"><span data-stu-id="73c8d-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="73c8d-146">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="73c8d-146">Click **OK**.</span></span> <span data-ttu-id="73c8d-147">Çalışan bir uygulama şu anda hiçbir şey yapmadan sahip olduğunuz visual Web Developer az önce oluşturduğunuz bir ASP.NET MVC projesi için varsayılan bir şablon kullanılan!</span><span class="sxs-lookup"><span data-stu-id="73c8d-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="73c8d-148">Bu, bir basit "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="73c8d-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="73c8d-149">Proje ve kullanıcının uygulamanızı başlatmak için iyi bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="73c8d-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="73c8d-150">Gelen **hata ayıklama** menüsünde **hata ayıklamayı Başlat**.</span><span class="sxs-lookup"><span data-stu-id="73c8d-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="73c8d-151">Klavye kısayolu hata ayıklamayı Başlat F5 olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="73c8d-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="73c8d-152">F5'e bir geliştirme web sunucusunu başlatmak ve web uygulamanızı çalıştırmak Visual Web Developer neden olur.</span><span class="sxs-lookup"><span data-stu-id="73c8d-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="73c8d-153">Visual Web Developer, ardından bir tarayıcı başlatır ve uygulamanın giriş sayfası açılır.</span><span class="sxs-lookup"><span data-stu-id="73c8d-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="73c8d-154">Tarayıcının adres çubuğunda yazılı bildirimi `localhost` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="73c8d-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="73c8d-155">Çünkü `localhost` her zaman bu durumda yeni oluşturduğunuz uygulamayı çalıştıran kendi yerel bilgisayara işaret eder.</span><span class="sxs-lookup"><span data-stu-id="73c8d-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="73c8d-156">Visual Web Developer bir web projesi çalıştığında, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="73c8d-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="73c8d-157">Aşağıdaki görüntüde, 43246 rastgele bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="73c8d-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="73c8d-158">Uygulamayı çalıştırdığınızda, büyük olasılıkla farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="73c8d-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="73c8d-159">Kullanıma hazır bu varsayılan şablonu ziyaret etmek için iki sayfa ve bir temel oturum açma sayfası sunar.</span><span class="sxs-lookup"><span data-stu-id="73c8d-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="73c8d-160">Bu uygulamanın nasıl çalıştığını değiştirmek ve biraz işlemde ASP.NET MVC hakkında bilgi edinmek için sonraki adımdır bakın.</span><span class="sxs-lookup"><span data-stu-id="73c8d-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="73c8d-161">Tarayıcınızı kapatın ve bazı kod değiştirelim.</span><span class="sxs-lookup"><span data-stu-id="73c8d-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="73c8d-162">Next</span><span class="sxs-lookup"><span data-stu-id="73c8d-162">Next</span></span>](adding-a-controller.md)
