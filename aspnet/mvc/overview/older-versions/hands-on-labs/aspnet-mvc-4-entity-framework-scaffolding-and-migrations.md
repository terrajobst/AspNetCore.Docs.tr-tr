---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: "ASP.NET MVC 4 Entity Framework İskele ve geçişleri | Microsoft Docs"
author: rick-anderson
description: "ASP.NET MVC 4 denetleyici yöntemleriyle bilginiz veya tamamladınız &quot;Yardımcıları, formlar ve doğrulama&quot; uygulamalı Laboratuvar olmanız gerekir kullanan..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 396859463446d95c58271c4b00fc950bcd0d539a
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="78aab-103">ASP.NET MVC 4 Entity Framework İskele ve geçişleri</span><span class="sxs-lookup"><span data-stu-id="78aab-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="78aab-104">Tarafından [Web Camps ekibi](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="78aab-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="78aab-105">Kit eğitim Web Camps indirin</span><span class="sxs-lookup"><span data-stu-id="78aab-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="78aab-106">ASP.NET MVC 4 denetleyici yöntemleriyle bilginiz veya tamamladınız &quot;Yardımcıları, formlar ve doğrulama&quot; uygulamalı Laboratuvar size dikkat oluşturmak için mantığı çoğunu güncelleştirme, listelemek ve onu yineleniyor herhangi bir veri varlık kaldırır uygulama arasında.</span><span class="sxs-lookup"><span data-stu-id="78aab-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="78aab-107">Modelinizi işlemek için birden fazla sınıf varsa, değil belirtmeyi, her varlık işlemi yanı sıra her görünüm için POST ve GET eylem yöntemleri yazma önemli bir süre beklemesini olası olacaktır.</span><span class="sxs-lookup"><span data-stu-id="78aab-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="78aab-108">Bu laboratuvarda, ASP.NET MVC 4 yapı iskelesi otomatik olarak, uygulamanızın CRUD (Oluştur, okuma, güncelleştirme ve silme) taban çizgisi oluşturmak için nasıl kullanılacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="78aab-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="78aab-109">Basit model sınıfından ve tek satırlık bir kod yazmayı olmadan başlayarak, tüm gerekli görünümler yanı sıra tüm CRUD işlemleri içeren bir denetleyici oluşturur.</span><span class="sxs-lookup"><span data-stu-id="78aab-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="78aab-110">Oluşturma ve basit çözüm çalıştırma sonra MVC mantığı ve verileri işleme için görünümleri ile birlikte oluşturulan uygulama veritabanı gerekir.</span><span class="sxs-lookup"><span data-stu-id="78aab-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="78aab-111">Ayrıca, tüm uygulama boyunca model güncelleştirmelerini gerçekleştirmek için Entity Framework geçişler kullanın ne kadar kolay olduğunu öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="78aab-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="78aab-112">Entity Framework geçişler, model ile basit adımları değiştikten sonra veritabanını değiştirme olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="78aab-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="78aab-113">Tüm bu konularda unutmayın, ASP.NET MVC 4'ın en son özelliklerini yararlanarak oluşturmak ve web uygulamalarını daha verimli bir şekilde korumak kuramaz.</span><span class="sxs-lookup"><span data-stu-id="78aab-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="78aab-114">Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresindeki kullanılabilir içinde yer alan [Microsoft-Web/WebCampTrainingKit sürümleri](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="78aab-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="78aab-115">Bu Laboratuvar için belirli proje şu adresten edinilebilir [ASP.NET MVC 4 Entity Framework İskele ve geçişleri](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="78aab-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="78aab-116">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="78aab-116">Objectives</span></span>

<span data-ttu-id="78aab-117">Bu uygulamalı laboratuar ortamında, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="78aab-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="78aab-118">ASP.NET iskele denetleyicileriyle CRUD işlemleri için kullanın.</span><span class="sxs-lookup"><span data-stu-id="78aab-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="78aab-119">Entity Framework geçişleri kullanmaya veritabanı modelini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="78aab-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="78aab-120">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="78aab-120">Prerequisites</span></span>

<span data-ttu-id="78aab-121">Bu laboratuvarı tamamlamak için aşağıdaki öğeleri sahip olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="78aab-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="78aab-122">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="78aab-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="78aab-123">Kurulum</span><span class="sxs-lookup"><span data-stu-id="78aab-123">Setup</span></span>

<span data-ttu-id="78aab-124">**Kod parçacıkları yükleme**</span><span class="sxs-lookup"><span data-stu-id="78aab-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="78aab-125">Kolaylık olması için bu Laboratuvar yönetme kod çoğunu Visual Studio kod parçacıkları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="78aab-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="78aab-126">Çalıştırma kod parçacıkları yüklemek için **.\Source\Setup\CodeSnippets.vsi** dosya.</span><span class="sxs-lookup"><span data-stu-id="78aab-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="78aab-127">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek B: kullanarak kod parçacıkları](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="78aab-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="78aab-128">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="78aab-128">Exercises</span></span>

<span data-ttu-id="78aab-129">Aşağıdaki alıştırmada bu uygulamalı laboratuarı olun:</span><span class="sxs-lookup"><span data-stu-id="78aab-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="78aab-130">ASP.NET MVC 4 yapı İskelesi Entity Framework geçişler ile kullanma</span><span class="sxs-lookup"><span data-stu-id="78aab-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="78aab-131">Bu alıştırmada tarafından eşlik bir **son** elde alıştırma tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="78aab-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="78aab-132">Alıştırma ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78aab-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="78aab-133">Bu laboratuvarı tamamlamak için süre tahmini: **30 dakika**</span><span class="sxs-lookup"><span data-stu-id="78aab-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="78aab-134">Alıştırma 1: ASP.NET MVC 4 yapı İskelesi Entity Framework geçişler ile kullanma</span><span class="sxs-lookup"><span data-stu-id="78aab-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="78aab-135">ASP.NET MVC yapı iskelesi CRUD işlemleri uygulamanızı veritabanı katmanı ile etkileşime olanak sağlayan gerekli mantığı oluşturma standartlaştırılmış bir şekilde oluşturmak için hızlı bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="78aab-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="78aab-136">Bu alıştırmada, ASP.NET MVC 4 yapı iskelesi koduyla ilk CRUD yöntemleri oluşturmak için nasıl kullanılacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="78aab-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="78aab-137">Ardından, Entity Framework geçişler kullanarak veritabanındaki değişiklikleri uygulamadan modelinizi güncelleştirme konusunda bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="78aab-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="78aab-138">Yapı İskelesi kullanarak görev 1-oluşturma yeni bir ASP.NET MVC 4 Proje</span><span class="sxs-lookup"><span data-stu-id="78aab-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="78aab-139">Zaten açık değilse, başlangıç **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="78aab-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="78aab-140">Seçin **dosya | Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="78aab-140">Select **File | New Project**.</span></span> <span data-ttu-id="78aab-141">Yeni Proje iletişim kutusunda, altında **Visual C# | Web** bölümünde, select **ASP.NET MVC 4 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="78aab-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="78aab-142">Proje adı **MVC4andEFMigrations** ve konumunu **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** bu laboratuvarı klasör.</span><span class="sxs-lookup"><span data-stu-id="78aab-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="78aab-143">Ayarlama **çözüm adı** için **başlamak** ve olun **çözüm için dizin oluştur** denetlenir.</span><span class="sxs-lookup"><span data-stu-id="78aab-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="78aab-144">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="78aab-144">Click **OK**.</span></span>

    <span data-ttu-id="78aab-145">![Yeni ASP.NET MVC 4 Proje iletişim kutusu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "yeni ASP.NET MVC 4 Proje iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="78aab-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="78aab-146">*Yeni ASP.NET MVC 4 Proje iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="78aab-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="78aab-147">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusu seç **Internet uygulama** şablon emin olun **Razor** seçili olan **görünüm altyapısı**.</span><span class="sxs-lookup"><span data-stu-id="78aab-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="78aab-148">Tıklatın **Tamam** projesi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="78aab-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="78aab-149">![Yeni ASP.NET MVC 4 Internet uygulaması](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "yeni bir ASP.NET MVC 4 Internet uygulaması")</span><span class="sxs-lookup"><span data-stu-id="78aab-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="78aab-150">*Yeni ASP.NET MVC 4 Internet uygulaması*</span><span class="sxs-lookup"><span data-stu-id="78aab-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="78aab-151">Çözüm Gezgini'nde sağ **modelleri** seçip **Ekle | Sınıf** basit sınıf kişi (POCO) oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="78aab-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="78aab-152">Bu ad **kişi** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="78aab-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="78aab-153">Kişi sınıfı açın ve aşağıdaki özellikleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="78aab-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="78aab-154">(Kod parçacığını - *ASP.NET MVC 4 ve Entity Framework geçişleri - Ex1 kişi özellikleri*)</span><span class="sxs-lookup"><span data-stu-id="78aab-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="78aab-155">Tıklatın **yapı | Çözümü derlemek** değişiklikleri kaydetmek ve projeyi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="78aab-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="78aab-156">![Uygulama oluşturma](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "uygulama oluşturma")</span><span class="sxs-lookup"><span data-stu-id="78aab-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="78aab-157">*Uygulama oluşturma*</span><span class="sxs-lookup"><span data-stu-id="78aab-157">*Building the Application*</span></span>
7. <span data-ttu-id="78aab-158">Çözüm Gezgini'nde denetleyicileri klasörünü sağ tıklatın ve seçin **Ekle | Denetleyici**.</span><span class="sxs-lookup"><span data-stu-id="78aab-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="78aab-159">Denetleyici adı *PersonController* ve tamamlamak **yapı İskelesi seçenekleri** aşağıdaki değerlere sahip.</span><span class="sxs-lookup"><span data-stu-id="78aab-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

    1. <span data-ttu-id="78aab-160">İçinde **şablonu** aşağı açılan listesinden, **okuma/yazma eylemleri ve Entity Framework kullanarak görünümleri ile MVC denetleyicisi** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="78aab-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
    2. <span data-ttu-id="78aab-161">İçinde **Model sınıfı** aşağı açılan listesinden, **kişi** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="78aab-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
    3. <span data-ttu-id="78aab-162">İçinde **veri bağlamı sınıfı** listesinde  **&lt;yeni veri bağlamı... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="78aab-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="78aab-163">Herhangi bir ad seçin ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="78aab-163">Choose any name and click **OK**.</span></span>
    4. <span data-ttu-id="78aab-164">İçinde **görünümleri** aşağı açılan listesinde, olduğundan emin olun **Razor** seçilir.</span><span class="sxs-lookup"><span data-stu-id="78aab-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

    <span data-ttu-id="78aab-165">![Yapı iskelesi ile kişi denetleyicisi ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "yapı iskelesi ile kişi denetleyicisi ekleme")</span><span class="sxs-lookup"><span data-stu-id="78aab-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

    <span data-ttu-id="78aab-166">*Yapı iskelesi ile kişi denetleyicisi ekleme*</span><span class="sxs-lookup"><span data-stu-id="78aab-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="78aab-167">Tıklatın **Ekle** yeni denetleyicisi kişi için yapı iskelesi ile oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="78aab-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="78aab-168">Denetleyici eylemleri ve bunun yanı sıra görünümleri şimdi oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="78aab-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="78aab-169">![Yapı iskelesi ile kişi denetleyicisi oluşturduktan sonra](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "yapı iskelesi ile kişi denetleyicisi oluşturduktan sonra")</span><span class="sxs-lookup"><span data-stu-id="78aab-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="78aab-170">*Yapı iskelesi ile kişi denetleyicisi oluşturduktan sonra*</span><span class="sxs-lookup"><span data-stu-id="78aab-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="78aab-171">Açık **PersonController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="78aab-171">Open **PersonController** class.</span></span> <span data-ttu-id="78aab-172">Tam CRUD eylem yöntemlerine otomatik olarak oluşturulmuş olan dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="78aab-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

    <span data-ttu-id="78aab-173">![Kişi denetleyicisi içinde](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "içinde kişi denetleyicisi")</span><span class="sxs-lookup"><span data-stu-id="78aab-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

    <span data-ttu-id="78aab-174">*İçinde kişi denetleyicisi*</span><span class="sxs-lookup"><span data-stu-id="78aab-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="78aab-175">Görev 2-çalışan uygulama</span><span class="sxs-lookup"><span data-stu-id="78aab-175">Task 2- Running the application</span></span>

<span data-ttu-id="78aab-176">Bu noktada, veritabanı henüz oluşturulmadı.</span><span class="sxs-lookup"><span data-stu-id="78aab-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="78aab-177">Bu görevde, uygulamayı ilk kez çalıştırma ve CRUD işlemleri test etme.</span><span class="sxs-lookup"><span data-stu-id="78aab-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="78aab-178">Veritabanı Code First ile kolay bir şekilde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="78aab-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="78aab-179">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="78aab-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="78aab-180">Tarayıcıda eklemek **/Person** kişi sayfasını açmak için URL.</span><span class="sxs-lookup"><span data-stu-id="78aab-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="78aab-181">![Uygulaması ilk çalıştırma](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "uygulaması ilk çalıştırma")</span><span class="sxs-lookup"><span data-stu-id="78aab-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="78aab-182">*Uygulama: ilk çalıştırma*</span><span class="sxs-lookup"><span data-stu-id="78aab-182">*Application: first run*</span></span>
3. <span data-ttu-id="78aab-183">Şimdi kişi sayfaları keşfedin ve CRUD işlemleri test.</span><span class="sxs-lookup"><span data-stu-id="78aab-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="78aab-184">Tıklatın **Yeni Oluştur** yeni bir kişiye eklemek için.</span><span class="sxs-lookup"><span data-stu-id="78aab-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="78aab-185">Bir ad ve soyadı girin ve tıklayın **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="78aab-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="78aab-186">![Yeni bir kişiye ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "yeni bir kişiye ekleme")</span><span class="sxs-lookup"><span data-stu-id="78aab-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="78aab-187">*Yeni bir kişiye ekleme*</span><span class="sxs-lookup"><span data-stu-id="78aab-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="78aab-188">Kişinin listesinde, silme, düzenleme veya öğeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="78aab-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="78aab-189">![kişi listesi](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "kişi listesi")</span><span class="sxs-lookup"><span data-stu-id="78aab-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="78aab-190">*Kişi listesi*</span><span class="sxs-lookup"><span data-stu-id="78aab-190">*Person list*</span></span>
    3. <span data-ttu-id="78aab-191">Tıklatın **ayrıntıları** kişinin ayrıntıları açın.</span><span class="sxs-lookup"><span data-stu-id="78aab-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="78aab-192">![Kişinin ayrıntıları](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "kişinin ayrıntıları")</span><span class="sxs-lookup"><span data-stu-id="78aab-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="78aab-193">*Kişinin ayrıntıları*</span><span class="sxs-lookup"><span data-stu-id="78aab-193">*Person's details*</span></span>
4. <span data-ttu-id="78aab-194">Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.</span><span class="sxs-lookup"><span data-stu-id="78aab-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="78aab-195">Kişi varlığı için tüm CRUD uygulamanızdan - görünümleri modeline - boyunca tek satırlık bir kod yazmak zorunda kalmadan oluşturduğunuz dikkat edin!</span><span class="sxs-lookup"><span data-stu-id="78aab-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="78aab-196">Görev 3-güncelleştirme Entity Framework geçişler kullanarak veritabanı</span><span class="sxs-lookup"><span data-stu-id="78aab-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="78aab-197">Bu görevde Entity Framework geçişler kullanarak veritabanını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="78aab-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="78aab-198">Model değiştirmek ve Entity Framework geçişler özelliğini kullanarak veritabanlarınızı değişiklikleri yansıtmak için ne kadar kolay olduğunu keşfeder.</span><span class="sxs-lookup"><span data-stu-id="78aab-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="78aab-199">Paket Yöneticisi Konsolu'nu açın.</span><span class="sxs-lookup"><span data-stu-id="78aab-199">Open the Package Manager Console.</span></span> <span data-ttu-id="78aab-200">Seçin **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="78aab-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="78aab-201">Paket Yöneticisi konsolunda aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="78aab-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="78aab-202">PMC</span><span class="sxs-lookup"><span data-stu-id="78aab-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="78aab-203">![Geçişler etkinleştirme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "geçişler etkinleştirme")</span><span class="sxs-lookup"><span data-stu-id="78aab-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="78aab-204">*Geçişler etkinleştirme*</span><span class="sxs-lookup"><span data-stu-id="78aab-204">*Enabling migrations*</span></span>

    <span data-ttu-id="78aab-205">Enable-Migration komutunu oluşturur **geçişler** veritabanını başlatmak için komut dosyası içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="78aab-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="78aab-206">![Migrations klasörünü](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "geçişler klasörü")</span><span class="sxs-lookup"><span data-stu-id="78aab-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="78aab-207">*Geçişler klasörü*</span><span class="sxs-lookup"><span data-stu-id="78aab-207">*Migrations folder*</span></span>
3. <span data-ttu-id="78aab-208">Açık **Configuration.cs** Migrations klasörünü dosyasında.</span><span class="sxs-lookup"><span data-stu-id="78aab-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="78aab-209">Sınıf oluşturucu bulun ve değiştirin **AutomaticMigrationsEnabled** değeri *doğru*.</span><span class="sxs-lookup"><span data-stu-id="78aab-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="78aab-210">Kişi sınıfı açın ve bir öznitelik için kişinin ikinci adını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="78aab-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="78aab-211">Bu yeni öznitelik model değiştiriyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="78aab-211">With this new attribute, you are changing the model.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="78aab-212">Seçin **yapı | Çözümü derlemek** uygulamayı yapılandırmak için menüsünde.</span><span class="sxs-lookup"><span data-stu-id="78aab-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="78aab-213">![Uygulama oluşturma](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "uygulama oluşturma")</span><span class="sxs-lookup"><span data-stu-id="78aab-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="78aab-214">*Uygulama oluşturma*</span><span class="sxs-lookup"><span data-stu-id="78aab-214">*Building the application*</span></span>
6. <span data-ttu-id="78aab-215">Paket Yöneticisi konsolunda aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="78aab-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="78aab-216">PMC</span><span class="sxs-lookup"><span data-stu-id="78aab-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="78aab-217">Bu komut veri nesnelerde değişiklikler arar ve daha sonra veritabanını uygun şekilde değiştirmek için gerekli komutları ekleyecektir.</span><span class="sxs-lookup"><span data-stu-id="78aab-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="78aab-218">![İkinci adı ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "ikinci adı ekleme")</span><span class="sxs-lookup"><span data-stu-id="78aab-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="78aab-219">*İkinci adı ekleme*</span><span class="sxs-lookup"><span data-stu-id="78aab-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="78aab-220">(İsteğe bağlı) Fark güncelleştirmesi ile bir SQL komut dosyası oluşturmak için aşağıdaki komutu çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78aab-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="78aab-221">Bu, veritabanını el ile güncelleştirmeniz olanak tanır (Bu durumda ise gerekli değildir), veya diğer veritabanlarındaki değişiklikleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="78aab-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="78aab-222">PMC</span><span class="sxs-lookup"><span data-stu-id="78aab-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="78aab-223">![SQL komut dosyası oluşturuluyor](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "SQL komut dosyası oluşturma")</span><span class="sxs-lookup"><span data-stu-id="78aab-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="78aab-224">*SQL komut dosyası oluşturma*</span><span class="sxs-lookup"><span data-stu-id="78aab-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="78aab-225">![SQL komut dosyasını güncelleştirme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL komut dosyasını güncelleştirme")</span><span class="sxs-lookup"><span data-stu-id="78aab-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="78aab-226">*SQL komut dosyasını güncelleştirme*</span><span class="sxs-lookup"><span data-stu-id="78aab-226">*SQL Script update*</span></span>
8. <span data-ttu-id="78aab-227">Paket Yöneticisi konsolunda veritabanını güncelleştirmek için aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="78aab-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="78aab-228">PMC</span><span class="sxs-lookup"><span data-stu-id="78aab-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="78aab-229">![Veritabanını güncelleştirme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "veritabanını güncelleştirme")</span><span class="sxs-lookup"><span data-stu-id="78aab-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="78aab-230">*Veritabanını güncelleştirme*</span><span class="sxs-lookup"><span data-stu-id="78aab-230">*Updating the Database*</span></span>

    <span data-ttu-id="78aab-231">Bu ekler **MiddleName** sütununda **kişiler** geçerli tanımı eşleştirmek için tablo **kişi** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="78aab-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="78aab-232">Veritabanı güncelleştirildikten sonra denetleyici klasörünü sağ tıklatın ve seçin **Ekle | Denetleyici** kişi denetleyicisi yeniden (aynı değerlere sahip eksiksiz) eklemek için.</span><span class="sxs-lookup"><span data-stu-id="78aab-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="78aab-233">Bu görünümler yeni öznitelik eklemek ve mevcut yöntemler güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="78aab-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="78aab-234">![Bir denetleyici güncelleştirmesini ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "denetleyici güncelleştirmesini ekleme")</span><span class="sxs-lookup"><span data-stu-id="78aab-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="78aab-235">*Denetleyici güncelleştiriliyor*</span><span class="sxs-lookup"><span data-stu-id="78aab-235">*Updating the controller*</span></span>
10. <span data-ttu-id="78aab-236">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="78aab-236">Click **Add**.</span></span> <span data-ttu-id="78aab-237">Ardından, değerleri seçin **üzerine PersonController.cs** ve **üzerine yaz ilişkili görünümleri** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="78aab-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

    ![Bir denetleyici üzerine yaz ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

    <span data-ttu-id="78aab-239">*Denetleyici güncelleştiriliyor*</span><span class="sxs-lookup"><span data-stu-id="78aab-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="78aab-240">Task4 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="78aab-240">Task4- Running the application</span></span>

1. <span data-ttu-id="78aab-241">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="78aab-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="78aab-242">Açık **/Person**.</span><span class="sxs-lookup"><span data-stu-id="78aab-242">Open **/Person**.</span></span> <span data-ttu-id="78aab-243">İkinci Ad sütunu eklendi ancak verileri, korundu olduğunu dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="78aab-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="78aab-244">![İkinci eklenen adı](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "ikinci eklenen adı")</span><span class="sxs-lookup"><span data-stu-id="78aab-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="78aab-245">*İkinci eklenen adı*</span><span class="sxs-lookup"><span data-stu-id="78aab-245">*Middle Name added*</span></span>
3. <span data-ttu-id="78aab-246">Tıklatırsanız **Düzenle**, geçerli kişinin ikinci adı eklemeniz mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="78aab-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="78aab-247">![İkinci adı edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "ikinci adı edition")</span><span class="sxs-lookup"><span data-stu-id="78aab-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="78aab-248">Özet</span><span class="sxs-lookup"><span data-stu-id="78aab-248">Summary</span></span>

<span data-ttu-id="78aab-249">Uygulamalı bu laboratuvarda, ASP.NET MVC 4 herhangi bir model sınıfını kullanarak İskele ile CRUD işlemleri oluşturmak için basit adımları öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="78aab-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="78aab-250">Ardından, Entity Framework geçişler kullanarak bir uçtan uca güncelleştirme uygulamanızdan - görünümleri veritabanına - gerçekleştirmek nasıl öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="78aab-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="78aab-251">Ek A: Yükleme Web Visual Studio Express 2012 için</span><span class="sxs-lookup"><span data-stu-id="78aab-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="78aab-252">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak  **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="78aab-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="78aab-253">Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="78aab-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="78aab-254">Git [ [https://go.microsoft.com/? LinkID 9810169 =](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="78aab-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="78aab-255">Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; *Visual Studio Express 2012 için Windows Azure SDK'sı Web*&quot;.</span><span class="sxs-lookup"><span data-stu-id="78aab-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="78aab-256">Tıklayın **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="78aab-256">Click on **Install Now**.</span></span> <span data-ttu-id="78aab-257">Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="78aab-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="78aab-258">Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="78aab-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="78aab-259">![Visual Studio Express yükleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "yükleme Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="78aab-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="78aab-260">*Visual Studio Express yükleme*</span><span class="sxs-lookup"><span data-stu-id="78aab-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="78aab-261">Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="78aab-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşulları kabul ediliyor](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="78aab-263">Lisans koşulları kabul ediliyor</span><span class="sxs-lookup"><span data-stu-id="78aab-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="78aab-264">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="78aab-264">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="78aab-266">Yükleme ilerleme durumu</span><span class="sxs-lookup"><span data-stu-id="78aab-266">*Installation progress*</span></span>
6. <span data-ttu-id="78aab-267">Yükleme tamamlandığında tıklatın **son**.</span><span class="sxs-lookup"><span data-stu-id="78aab-267">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="78aab-269">Yükleme tamamlandı</span><span class="sxs-lookup"><span data-stu-id="78aab-269">*Installation completed*</span></span>
7. <span data-ttu-id="78aab-270">Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="78aab-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="78aab-271">Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.</span><span class="sxs-lookup"><span data-stu-id="78aab-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web döşemeye](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="78aab-273">VS Express Web döşemeye</span><span class="sxs-lookup"><span data-stu-id="78aab-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="78aab-274">Ek B: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="78aab-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="78aab-275">Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip.</span><span class="sxs-lookup"><span data-stu-id="78aab-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="78aab-276">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="78aab-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="78aab-277">![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="78aab-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="78aab-278">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="78aab-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="78aab-279">***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="78aab-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="78aab-280">İmleci, burada kod eklemek istediğiniz yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="78aab-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="78aab-281">(Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="78aab-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="78aab-282">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="78aab-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="78aab-283">Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="78aab-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="78aab-284">İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="78aab-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="78aab-285">![Kod parçacığında adını yazmaya başlayın](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="78aab-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="78aab-286">*Kod parçacığında adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="78aab-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="78aab-287">![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="78aab-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="78aab-288">*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="78aab-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="78aab-289">![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")</span><span class="sxs-lookup"><span data-stu-id="78aab-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="78aab-290">*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*</span><span class="sxs-lookup"><span data-stu-id="78aab-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="78aab-291">***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1.</span><span class="sxs-lookup"><span data-stu-id="78aab-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="78aab-292">Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="78aab-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="78aab-293">Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.</span><span class="sxs-lookup"><span data-stu-id="78aab-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="78aab-294">Tıklayarak ilgili kod parçacığında listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="78aab-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="78aab-295">![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")</span><span class="sxs-lookup"><span data-stu-id="78aab-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="78aab-296">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*</span><span class="sxs-lookup"><span data-stu-id="78aab-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="78aab-297">![Tıklayarak ilgili kod parçacığında listeden çekme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "tıklayarak ilgili kod parçacığında listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="78aab-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="78aab-298">*Tıklayarak ilgili kod parçacığında listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="78aab-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
