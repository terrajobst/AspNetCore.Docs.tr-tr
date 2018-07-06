---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework iskele oluşturma ve geçişler | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 denetleyici yöntemleri ile ilgili bilgi sahibi olduğunuz veya tamamlamış olmanız durumunda &quot;yardımcılar, formlar ve doğrulama&quot; uygulamalı laboratuvarı size dikkat edin...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 31f593f294c4865f621a8556cb43d0d9c42f2660
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814128"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="9e7ff-103">ASP.NET MVC 4 Entity Framework iskele oluşturma ve geçişler</span><span class="sxs-lookup"><span data-stu-id="9e7ff-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="9e7ff-104">Tarafından [Team Web Kampları](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="9e7ff-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="9e7ff-105">Eğitim Seti Web Kampları indirin</span><span class="sxs-lookup"><span data-stu-id="9e7ff-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="9e7ff-106">ASP.NET MVC 4 denetleyici yöntemleri ile ilgili bilgi sahibi olduğunuz veya tamamlamış olmanız durumunda &quot;yardımcılar, formlar ve doğrulama&quot; uygulamalı laboratuvarı olmanız gerekir birçok oluşturmak için mantıksal güncelleştirme, liste ve bunu yinelenir herhangi bir veri varlığı kaldırmak olduğunu aklınızda bulundurun uygulama arasında.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="9e7ff-107">Modelinizi işlemek için çeşitli sınıflar varsa, uyumlu için büyük olasılıkla her varlık işlemi yanı sıra her bir görünüm için POST ve GET eylem yöntemlerini yazma önemli bir zaman ayırın.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="9e7ff-108">Bu laboratuvarda, ASP.NET MVC 4 yapı iskelesi otomatik olarak uygulamanızın CRUD (oluşturma, okuma, güncelleştirme ve silme) taban çizgisini oluşturmak için nasıl kullanılacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="9e7ff-109">Basit model sınıfı ve, tek satırlık bir kod yazmadan başlayarak, tüm gerekli görünümleri yanı sıra tüm CRUD işlemleri içeren bir denetleyici oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="9e7ff-110">Oluşturma ve basit bir çözüm çalıştırma sonrasında, MVC mantığı ve veri işleme için görünümleri ile birlikte oluşturulan uygulama veritabanı gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="9e7ff-111">Ayrıca, tüm uygulamanızda model güncelleştirmelerini gerçekleştirmek için Entity Framework geçişleri kullanmak için ne kadar kolay olduğunu öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="9e7ff-112">Entity Framework geçişleri, model basit adımlarla değiştirildikten sonra veritabanını değiştirme olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="9e7ff-113">Tüm bu konularda unutmayın, ASP.NET MVC 4'ün en son özelliklerden yararlanarak oluşturun ve web uygulamalarını daha verimli bir şekilde korumak mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="9e7ff-114">Web Kampları eğitim Seti'nde bulunan, tüm örnek kodu ve kod parçacıkları dahil [Microsoft-Web/WebCampTrainingKit yayınlar](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="9e7ff-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="9e7ff-115">Bu Laboratuvar için belirli proje kullanılabilir [ASP.NET MVC 4 Entity Framework iskele oluşturma ve geçişler](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="9e7ff-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="9e7ff-116">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="9e7ff-116">Objectives</span></span>

<span data-ttu-id="9e7ff-117">Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="9e7ff-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="9e7ff-118">ASP.NET iskeleti oluşturma denetleyicileri CRUD işlemleri için kullanın.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="9e7ff-119">Entity Framework geçişleri kullanarak veritabanı modeli değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="9e7ff-120">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="9e7ff-120">Prerequisites</span></span>

<span data-ttu-id="9e7ff-121">Bu laboratuvarı tamamlamak için aşağıdakiler olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="9e7ff-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="9e7ff-122">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya üst (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="9e7ff-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="9e7ff-123">Kurulum</span><span class="sxs-lookup"><span data-stu-id="9e7ff-123">Setup</span></span>

<span data-ttu-id="9e7ff-124">**Kod parçacıkları yükleniyor**</span><span class="sxs-lookup"><span data-stu-id="9e7ff-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="9e7ff-125">Kolaylık olması için bu Laboratuvar yöneteceğiniz kodun çoğu Visual Studio kod parçacıkları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="9e7ff-126">Kod parçacıklarını çalıştırmak yüklenecek **.\Source\Setup\CodeSnippets.vsi** dosya.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="9e7ff-127">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istediğiniz konusunda bilgi sahibi değilseniz, bu belge, ek başvurabilir &quot; [ek B: kullanarak kod parçacıklarına](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="9e7ff-128">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="9e7ff-128">Exercises</span></span>

<span data-ttu-id="9e7ff-129">Aşağıdaki alıştırmada bu uygulamalı laboratuvarı ayarlama yapın:</span><span class="sxs-lookup"><span data-stu-id="9e7ff-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="9e7ff-130">ASP.NET MVC 4 yapı İskelesi ile Entity Framework geçişleri kullanma</span><span class="sxs-lookup"><span data-stu-id="9e7ff-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="9e7ff-131">Bu alıştırmada eşlik ettiği bir **son** elde alıştırma tamamlandıktan sonra elde edilen çözümü içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="9e7ff-132">Alıştırma çalışma ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="9e7ff-133">Bu laboratuvarı tamamlamak için tahmini süre: **30 dakika**</span><span class="sxs-lookup"><span data-stu-id="9e7ff-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="9e7ff-134">Alıştırma 1: ASP.NET MVC 4 yapı İskelesi ile Entity Framework geçişleri kullanma</span><span class="sxs-lookup"><span data-stu-id="9e7ff-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="9e7ff-135">ASP.NET MVC yapı iskelesi CRUD işlemleri uygulamanızı veritabanı katmanı ile etkileşime olanak sağlayan gerekli mantığı oluşturma standartlaştırılmış bir şekilde oluşturmak için hızlı bir yolunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="9e7ff-136">Bu alıştırmada, ASP.NET MVC 4 yapı iskelesi ile kod ilk CRUD yöntemler oluşturmak için nasıl kullanılacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="9e7ff-137">Ardından, varlık çerçevesi geçişleriyle kullanarak veritabanındaki değişiklikleri uygulamadan modelinizi güncelleştirme öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="9e7ff-138">Görev 1-oluşturma yeni bir ASP.NET MVC 4 Proje yapı iskelesini kullanmaya</span><span class="sxs-lookup"><span data-stu-id="9e7ff-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="9e7ff-139">Açık değilse, başlangıç **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="9e7ff-140">Seçin **dosya | Yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-140">Select **File | New Project**.</span></span> <span data-ttu-id="9e7ff-141">Yeni iletişim kutusunda, altında proje **Visual C# | Web** bölümünden **ASP.NET MVC 4 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="9e7ff-142">Projeye ad **MVC4andEFMigrations** ve konumunu **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** , bu Laboratuvarın klasör.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="9e7ff-143">Ayarlama **çözüm adı** için **başlamak** olun **çözüm için dizin oluştur** denetlenir.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="9e7ff-144">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-144">Click **OK**.</span></span>

    <span data-ttu-id="9e7ff-145">![Yeni ASP.NET MVC 4 Proje iletişim kutusu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "yeni ASP.NET MVC 4 Proje iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="9e7ff-146">*Yeni ASP.NET MVC 4 Proje iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="9e7ff-147">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunu seçin **Internet uygulaması** şablon emin olun **Razor** seçili olduğu **Görünümaltyapısı**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="9e7ff-148">Tıklayın **Tamam** projeyi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="9e7ff-149">![Yeni bir ASP.NET MVC 4 Internet uygulaması](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "yeni bir ASP.NET MVC 4 Internet uygulaması")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="9e7ff-150">*Yeni bir ASP.NET MVC 4 Internet uygulaması*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="9e7ff-151">Çözüm Gezgini'nde sağ **modelleri** seçip **Ekle | Sınıf** basit sınıf kişi (POCO) oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="9e7ff-152">Adlandırın **kişi** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="9e7ff-153">Kişi sınıfını açın ve aşağıdaki özellikleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="9e7ff-154">(Kod parçacığını - *ASP.NET MVC 4 ve kurum çerçevesi geçişlerine - Ex1 kişi özellikleri*)</span><span class="sxs-lookup"><span data-stu-id="9e7ff-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="9e7ff-155">Tıklayın **yapı | Çözüm yapı** değişiklikleri kaydedin ve projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="9e7ff-156">![Uygulama oluşturma](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "uygulama oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="9e7ff-157">*Uygulama oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-157">*Building the Application*</span></span>
7. <span data-ttu-id="9e7ff-158">Çözüm Gezgini'nde denetleyicileri klasörüne sağ tıklayıp **Ekle | Denetleyici**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="9e7ff-159">Denetleyici adı *PersonController* ve tamamlayın **yapı İskelesi seçenekleri** şu değerlere sahip.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="9e7ff-160">İçinde **şablon** aşağı açılan listesinden **okuma/yazma eylemleri ve Entity Framework kullanarak görünümler ile MVC denetleyicisi** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="9e7ff-161">İçinde **Model sınıfı** aşağı açılan listesinden **kişi** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="9e7ff-162">İçinde **veri bağlamı sınıfı** listesinden  **&lt;yeni veri bağlamı... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="9e7ff-163">Herhangi bir ad seçin ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="9e7ff-164">İçinde **görünümleri** aşağı açılan listesinde, emin **Razor** seçilir.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="9e7ff-165">![Yapı iskelesi ile kişi denetleyicisi ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "yapı iskelesi ile kişi denetleyicisi ekleme")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="9e7ff-166">*Yapı iskelesi ile kişi denetleyicisi ekleme*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="9e7ff-167">Tıklayın **Ekle** yeni denetleyici kişi için yapı iskelesi ile oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="9e7ff-168">Şimdi, denetleyici eylemlerini ve bunun yanı sıra görünümleri oluşturduktan.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="9e7ff-169">![Kişi denetleyicisi ile yapı iskelesi oluşturduktan sonra](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "kişi denetleyicisi ile yapı iskelesi oluşturduktan sonra")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="9e7ff-170">*Kişi denetleyicisi ile yapı iskelesi oluşturduktan sonra*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="9e7ff-171">Açık **PersonController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-171">Open **PersonController** class.</span></span> <span data-ttu-id="9e7ff-172">Tam CRUD eylem yöntemlerine otomatik olarak oluşturulmuş dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="9e7ff-173">![Kişi denetleyicisi içinde](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "içinde kişi denetleyicisi")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="9e7ff-174">*İçinde kişi denetleyicisi*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="9e7ff-175">Görev 2-çalışan uygulama</span><span class="sxs-lookup"><span data-stu-id="9e7ff-175">Task 2- Running the application</span></span>

<span data-ttu-id="9e7ff-176">Bu noktada, veritabanı henüz oluşturulmamış.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="9e7ff-177">Bu görevde, uygulamayı ilk kez çalıştırmak ve CRUD işlemleri test edin.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="9e7ff-178">Veritabanı hareket halindeyken Code First ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="9e7ff-179">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="9e7ff-180">Tarayıcıda ekleme **/Person** kişi sayfasını açmak için URL.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="9e7ff-181">![Uygulaması ilk çalıştırma](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "uygulaması ilk çalıştırma")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="9e7ff-182">*Uygulama: ilk çalıştırma*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-182">*Application: first run*</span></span>
3. <span data-ttu-id="9e7ff-183">Şimdi kişi sayfaları keşfedin ve CRUD işlemleri test edin.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="9e7ff-184">Tıklayın **Yeni Oluştur** yeni bir kişiye eklemek için.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="9e7ff-185">Bir ad ve soyadı girin ve tıklayın **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="9e7ff-186">![Yeni bir kişi ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "yeni bir kişi ekleme")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="9e7ff-187">*Yeni bir kişi ekleme*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="9e7ff-188">Kişinin listesinde, silmek, düzenlemek veya öğeleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="9e7ff-189">![kişi listesi](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "kişi listesi")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="9e7ff-190">*Kişi listesi*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-190">*Person list*</span></span>
    3. <span data-ttu-id="9e7ff-191">Tıklayın **ayrıntıları** kişinin ayrıntılarını açmak için.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="9e7ff-192">![Kişi ayrıntıları](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "kişinin ayrıntıları")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="9e7ff-193">*Kişi ayrıntıları*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-193">*Person's details*</span></span>
4. <span data-ttu-id="9e7ff-194">Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="9e7ff-195">Kişi varlığı için tüm CRUD - modelinden görünümlere - uygulamanızda tek satırlık bir kod yazmak zorunda kalmadan oluşturduğunuz dikkat edin!</span><span class="sxs-lookup"><span data-stu-id="9e7ff-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="9e7ff-196">Görev 3-güncelleştirme veritabanını kullanarak Entity Framework geçişleri</span><span class="sxs-lookup"><span data-stu-id="9e7ff-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="9e7ff-197">Bu görevde, varlık çerçevesi geçişleriyle kullanarak veritabanını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="9e7ff-198">Modeli değiştirmek ve kurum çerçevesi geçişlerine özelliğini kullanarak veritabanlarınızı değişiklikleri yansıtmak için ne kadar kolay olduğunu keşfeder.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="9e7ff-199">Paket Yöneticisi konsolu açın.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-199">Open the Package Manager Console.</span></span> <span data-ttu-id="9e7ff-200">Seçin **araçları | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="9e7ff-201">Paket Yöneticisi konsolunda aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="9e7ff-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="9e7ff-202">PMC</span><span class="sxs-lookup"><span data-stu-id="9e7ff-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="9e7ff-203">![Geçişleri etkinleştirme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "geçişler etkinleştirme")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="9e7ff-204">*Geçiş etkinleştiriliyor*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-204">*Enabling migrations*</span></span>

    <span data-ttu-id="9e7ff-205">Geçişi etkinleştirmeyi komut oluşturur **geçişler** veritabanı başlatmak üzere bir betiğin bulunduğu klasör.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="9e7ff-206">![Geçişleri klasör](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "geçişler klasörü")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="9e7ff-207">*Geçişleri klasörü*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-207">*Migrations folder*</span></span>
3. <span data-ttu-id="9e7ff-208">Açık **Configuration.cs** geçişler klasöründeki dosya.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="9e7ff-209">Sınıf oluşturucu bulun ve değiştirin **AutomaticMigrationsEnabled** değerini *true*.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="9e7ff-210">Kişi sınıfını açın ve kişinin ikinci adı için bir öznitelik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="9e7ff-211">Bu yeni öznitelikle modelini değiştiriyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="9e7ff-212">Seçin **yapı | Çözüm yapı** uygulamayı oluşturmak için menüde.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="9e7ff-213">![Uygulama oluşturma](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "uygulama oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="9e7ff-214">*Uygulama oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-214">*Building the application*</span></span>
6. <span data-ttu-id="9e7ff-215">Paket Yöneticisi konsolunda aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="9e7ff-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="9e7ff-216">PMC</span><span class="sxs-lookup"><span data-stu-id="9e7ff-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="9e7ff-217">Bu komut veri nesnelerini değişiklikleri arar ve ardından, veritabanını uygun şekilde değiştirmek için gerekli komutları ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="9e7ff-218">![İkinci ad ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "ikinci adı ekleme")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="9e7ff-219">*İkinci adı ekleme*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="9e7ff-220">(İsteğe bağlı) Fark güncelleştirme ile bir SQL betiği oluşturmak için aşağıdaki komutu çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="9e7ff-221">Bu, veritabanını el ile güncelleştirmenizi sağlar (Bu durumda gerekli değildir), veya diğer veritabanlarındaki değişiklikleri uygulayın:</span><span class="sxs-lookup"><span data-stu-id="9e7ff-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="9e7ff-222">PMC</span><span class="sxs-lookup"><span data-stu-id="9e7ff-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="9e7ff-223">![SQL komut dosyası oluşturuluyor](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "SQL komut dosyası oluşturuluyor")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="9e7ff-224">*SQL komut dosyası oluşturuluyor*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="9e7ff-225">![SQL betiği güncelleştirme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL betiğini güncelleştirme")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="9e7ff-226">*SQL komut dosyasını güncelleştirme*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-226">*SQL Script update*</span></span>
8. <span data-ttu-id="9e7ff-227">Paket Yöneticisi Konsolu'nda veritabanını güncelleştirmek için aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="9e7ff-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="9e7ff-228">PMC</span><span class="sxs-lookup"><span data-stu-id="9e7ff-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="9e7ff-229">![Veritabanını güncelleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "veritabanı güncelleştiriliyor")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="9e7ff-230">*Veritabanı güncelleştiriliyor*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-230">*Updating the Database*</span></span>

    <span data-ttu-id="9e7ff-231">Bu ekler **MiddleName** sütununda **kişiler** geçerli tanımı eşleştirilecek tablo **kişi** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="9e7ff-232">Veritabanı güncelleştirildikten sonra denetleyici klasörünü sağ tıklatın ve seçin **Ekle | Denetleyici** kişi denetleyicisi yeniden (aynı değerlere sahip eksiksiz) eklemek için.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="9e7ff-233">Bu, yeni bir öznitelik ekleme görünümleri ve var olan yöntemler güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="9e7ff-234">![Denetleyici güncelleştirmesi ekleme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "denetleyicisi güncelleştirme ekleme")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="9e7ff-235">*Denetleyici güncelleştiriliyor*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-235">*Updating the controller*</span></span>
10. <span data-ttu-id="9e7ff-236">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-236">Click **Add**.</span></span> <span data-ttu-id="9e7ff-237">Ardından, değerleri seçin **üzerine PersonController.cs** ve **üzerine yazma görünümleri ilişkili** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Ekleme denetleyicisi üzerine yaz](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="9e7ff-239">*Denetleyici güncelleştiriliyor*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="9e7ff-240">Task4 - uygulama çalıştırma</span><span class="sxs-lookup"><span data-stu-id="9e7ff-240">Task4- Running the application</span></span>

1. <span data-ttu-id="9e7ff-241">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="9e7ff-242">Açık **/Person**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-242">Open **/Person**.</span></span> <span data-ttu-id="9e7ff-243">İkinci Ad sütunu eklendi ancak verileri, korundu, dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="9e7ff-244">![İkinci Ad eklenen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "ortasına adı eklendi")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="9e7ff-245">*İkinci Ad eklendi*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-245">*Middle Name added*</span></span>
3. <span data-ttu-id="9e7ff-246">Tıklarsanız **Düzenle**, geçerli kişinin ikinci adı eklemek mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="9e7ff-247">![İkinci Ad edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "ikinci adı sürümü")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="9e7ff-248">Özet</span><span class="sxs-lookup"><span data-stu-id="9e7ff-248">Summary</span></span>

<span data-ttu-id="9e7ff-249">Bu uygulamalı bir laboratuvarda, basit CRUD işlemleri ile ASP.NET MVC 4 herhangi bir model sınıfı kullanarak İskele oluşturma adımlarını öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="9e7ff-250">Ardından, varlık çerçevesi geçişleriyle kullanarak uygulamanızda - veritabanından görünümler - uçtan uca bir güncelleştirme gerçekleştirmek nasıl öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="9e7ff-251">Ek A: Visual Studio'yu Web için Express 2012</span><span class="sxs-lookup"><span data-stu-id="9e7ff-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="9e7ff-252">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="9e7ff-253">Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="9e7ff-254">Git [ [ https://go.microsoft.com/? LinkId 9810169 =](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="9e7ff-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="9e7ff-255">Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="9e7ff-256">Tıklayarak **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-256">Click on **Install Now**.</span></span> <span data-ttu-id="9e7ff-257">Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="9e7ff-258">Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="9e7ff-259">![Visual Studio Express yükleyin](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Visual Studio Express'i yükle")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="9e7ff-260">*Visual Studio Express yükleyin*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="9e7ff-261">Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşullarını kabul etme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="9e7ff-263">*Lisans koşullarını kabul etme*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="9e7ff-264">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-264">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="9e7ff-266">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-266">*Installation progress*</span></span>
6. <span data-ttu-id="9e7ff-267">Yükleme tamamlandığında, tıklayın **son**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-267">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="9e7ff-269">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-269">*Installation completed*</span></span>
7. <span data-ttu-id="9e7ff-270">Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="9e7ff-271">Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Web kutucuğu için VS Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="9e7ff-273">*Web kutucuğu için VS Express*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="9e7ff-274">Ek B: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="9e7ff-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="9e7ff-275">Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="9e7ff-276">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="9e7ff-277">![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="9e7ff-278">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="9e7ff-279">***Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="9e7ff-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="9e7ff-280">Kod eklemesini istediğiniz imleci yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="9e7ff-281">(Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="9e7ff-282">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="9e7ff-283">Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="9e7ff-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="9e7ff-284">İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="9e7ff-285">![Kod parçacığı adını yazmaya başlayın](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "kod parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="9e7ff-286">*Kod parçacığı adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="9e7ff-287">![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="9e7ff-288">*Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="9e7ff-289">![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="9e7ff-290">*Yeniden Tab tuşuna basın ve kod parçacığı genişletir*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="9e7ff-291">***Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="9e7ff-292">Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="9e7ff-293">Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="9e7ff-294">Tıklayarak ilgili kod parçacığı listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="9e7ff-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="9e7ff-295">![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="9e7ff-296">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="9e7ff-297">![Tıklayarak ilgili kod parçacığını listesinden çekme](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "tıklayarak ilgili kod parçacığı listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="9e7ff-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="9e7ff-298">*Tıklayarak ilgili kod parçacığı listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="9e7ff-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
