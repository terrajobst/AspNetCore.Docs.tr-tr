---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: ASP.NET MVC giriş | Microsoft Docs
author: shanselman
description: ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 476d832e389b9b5a26fe2d552ca648c79b100056
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
ms.locfileid: "30868496"
---
<a name="intro-to-aspnet-mvc"></a><span data-ttu-id="d43e7-104">ASP.NET MVC giriş</span><span class="sxs-lookup"><span data-stu-id="d43e7-104">Intro to ASP.NET MVC</span></span>
====================
<span data-ttu-id="d43e7-105">tarafından [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="d43e7-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="d43e7-106">Bu öğretici kullanılabiliyorsa, güncelleştirilmiş bir sürümünü [burada](../../getting-started/introduction/getting-started.md) kullanarak [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span><span class="sxs-lookup"><span data-stu-id="d43e7-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="d43e7-107">Yeni öğretici Bu öğretici birçok iyileştirme sağlayan ASP.NET MVC 5 kullanır.</span><span class="sxs-lookup"><span data-stu-id="d43e7-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="d43e7-108">ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur.</span><span class="sxs-lookup"><span data-stu-id="d43e7-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="d43e7-109">Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="d43e7-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="d43e7-110">Ziyaret [ASP.NET MVC öğrenme Merkezi](../../../index.md) diğer ASP.NET MVC öğreticiler ve örnekleri bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="d43e7-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="d43e7-111">Bizim ilk ASP.NET MVC Web uygulaması kullanarak olalım [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="d43e7-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="d43e7-112">Biz, bize oluşturur ve filmler listesinde biraz film listesi uygulaması yapacağız.</span><span class="sxs-lookup"><span data-stu-id="d43e7-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="d43e7-113">Ne oluşturacağınız</span><span class="sxs-lookup"><span data-stu-id="d43e7-113">What You'll Build</span></span>

<span data-ttu-id="d43e7-114">Burada, oluşturacağınız uygulamasının iki ekran görüntüleri verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d43e7-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="d43e7-115">Film çeşitli sütunları içeren basit bir tablo sahip olur.</span><span class="sxs-lookup"><span data-stu-id="d43e7-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="d43e7-116">[![Film listesi - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d43e7-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="d43e7-117">Ve filmler listesine ekleyebilmek için bir kayıt formunu sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="d43e7-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="d43e7-118">[![Windows Internet Explorer (2) bir filmi - oluşturun](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d43e7-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="d43e7-119">Bilgi edineceksiniz yetenekleri</span><span class="sxs-lookup"><span data-stu-id="d43e7-119">Skills You'll Learn</span></span>

<span data-ttu-id="d43e7-120">Bu öğretici Visual Studio kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek.</span><span class="sxs-lookup"><span data-stu-id="d43e7-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="d43e7-121">Şunları öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="d43e7-121">You'll learn:</span></span>

- <span data-ttu-id="d43e7-122">Yeni bir ASP.NET MVC projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="d43e7-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="d43e7-123">SQL Server ile yeni bir veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="d43e7-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="d43e7-124">ASP.NET MVC denetleyicileri ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="d43e7-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="d43e7-125">Nasıl alınacağını ve verileri görüntüle</span><span class="sxs-lookup"><span data-stu-id="d43e7-125">How to retrieve and display data</span></span>
- <span data-ttu-id="d43e7-126">Verileri düzenleme ve veri doğrulama etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="d43e7-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="d43e7-127">Veritabanı şemasının güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="d43e7-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="d43e7-128">Başlarken</span><span class="sxs-lookup"><span data-stu-id="d43e7-128">Get Started</span></span>

<span data-ttu-id="d43e7-129">Visual Web Developer 2010 (ı "VWD" şu andan itibaren çağrısından) Express ve yeni proje seçin Başlat ekranından çalıştırarak başlayın.</span><span class="sxs-lookup"><span data-stu-id="d43e7-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="d43e7-130">Visual Web Developer bir IDE veya tümleşik geliştirme ortamı değil.</span><span class="sxs-lookup"><span data-stu-id="d43e7-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="d43e7-131">Belgeleri yazmak için Microsoft Word kullanma gibi uygulamaları oluşturmak için bir IDE kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="d43e7-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="d43e7-132">Bir araç çubuğu, yanı sıra de kullanmış seçin dosyasına menü için çeşitli seçenekleri gösteren üstünde yoktur | Yeni proje.</span><span class="sxs-lookup"><span data-stu-id="d43e7-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="d43e7-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d43e7-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="d43e7-134">İlk uygulamanızı oluşturma</span><span class="sxs-lookup"><span data-stu-id="d43e7-134">Creating Your First Application</span></span>

<span data-ttu-id="d43e7-135">Visual Basic veya Visual C# kullanarak uygulama oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d43e7-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="d43e7-136">Şu an seçin Visual C# solda, ardından "ASP.NET MVC 2 Web uygulaması." seçin</span><span class="sxs-lookup"><span data-stu-id="d43e7-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="d43e7-137">"Filmler" projenizi adlandırın ve Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="d43e7-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="d43e7-138">[![Yeni Proje](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="d43e7-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="d43e7-139">Sağ tarafta tüm dosya ve klasörler, uygulamanızda gösteren Çözüm Gezgini ' dir.</span><span class="sxs-lookup"><span data-stu-id="d43e7-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="d43e7-140">Büyük ortasında burada kodunuzu düzenleyin ve çoğu zaman, harcamanız penceredir.</span><span class="sxs-lookup"><span data-stu-id="d43e7-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="d43e7-141">Visual Studio varsayılan bir şablon, yeni oluşturduğunuz, ASP.NET MVC proje için kullanılan çalışan bir uygulama şu anda herhangi bir şey yapmadan elinizde!</span><span class="sxs-lookup"><span data-stu-id="d43e7-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="d43e7-142">Bu bir basit "Hello World! olur</span><span class="sxs-lookup"><span data-stu-id="d43e7-142">This is a simple "Hello World!</span></span> <span data-ttu-id="d43e7-143">Proje ve uygulamamız için başlatmak için iyi bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="d43e7-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="d43e7-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="d43e7-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="d43e7-145">Araç çubuğuna "Çalıştır" düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="d43e7-145">Select the "play" button to the toolbar.</span></span>

![Hata ayıklama başlatılamıyor](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="d43e7-147">Programınızı derlemek ve uygulamanızı bir web tarayıcısında başlatın sağa gösteren yeşil ok olur.</span><span class="sxs-lookup"><span data-stu-id="d43e7-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="d43e7-148">*Not: Yerine klavyenizde F5'e basın veya için hata ayıklama - seçin&gt;hata ayıklamayı Başlat "Debug" menüsünden.*</span><span class="sxs-lookup"><span data-stu-id="d43e7-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="d43e7-149">Bu, bir geliştirme web sunucusu başlatmak ve (yapılandırma veya yok bunu etkinleştirmek için gereken adımları el ile) web uygulamamızı çalıştırmak Visual Web Developer neden olur.</span><span class="sxs-lookup"><span data-stu-id="d43e7-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="d43e7-150">Ardından bir tarayıcıyı başlatacak ve uygulama giriş sayfası göz atmak için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d43e7-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="d43e7-151">Aşağıda tarayıcının adres çubuğunda "localhost" ve example.com gibi şeyler diyor dikkat edin. Localhost her zaman bu durumda yalnızca oluşturduğumuz uygulama çalışan kendi yerel bilgisayarına - işaret eden olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="d43e7-151">Notice below that the address bar of the browser says "localhost", and not something like example.com. That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="d43e7-152">[![Giriş sayfası](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="d43e7-152">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="d43e7-153">Kutudan çıktığında bu varsayılan şablonu ziyaret etmek için iki sayfaları ve temel oturum açma sayfasına sunar.</span><span class="sxs-lookup"><span data-stu-id="d43e7-153">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="d43e7-154">Şimdi bu uygulamayı nasıl çalıştığını değiştirin ve biraz işleminde ASP.NET MVC hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="d43e7-154">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="d43e7-155">Tarayıcınızı kapatın ve bazı kodunu değiştirmek olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="d43e7-155">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d43e7-156">Next</span><span class="sxs-lookup"><span data-stu-id="d43e7-156">Next</span></span>](getting-started-with-mvc-part2.md)
