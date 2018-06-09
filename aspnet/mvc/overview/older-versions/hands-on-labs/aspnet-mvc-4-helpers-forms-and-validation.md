---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 Yardımcıları, formlar ve doğrulama | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 modelleri ve veri erişim uygulamalı Laboratuvar, alınan yükleniyor ve veritabanındaki verileri görüntüleme. Bu uygulamalı Laboratuvar için ekleyeceksiniz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 4cfa98144919c3f1bdb3608970af1a7952fe6ea7
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30878184"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="30f63-104">ASP.NET MVC 4 Yardımcıları, formlar ve doğrulama</span><span class="sxs-lookup"><span data-stu-id="30f63-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="30f63-105">Tarafından [Web Camps ekibi](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="30f63-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="30f63-106">Kit eğitim Web Camps indirin</span><span class="sxs-lookup"><span data-stu-id="30f63-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="30f63-107">İçinde **ASP.NET MVC 4 modelleri ve veri erişimi** uygulamalı Laboratuvar yükleniyor ve veritabanındaki verileri görüntüleme olmuştur.</span><span class="sxs-lookup"><span data-stu-id="30f63-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="30f63-108">Bu uygulamalı laboratuar ortamında, ekleyecek **müzik deposu** uygulama bu verileri düzenleme özelliği.</span><span class="sxs-lookup"><span data-stu-id="30f63-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="30f63-109">Bu hedefi dikkate alarak, albümleri oluşturma, okuma, güncelleştirme ve silme (CRUD) eylemleri destekleyecek denetleyicisi ilk oluşturur.</span><span class="sxs-lookup"><span data-stu-id="30f63-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="30f63-110">Bir HTML tablosunda albümleri özelliklerini görüntülemek için ASP.NET MVC'ın iskele özelliği yararlanarak bir dizin görünümünün şablonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="30f63-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="30f63-111">Bu görünüm geliştirmek için uzun açıklamaları keser özel bir HTML Yardımcısı ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="30f63-112">Daha sonra düzenleme ve oluşturma sağlayan görünümleri bırakmalar gibi form öğelerin yardımıyla veritabanında albümleri alter ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="30f63-113">Son olarak, albümü silme kullanıcılar sağlar ve ayrıca, bunları kendi girişi doğrulama tarafından yanlış veri geçmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="30f63-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="30f63-114">Bu uygulamalı Laboratuvar temel bilgiye sahip varsayar **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="30f63-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="30f63-115">Değil kullandıysanız **ASP.NET MVC** gitmenizi öneririz önce **ASP.NET MVC Temelleri** uygulamalı Laboratuvar.</span><span class="sxs-lookup"><span data-stu-id="30f63-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="30f63-116">Bu Laboratuvar, kaynak klasöre sağlanan örnek bir Web uygulamasına küçük değişiklikler uygulayarak daha önce açıklanan yeni özellikleri ve geliştirmeleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="30f63-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="30f63-117">Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [Microsoft-Web/WebCampTrainingKit sürümleri](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="30f63-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="30f63-118">Bu Laboratuvar için belirli proje şu adresten edinilebilir [ASP.NET MVC 4 Yardımcıları, formlar ve doğrulama](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="30f63-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="30f63-119">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="30f63-119">Objectives</span></span>

<span data-ttu-id="30f63-120">Bu uygulamalı laboratuar ortamında, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="30f63-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="30f63-121">CRUD işlemleri desteklemek için bir denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="30f63-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="30f63-122">Bir HTML tablosunda varlık özellikleri görüntülemek için bir dizin Görünüm Oluştur</span><span class="sxs-lookup"><span data-stu-id="30f63-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="30f63-123">Özel bir HTML Yardımcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="30f63-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="30f63-124">Oluşturma ve düzenleme görünümü özelleştirme</span><span class="sxs-lookup"><span data-stu-id="30f63-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="30f63-125">HTTP GET veya POST HTTP çağrıları tepki eylem yöntemleri birbirinden</span><span class="sxs-lookup"><span data-stu-id="30f63-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="30f63-126">Ekleme ve Görünüm Oluştur özelleştirme</span><span class="sxs-lookup"><span data-stu-id="30f63-126">Add and customize a Create View</span></span>
- <span data-ttu-id="30f63-127">Tanıtıcı bir varlığı silme</span><span class="sxs-lookup"><span data-stu-id="30f63-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="30f63-128">Kullanıcı girişini doğrulama</span><span class="sxs-lookup"><span data-stu-id="30f63-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="30f63-129">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="30f63-129">Prerequisites</span></span>

<span data-ttu-id="30f63-130">Bu laboratuvarı tamamlamak için aşağıdaki öğeleri sahip olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="30f63-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="30f63-131">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="30f63-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="30f63-132">Kurulum</span><span class="sxs-lookup"><span data-stu-id="30f63-132">Setup</span></span>

<span data-ttu-id="30f63-133">**Kod parçacıkları yükleme**</span><span class="sxs-lookup"><span data-stu-id="30f63-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="30f63-134">Kolaylık olması için bu Laboratuvar yönetme kod çoğunu Visual Studio kod parçacıkları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="30f63-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="30f63-135">Çalıştırma kod parçacıkları yüklemek için **.\Source\Setup\CodeSnippets.vsi** dosya.</span><span class="sxs-lookup"><span data-stu-id="30f63-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="30f63-136">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek B: kullanarak kod parçacıkları](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="30f63-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="30f63-137">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="30f63-137">Exercises</span></span>

<span data-ttu-id="30f63-138">Aşağıdaki alıştırmada bu uygulamalı laboratuarı olun:</span><span class="sxs-lookup"><span data-stu-id="30f63-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="30f63-139">Mağaza Yöneticisi denetleyicisi ve onun dizini görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="30f63-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="30f63-140">Bir HTML Yardımcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="30f63-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="30f63-141">Düzenleme görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="30f63-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="30f63-142">Oluştur görünümünün ekleme</span><span class="sxs-lookup"><span data-stu-id="30f63-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="30f63-143">İşleme silme</span><span class="sxs-lookup"><span data-stu-id="30f63-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="30f63-144">Doğrulama Ekleme</span><span class="sxs-lookup"><span data-stu-id="30f63-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="30f63-145">İstemci tarafında örtük jQuery kullanma</span><span class="sxs-lookup"><span data-stu-id="30f63-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="30f63-146">Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="30f63-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="30f63-147">Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="30f63-148">Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**</span><span class="sxs-lookup"><span data-stu-id="30f63-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="30f63-149">Alıştırma 1: mağaza yöneticisi denetleyicisi ve onun dizini görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="30f63-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="30f63-150">Bu alıştırmada, CRUD işlemleri desteklemek için veritabanını ve son olarak ASP.NET MVC'nin yapı iskelesi yararlanarak bir dizin görünümünün şablonu oluşturma albümleri listesini döndürmek için dizin eylem yöntemini Özelleştir yeni bir denetleyici oluşturma öğreneceksiniz. bir HTML tablosunda albümleri özelliklerini görüntülemek için özellik.</span><span class="sxs-lookup"><span data-stu-id="30f63-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="30f63-151">Görev 1 - StoreManagerController oluşturma</span><span class="sxs-lookup"><span data-stu-id="30f63-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="30f63-152">Bu görevde adlı yeni bir denetleyici oluşturacak **StoreManagerController** CRUD işlemleri desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="30f63-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="30f63-153">Açık **başlamak** çözüm bulunan **kaynak/Ex1-CreatingTheStoreManagerController/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="30f63-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="30f63-154">Bazı eksik NuGet paketlerini indirmek gerekir devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="30f63-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="30f63-155">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="30f63-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="30f63-156">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="30f63-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="30f63-157">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="30f63-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="30f63-158">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="30f63-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="30f63-159">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="30f63-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="30f63-160">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="30f63-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="30f63-161">Yeni denetleyici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="30f63-161">Add a new controller.</span></span> <span data-ttu-id="30f63-162">Bunu yapmak için sağ **denetleyicileri** Klasör Seç Çözüm Gezgini içinde **Ekle** ve ardından **denetleyicisi** komutu.</span><span class="sxs-lookup"><span data-stu-id="30f63-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="30f63-163">Değişiklik **denetleyicisi** **adı** için **StoreManagerController** ve seçeneği emin olun **boşokuma/yazmaeylemleriileMVCdenetleyicisi**seçilir.</span><span class="sxs-lookup"><span data-stu-id="30f63-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="30f63-164">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="30f63-164">Click **Add**.</span></span>

    <span data-ttu-id="30f63-165">![Ekle denetleyicisi iletişim](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Ekle denetleyicisi iletişim")</span><span class="sxs-lookup"><span data-stu-id="30f63-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="30f63-166">*Denetleyici Ekle iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="30f63-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="30f63-167">Yeni bir denetleyici sınıfı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="30f63-167">A new Controller class is generated.</span></span> <span data-ttu-id="30f63-168">Okuma/yazma, kişiler, saplama yöntemleri için Eylemler eklemek için belirtilen beri genel CRUD eylemleri uygulama belirli mantığı eklemek isteyen doldurulur Yapılacaklar açıklamaları ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="30f63-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="30f63-169">Görev 2 - StoreManager dizinini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="30f63-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="30f63-170">Bu görevde, veritabanından albümleri listesiyle birlikte bir görünüme dönmek için StoreManager dizin eylem yönteminin özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="30f63-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="30f63-171">Aşağıdaki StoreManagerController sınıfında ekleyin *kullanarak* yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="30f63-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="30f63-172">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex1 MvcMusicStore kullanarak*)</span><span class="sxs-lookup"><span data-stu-id="30f63-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="30f63-173">Bir alan eklemek **StoreManagerController** örneği tutmak için **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="30f63-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="30f63-174">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="30f63-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="30f63-175">Albümleri listesiyle birlikte bir görünüme dönmek için StoreManagerController dizin eylemi uygulayın.</span><span class="sxs-lookup"><span data-stu-id="30f63-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="30f63-176">Denetleyici eylem mantığı StoreController'ın dizin eylem için daha önce yazılmış çok benzer.</span><span class="sxs-lookup"><span data-stu-id="30f63-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="30f63-177">LINQ tarz ve sanatçı bilgilerini görüntülemek için de dahil olmak üzere tüm albümleri almak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="30f63-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="30f63-178">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex1 StoreManagerController dizin*)</span><span class="sxs-lookup"><span data-stu-id="30f63-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="30f63-179">Görev 3 - dizin görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="30f63-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="30f63-180">Bu görevde tarafından döndürülen albümleri listesini görüntülemek için dizin görünümünün şablonu oluşturacak **StoreManager** denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="30f63-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="30f63-181">Yeni Görünüm şablonu oluşturmadan önce projeyi derlemek böylece **Ekle iletişim kutusunu görüntüle** bildiği **albüm** kullanılacak sınıfı.</span><span class="sxs-lookup"><span data-stu-id="30f63-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="30f63-182">Seçin **yapı | MvcMusicStore derleme** Projeyi derlemek için.</span><span class="sxs-lookup"><span data-stu-id="30f63-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="30f63-183">İçinde sağ **dizin** eylem yöntemi ve select **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="30f63-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="30f63-184">Bu getirir **Görünüm Ekle** iletişim.</span><span class="sxs-lookup"><span data-stu-id="30f63-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="30f63-185">![Görünüm Ekle](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="30f63-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="30f63-186">*Dizin yöntemi içinden bir görünümle ekleme*</span><span class="sxs-lookup"><span data-stu-id="30f63-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="30f63-187">Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **dizin**.</span><span class="sxs-lookup"><span data-stu-id="30f63-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="30f63-188">Seçin **kesin türü belirtilmiş görünüm oluşturma** seçeneğini ve **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır.</span><span class="sxs-lookup"><span data-stu-id="30f63-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="30f63-189">Seçin **listesi** gelen **İskele şablonu** açılır.</span><span class="sxs-lookup"><span data-stu-id="30f63-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="30f63-190">Bırakın **görünüm altyapısı** için **Razor** ve diğer alanları varsayılan ile değer ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="30f63-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="30f63-191">![Bir dizini görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "bir dizini görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="30f63-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="30f63-192">*Bir dizini görünümü ekleme*</span><span class="sxs-lookup"><span data-stu-id="30f63-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="30f63-193">Görev 4 - dizin görünümünün iskele özelleştirme</span><span class="sxs-lookup"><span data-stu-id="30f63-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="30f63-194">Bu görevde, istediğiniz alanları görüntülemek için ASP.NET MVC yapı iskelesi özelliğiyle oluşturulmuş basit görünüm şablonu ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="30f63-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="30f63-195">**İskele** ASP.NET MVC içinde desteği albüm modelindeki tüm alanları listeleyen basit bir görünüm şablon oluşturur.</span><span class="sxs-lookup"><span data-stu-id="30f63-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="30f63-196">**İskele** kesin türü belirtilmiş bir görünüm üzerinde çalışmaya başlamak için hızlı bir yol sunar: görünüm şablonu el ile yazmak yerine hızlı bir şekilde iskele varsayılan şablon oluşturur ve oluşturulan kod daha sonra değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="30f63-197">Oluşturulan kod gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="30f63-197">Review the code created.</span></span> <span data-ttu-id="30f63-198">Alan listesinin oluşturulmasını aşağıdaki parçası olacak HTML tablo **İskele** tablo veri görüntülemek için kullanma.</span><span class="sxs-lookup"><span data-stu-id="30f63-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="30f63-199">Değiştir **&lt;tablo&gt;** yalnızca görüntülemek için aşağıdaki kodu koduyla **Tarz**, **sanatçı**, **albüm başlığı**, ve **fiyat** alanları.</span><span class="sxs-lookup"><span data-stu-id="30f63-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="30f63-200">Bu siler **AlbumId** ve **albüm resim URL'si** sütun.</span><span class="sxs-lookup"><span data-stu-id="30f63-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="30f63-201">Ayrıca, kullanıcıların bağlı sınıf özelliklerini görüntülemek için GenreId ve ArtistId sütunları değiştirir **Artist.Name** ve **Genre.Name**ve kaldırır **ayrıntıları** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="30f63-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="30f63-202">Aşağıdaki açıklamaları değiştirin.</span><span class="sxs-lookup"><span data-stu-id="30f63-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="30f63-203">Görev 5 - Uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="30f63-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="30f63-204">Bu görevde, sınayacak **StoreManager** **dizin** şablonu görüntüleme önceki adımları tasarımını göre Albümler listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="30f63-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="30f63-205">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="30f63-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="30f63-206">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="30f63-206">The project starts in the Home page.</span></span> <span data-ttu-id="30f63-207">URL'ye değiştirin **/StoreManager** gösteren albümleri listesini görüntülendiğini doğrulamak için kendi **başlık**, **sanatçı** ve **Tarz**.</span><span class="sxs-lookup"><span data-stu-id="30f63-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="30f63-208">![Albümleri listesi gözatma](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "albümleri listesi gözatma")</span><span class="sxs-lookup"><span data-stu-id="30f63-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="30f63-209">*Albümleri listesi gözatma*</span><span class="sxs-lookup"><span data-stu-id="30f63-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="30f63-210">Alıştırma 2: bir HTML Yardımcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="30f63-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="30f63-211">StoreManager dizin sayfası olası bir sorunu var: Başlık ve sanatçı ad özellikleri her ikisi de olabilir tablosunu biçimlendirme kapalı throw yetecek kadar uzun.</span><span class="sxs-lookup"><span data-stu-id="30f63-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="30f63-212">Bu alıştırmada, metnin kesmek için özel bir HTML Yardımcısı eklemek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="30f63-213">Aşağıdaki şekilde, bir küçük tarayıcı boyutu kullandığınızda biçimi metnin uzunluğu nedeniyle nasıl değiştirilir görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="30f63-214">![İle albümleri listesi gözatma değil metin kesilmiş](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "ile albümleri listesi gözatma değil metin kesildi")</span><span class="sxs-lookup"><span data-stu-id="30f63-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="30f63-215">*İle albümleri listesi gözatma metin kesilmiş değil*</span><span class="sxs-lookup"><span data-stu-id="30f63-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="30f63-216">Görev 1 - HTML Yardımcısı genişletme</span><span class="sxs-lookup"><span data-stu-id="30f63-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="30f63-217">Bu görevde, yeni bir yöntem ekleyecek **Truncate** için **HTML** ASP.NET MVC görünümler içinde sunulan nesne.</span><span class="sxs-lookup"><span data-stu-id="30f63-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="30f63-218">Bunu yapmak için gerçekleştireceksiniz bir **genişletme yöntemi** yerleşik için **System.Web.Mvc.HtmlHelper** ASP.NET MVC tarafından sağlanan sınıfı.</span><span class="sxs-lookup"><span data-stu-id="30f63-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="30f63-219">Hakkında daha fazla bilgi edinmek için **genişletme yöntemleri**, lütfen bu msdn makalesine bakın.</span><span class="sxs-lookup"><span data-stu-id="30f63-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="30f63-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="30f63-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="30f63-221">Açık **başlamak** çözüm bulunan **kaynak/Ex2-AddingAnHTMLHelper/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="30f63-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="30f63-222">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="30f63-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="30f63-223">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="30f63-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="30f63-224">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="30f63-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="30f63-225">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="30f63-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="30f63-226">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="30f63-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="30f63-227">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="30f63-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="30f63-228">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="30f63-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="30f63-229">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="30f63-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="30f63-230">StoreManager'ın dizini görünümünü açın.</span><span class="sxs-lookup"><span data-stu-id="30f63-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="30f63-231">Bunu yapmak için Çözüm Gezgini'nde genişletin **görünümleri** klasörü, sonra **StoreManager** açarak **Index.cshtml** dosya.</span><span class="sxs-lookup"><span data-stu-id="30f63-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="30f63-232">Aşağıdaki kodu ekleyin <strong>@model</strong> tanımlamak için yönergesi <strong>Truncate</strong> yardımcı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="30f63-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="30f63-233">Görev 2 - metnin sayfadaki kesiliyor</span><span class="sxs-lookup"><span data-stu-id="30f63-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="30f63-234">Bu görevde, kullanacağınız **Truncate** şablonu görüntüleme metni kesmek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="30f63-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="30f63-235">StoreManager'ın dizini görünümünü açın.</span><span class="sxs-lookup"><span data-stu-id="30f63-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="30f63-236">Bunu yapmak için Çözüm Gezgini'nde genişletin **görünümleri** klasörü, sonra **StoreManager** açarak **Index.cshtml** dosya.</span><span class="sxs-lookup"><span data-stu-id="30f63-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="30f63-237">Göster satırları değiştirmek **sanatçı adı** ve albüm **başlık**.</span><span class="sxs-lookup"><span data-stu-id="30f63-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="30f63-238">Bunu yapmak için aşağıdaki satırları değiştirin.</span><span class="sxs-lookup"><span data-stu-id="30f63-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="30f63-239">Görev 3 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="30f63-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="30f63-240">Bu görevde, sınayacak **StoreManager** **dizin** şablonu görüntüleme albüm başlık ve sanatçı adı tamsayıya dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="30f63-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="30f63-241">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="30f63-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="30f63-242">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="30f63-242">The project starts in the Home page.</span></span> <span data-ttu-id="30f63-243">URL'ye değiştirin **/StoreManager** bu uzun doğrulamak için metinleri **başlık** ve **sanatçı** sütun kesilir.</span><span class="sxs-lookup"><span data-stu-id="30f63-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="30f63-244">![Kesilen başlıkları ve Sanatçılar adları](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "kesilmiş başlıkları ve Sanatçılar adları")</span><span class="sxs-lookup"><span data-stu-id="30f63-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="30f63-245">*Kesilmiş başlıklarını ve sanatçı adları*</span><span class="sxs-lookup"><span data-stu-id="30f63-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="30f63-246">Alıştırma 3: düzenleme görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="30f63-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="30f63-247">Bu alıştırmada, albüm düzenlemek Mağaza yöneticileri izin vermek için bir formun nasıl oluşturulacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="30f63-248">Gözat **/StoreManager/Edit/id** URL (**kimliği** düzenlemek için albümü benzersiz kimliğini olan), böylece sunucusuna bir HTTP GET çağrısı yapma.</span><span class="sxs-lookup"><span data-stu-id="30f63-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="30f63-249">Denetleyici Düzenle eylem yöntemi uygun albüm veritabanından, oluşturma bir **StoreManagerViewModel** nesne (listesini Sanatçılar ve türler ile birlikte) kapsüllemek ve daha sonra bir görünüm şablonuna geçirin için HTML sayfası kullanıcıya geri işlenemiyor.</span><span class="sxs-lookup"><span data-stu-id="30f63-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="30f63-250">Bu sayfayı içerecek bir **&lt;form&gt;** metin kutuları ve albüm özelliklerini düzenlemek için bırakmalar öğe.</span><span class="sxs-lookup"><span data-stu-id="30f63-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="30f63-251">Kullanıcı albüm form değerleri güncelleştirir ve tıklar sonra **kaydetmek** düğmesi, değişiklikleri aracılığıyla gönderilir bir HTTP POST geri çağrı **/StoreManager/Edit/id**. URL son çağrı olduğu gibi aynı kalsa da, ASP.NET MVC, bir HTTP POST ve bu nedenle farklı bir düzen eylem yöntemi yürütülmeden bu kez tanımlar (bir donatılmış ile **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="30f63-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="30f63-252">Görev 1 - HTTP GET düzenleme eylem yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="30f63-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="30f63-253">Bu görevde, uygun albüm veritabanından almak için düzenleme eylem yöntemini HTTP GET sürümü yanı sıra, tüm türler ve Sanatçılar listesini gerçekleştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="30f63-254">Bu veriler içine paketi **StoreManagerViewModel** ardından yanıtı işlemek için bir şablonu görüntüleme geçirilecektir son adımda tanımlanan nesne.</span><span class="sxs-lookup"><span data-stu-id="30f63-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="30f63-255">Açık **başlamak** çözüm bulunan **kaynak/Ex3-CreatingTheEditView/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="30f63-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="30f63-256">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="30f63-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="30f63-257">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="30f63-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="30f63-258">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="30f63-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="30f63-259">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="30f63-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="30f63-260">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="30f63-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="30f63-261">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="30f63-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="30f63-262">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="30f63-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="30f63-263">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="30f63-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="30f63-264">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="30f63-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="30f63-265">Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="30f63-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="30f63-266">Değiştir **HTTP GET Düzenle** uygun almak için aşağıdaki kodu eylem yöntemiyle **albüm** yanı sıra **türler** ve **Sanatçılar**listeler.</span><span class="sxs-lookup"><span data-stu-id="30f63-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="30f63-267">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex3 StoreManagerController HTTP GET Düzenle eylem*)</span><span class="sxs-lookup"><span data-stu-id="30f63-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="30f63-268">Kullanmakta olduğunuz **System.Web.Mvc** **SelectList** Sanatçılar ve yerine türler için **System.Collections.Generic** listesi.</span><span class="sxs-lookup"><span data-stu-id="30f63-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="30f63-269">**SelectList** HTML bırakmalar doldurmak ve geçerli seçim gibi şeyleri yönetmek için temiz bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="30f63-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="30f63-270">Örnek oluşturma ve bu denetleyici eylemini ViewModel nesneleri sonraki ayarlama düzenleme form senaryo temizleyici hale getirir.</span><span class="sxs-lookup"><span data-stu-id="30f63-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="30f63-271">Görev 2 - düzenleme görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="30f63-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="30f63-272">Bu görevde, daha sonra Albüm özellikleri görüntüleyecek bir görünümü Düzenle şablonu oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="30f63-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="30f63-273">Düzenleme görünümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="30f63-273">Create the Edit View.</span></span> <span data-ttu-id="30f63-274">İçinde Bunu yapmak için sağ **Düzenle** eylem yöntemi ve select **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="30f63-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="30f63-275">Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="30f63-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="30f63-276">Denetleyin **kesin türü belirtilmiş görünüm oluşturmak** onay kutusunu seçip **albüm (MvcMusicStore.Models)** gelen **görüntülemek veri sınıfı** açılır.</span><span class="sxs-lookup"><span data-stu-id="30f63-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="30f63-277">Seçin **Düzenle** gelen **İskele şablonu** açılır.</span><span class="sxs-lookup"><span data-stu-id="30f63-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="30f63-278">Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="30f63-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="30f63-279">![Düzenleme görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "düzenleme görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="30f63-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="30f63-280">*Düzenleme görünümü ekleme*</span><span class="sxs-lookup"><span data-stu-id="30f63-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="30f63-281">Görev 3 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="30f63-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="30f63-282">Bu görevde, sınayacak **StoreManager** **Düzenle** görünüm sayfası parametre olarak geçirilen albüm özelliklerin değerlerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="30f63-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="30f63-283">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="30f63-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="30f63-284">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="30f63-284">The project starts in the Home page.</span></span> <span data-ttu-id="30f63-285">URL'ye değiştirin **/StoreManager/Edit/1** geçirilen albüm özelliklerin değerlerini görüntülendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="30f63-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="30f63-286">![Albüm düzenleme görünümü gözatma](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "albüm düzenleme görünümü gözatma")</span><span class="sxs-lookup"><span data-stu-id="30f63-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="30f63-287">*Albüm düzenleme görünümü gözatma*</span><span class="sxs-lookup"><span data-stu-id="30f63-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="30f63-288">Görev 4 - aşağı açılan listeler albüm Düzenleyicisi şablonu uygulama</span><span class="sxs-lookup"><span data-stu-id="30f63-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="30f63-289">Sanatçılar ve türler listesinden seçebilmeniz için bu görevde, aşağı açılan listeler son görevde oluşturulan görünüm şablona ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="30f63-290">Tüm değiştirmek **albüm** fieldset kodu aşağıdakilerle:</span><span class="sxs-lookup"><span data-stu-id="30f63-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="30f63-291">Bir **Html.DropDownList** Yardımcısı, Sanatçılar ve türler seçmek için aşağı açılan listeler işlemek için eklendi.</span><span class="sxs-lookup"><span data-stu-id="30f63-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="30f63-292">Parametreleri geçirilen **Html.DropDownList** şunlardır:</span><span class="sxs-lookup"><span data-stu-id="30f63-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="30f63-293">Form alanı adını (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="30f63-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="30f63-294">**SelectList** aşağı açılan değer.</span><span class="sxs-lookup"><span data-stu-id="30f63-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="30f63-295">Görev 5 - Uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="30f63-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="30f63-296">Bu görevde, sınayacak **StoreManager** **Düzenle** görünüm sayfası aşağı açılan listeler sanatçı ve Tarz kimliği metin alanları yerine görüntüler.</span><span class="sxs-lookup"><span data-stu-id="30f63-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="30f63-297">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="30f63-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="30f63-298">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="30f63-298">The project starts in the Home page.</span></span> <span data-ttu-id="30f63-299">URL'ye değiştirin **/StoreManager/Edit/1** aşağı açılan listeler sanatçı ve Tarz kimliği metin alanları yerine görüntülediğini doğrulamak üzere.</span><span class="sxs-lookup"><span data-stu-id="30f63-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="30f63-300">![Albüm Görünümü Düzenle açılan listeleri ile tarama](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "gözatma albüm düzenleme görünümü açılan listeleri")</span><span class="sxs-lookup"><span data-stu-id="30f63-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="30f63-301">*Albüm düzenleme görünümü, bu kez bırakmalar gözatma*</span><span class="sxs-lookup"><span data-stu-id="30f63-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="30f63-302">Görev 6 - HTTP POST Düzenle eylem yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="30f63-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="30f63-303">Görünümü Düzenle beklendiği gibi görüntüler, albümü yapılan değişiklikleri kaydetmek için HTTP POST Düzenle eylem yöntemi uygulamak için gerekir.</span><span class="sxs-lookup"><span data-stu-id="30f63-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="30f63-304">Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="30f63-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="30f63-305">Açık **StoreManagerController** gelen **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="30f63-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="30f63-306">Değiştir **HTTP POST Düzenle** eylem yöntemini aşağıdaki kodla (değiştirilmelidir yöntemi iki parametre alan aşırı yüklenmiş sürümü olduğunu unutmayın):</span><span class="sxs-lookup"><span data-stu-id="30f63-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="30f63-307">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex3 StoreManagerController HTTP POST Düzenle eylem*)</span><span class="sxs-lookup"><span data-stu-id="30f63-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="30f63-308">Bu yöntem kullanıcı tıklattığında yürütülür **kaydetmek** görünümünün düğmesi ve bir HTTP veritabanında kalıcı hale getirmek için POST form değerlerinin sunucu gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="30f63-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="30f63-309">Oluşturma öğesi **[HttpPost]** yöntemi bu HTTP POST senaryoları için kullanılması gerektiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="30f63-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="30f63-310">Yöntem alır bir **albüm** nesnesi.</span><span class="sxs-lookup"><span data-stu-id="30f63-310">The method takes an **Album** object.</span></span> <span data-ttu-id="30f63-311">ASP.NET MVC otomatik olarak oluşturacak albüm nesne gönderilen &lt;form&gt; değerleri.</span><span class="sxs-lookup"><span data-stu-id="30f63-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="30f63-312">Yöntemi, aşağıdaki adımları gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="30f63-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="30f63-313">Model geçerliyse:</span><span class="sxs-lookup"><span data-stu-id="30f63-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="30f63-314">Değiştirilen bir nesne olarak işaretlemek için bağlamda albüm giriş güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="30f63-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="30f63-315">Değişiklikleri kaydetmek ve dizin görünümüne yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="30f63-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="30f63-316">Model geçerli değilse, Görünüm Paketi ile doldurulacaktır **GenreId** ve **ArtistId**, izin vermek için alınan albüm nesnenin görünümüyle döndürecektir gerekli herhangi bir güncelleştirme gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="30f63-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="30f63-317">7 - uygulama çalışan görev</span><span class="sxs-lookup"><span data-stu-id="30f63-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="30f63-318">Bu görevde, sınayacak **StoreManager düzenleme** görünüm sayfası veritabanında gerçekten güncelleştirilmiş albüm verileri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="30f63-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="30f63-319">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="30f63-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="30f63-320">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="30f63-320">The project starts in the Home page.</span></span> <span data-ttu-id="30f63-321">URL'ye değiştirin **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="30f63-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="30f63-322">Albüm başlığını değiştirme **yük** ve tıklayın **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="30f63-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="30f63-323">Albüm başlık gerçekte Albümler listesinde değiştiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="30f63-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="30f63-324">![Albüm güncelleştirme](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "albüm güncelleştiriliyor")</span><span class="sxs-lookup"><span data-stu-id="30f63-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="30f63-325">*Albüm güncelleştiriliyor*</span><span class="sxs-lookup"><span data-stu-id="30f63-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="30f63-326">Alıştırma 4: oluşturma görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="30f63-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="30f63-327">Şimdi **StoreManagerController** destekleyen **Düzenle** özelliği, depolama sağlamak için Create VIEW şablon eklemek nasıl öğrenin Bu alıştırmada yöneticileri uygulamaya yeni Albümler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="30f63-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="30f63-328">Düzenleme işlevselliği ile yaptığınız gibi içinde iki ayrı yöntemlerini kullanarak oluşturma senaryosu gerçekleştireceksiniz **StoreManagerController** sınıfı:</span><span class="sxs-lookup"><span data-stu-id="30f63-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="30f63-329">Bir eylem yöntemi, boş bir form görüntülenir, Mağaza yöneticileri ilk ziyaret ettiğinizde **/StoreManager/oluşturma** URL.</span><span class="sxs-lookup"><span data-stu-id="30f63-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="30f63-330">İkinci bir eylem yöntemi, burada mağaza yöneticisi tıklar senaryo işleyecek **kaydetmek** düğmesini formda ve değerleri yeniden gönderir **/StoreManager/oluşturma** URL bir HTTP POST olarak.</span><span class="sxs-lookup"><span data-stu-id="30f63-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="30f63-331">Görev 1 - HTTP GET oluşturma eylem yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="30f63-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="30f63-332">Bu görevde, tüm türler ve Sanatçılar listesini almak için içine bu veri paketini oluşturma eylem yönteminin HTTP GET sürümünü uygulayacak bir **StoreManagerViewModel** sonra şablonu görüntüleme için geçirilen nesne.</span><span class="sxs-lookup"><span data-stu-id="30f63-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="30f63-333">Açık **başlamak** çözüm bulunan **kaynak/Ex4-AddingACreateView/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="30f63-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="30f63-334">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="30f63-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="30f63-335">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="30f63-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="30f63-336">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="30f63-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="30f63-337">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="30f63-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="30f63-338">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="30f63-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="30f63-339">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="30f63-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="30f63-340">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="30f63-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="30f63-341">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="30f63-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="30f63-342">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="30f63-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="30f63-343">Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="30f63-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="30f63-344">Değiştir **oluşturma** eylem yöntemini aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="30f63-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="30f63-345">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex4 StoreManagerController HTTP GET oluşturma eylem*)</span><span class="sxs-lookup"><span data-stu-id="30f63-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="30f63-346">Görev 2 - oluşturma görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="30f63-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="30f63-347">Bu görevde, yeni (boş) albüm form görüntüler Create VIEW şablon ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="30f63-348">İçinde sağ **oluşturma** eylem yöntemi ve select **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="30f63-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="30f63-349">Bu Görünüm Ekle iletişim kutusunu getirir.</span><span class="sxs-lookup"><span data-stu-id="30f63-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="30f63-350">Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="30f63-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="30f63-351">Seçin **kesin türü belirtilmiş görünüm oluşturma** seçeneğini ve **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır ve **Oluştur** gelen **İskele şablonu** açılır.</span><span class="sxs-lookup"><span data-stu-id="30f63-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="30f63-352">Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="30f63-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="30f63-353">![Oluştur görünümünün ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "ekleme-a-create-view.png")</span><span class="sxs-lookup"><span data-stu-id="30f63-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="30f63-354">*Oluştur görünümünün ekleme*</span><span class="sxs-lookup"><span data-stu-id="30f63-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="30f63-355">Güncelleştirme **GenreId** ve **ArtistId** alanları aşağıda gösterildiği gibi açılır listesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="30f63-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="30f63-356">Görev 3 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="30f63-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="30f63-357">Bu görevde, sınayacak **StoreManager** **oluşturma** görünüm sayfası boş bir albüm formu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="30f63-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="30f63-358">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="30f63-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="30f63-359">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="30f63-359">The project starts in the Home page.</span></span> <span data-ttu-id="30f63-360">URL'ye değiştirin **oluşturma/StoreManager/**.</span><span class="sxs-lookup"><span data-stu-id="30f63-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="30f63-361">Yeni albüm özellikleri doldurmak için boş bir form görüntülendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="30f63-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="30f63-362">![Boş bir formla görünümü oluşturma](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "boş bir formla Görünüm Oluştur")</span><span class="sxs-lookup"><span data-stu-id="30f63-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="30f63-363">*Boş bir formla görünümü oluşturma*</span><span class="sxs-lookup"><span data-stu-id="30f63-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="30f63-364">Görev 4 - HTTP POST uygulama oluşturma eylemini yöntemi</span><span class="sxs-lookup"><span data-stu-id="30f63-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="30f63-365">Bu görevde, bir kullanıcı tıkladığında çağrılacak oluşturma eylem yöntemine HTTP POST sürümü gerçekleştireceksiniz **kaydetmek** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="30f63-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="30f63-366">Yöntem yeni albümü veritabanında kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="30f63-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="30f63-367">Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="30f63-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="30f63-368">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="30f63-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="30f63-369">Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="30f63-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="30f63-370">Değiştir **HTTP POST oluşturmak** eylem yöntemini aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="30f63-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="30f63-371">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex4 StoreManagerController HTTP POST Oluştur eylemi*)</span><span class="sxs-lookup"><span data-stu-id="30f63-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="30f63-372">Oluşturma eylemini önceki düzenleme eylem yöntemine oldukça benzer, ancak nesne değiştirilmiş olarak ayarlamak yerine, bu bağlamda ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="30f63-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="30f63-373">Görev 5 - Uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="30f63-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="30f63-374">Bu görevde, sınayacak **StoreManager oluşturma** görünüm sayfası, yeni albümü oluşturmanıza olanak tanır ve StoreManager dizin görünümüne yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="30f63-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="30f63-375">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="30f63-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="30f63-376">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="30f63-376">The project starts in the Home page.</span></span> <span data-ttu-id="30f63-377">URL'ye değiştirin **oluşturma/StoreManager/**.</span><span class="sxs-lookup"><span data-stu-id="30f63-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="30f63-378">Tüm form alanlarını, aşağıdaki şekilde bir gibi yeni bir albüm için verilerle doldurun:</span><span class="sxs-lookup"><span data-stu-id="30f63-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="30f63-379">![Albüm oluşturma](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "albüm oluşturma")</span><span class="sxs-lookup"><span data-stu-id="30f63-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="30f63-380">*Albüm oluşturma*</span><span class="sxs-lookup"><span data-stu-id="30f63-380">*Creating an Album*</span></span>
3. <span data-ttu-id="30f63-381">Yeni oluşturduğunuz yeni albümü içeren StoreManager dizini görünümünü yönlendirilirsiniz doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="30f63-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="30f63-382">![Oluşturulan yeni albümü](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "oluşturulan yeni albümü")</span><span class="sxs-lookup"><span data-stu-id="30f63-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="30f63-383">*Oluşturulan yeni albümü*</span><span class="sxs-lookup"><span data-stu-id="30f63-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="30f63-384">Alıştırma 5: Silme işleme</span><span class="sxs-lookup"><span data-stu-id="30f63-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="30f63-385">Albümleri silme becerisine henüz uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="30f63-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="30f63-386">Bu alıştırmada hakkında olacaktır budur.</span><span class="sxs-lookup"><span data-stu-id="30f63-386">This is what this exercise will be about.</span></span> <span data-ttu-id="30f63-387">İçinde iki ayrı yöntemlerini kullanarak silme senaryoyu önce gerçekleştireceksiniz gibi **StoreManagerController** sınıfı:</span><span class="sxs-lookup"><span data-stu-id="30f63-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="30f63-388">Bir eylem yöntemi bir onay form görüntülenir</span><span class="sxs-lookup"><span data-stu-id="30f63-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="30f63-389">İkinci bir eylem yöntemi form gönderme işleyecek</span><span class="sxs-lookup"><span data-stu-id="30f63-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="30f63-390">Görev 1 - HTTP GET silme eylem yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="30f63-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="30f63-391">Bu görevde, albüm bilgileri almak için Delete eylem yöntemini HTTP GET sürümü gerçekleştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="30f63-392">Açık **başlamak** çözüm bulunan **kaynak/Ex5-HandlingDeletion/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="30f63-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="30f63-393">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="30f63-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="30f63-394">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="30f63-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="30f63-395">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="30f63-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="30f63-396">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="30f63-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="30f63-397">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="30f63-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="30f63-398">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="30f63-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="30f63-399">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="30f63-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="30f63-400">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="30f63-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="30f63-401">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="30f63-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="30f63-402">Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="30f63-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="30f63-403">Delete denetleyici eylemi tam olarak önceki deposu ayrıntıları denetleyici eylemi aynıdır: Bu sorgular **albüm** kullanarak veritabanını nesnesinin **kimliği** sağlanan URL ve döndürür uygun **Görünüm**.</span><span class="sxs-lookup"><span data-stu-id="30f63-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="30f63-404">Bunu yapmak için HTTP GET yerini **silmek** eylem yöntemini aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="30f63-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="30f63-405">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex5 işleme silme HTTP GET silme eylem*)</span><span class="sxs-lookup"><span data-stu-id="30f63-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="30f63-406">İçinde sağ **silmek** eylem yöntemi ve select **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="30f63-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="30f63-407">Bu Görünüm Ekle iletişim kutusunu getirir.</span><span class="sxs-lookup"><span data-stu-id="30f63-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="30f63-408">Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **silmek**.</span><span class="sxs-lookup"><span data-stu-id="30f63-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="30f63-409">Seçin **kesin türü belirtilmiş görünüm oluşturma** seçeneğini ve **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır.</span><span class="sxs-lookup"><span data-stu-id="30f63-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="30f63-410">Seçin **silmek** gelen **İskele şablonu** açılır.</span><span class="sxs-lookup"><span data-stu-id="30f63-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="30f63-411">Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="30f63-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="30f63-412">![Delete görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Delete görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="30f63-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="30f63-413">*Delete görünümü ekleme*</span><span class="sxs-lookup"><span data-stu-id="30f63-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="30f63-414">Delete şablonu modelden tüm alanları gösterir.</span><span class="sxs-lookup"><span data-stu-id="30f63-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="30f63-415">Yalnızca albüm başlık gösterilir.</span><span class="sxs-lookup"><span data-stu-id="30f63-415">You will show only the album's title.</span></span> <span data-ttu-id="30f63-416">Bunu yapmak için görünümü içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="30f63-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="30f63-417">Görev 2 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="30f63-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="30f63-418">Bu görevde, sınayacak **StoreManager** **silmek** görünüm sayfası onay silme form görüntüler.</span><span class="sxs-lookup"><span data-stu-id="30f63-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="30f63-419">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="30f63-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="30f63-420">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="30f63-420">The project starts in the Home page.</span></span> <span data-ttu-id="30f63-421">URL'ye değiştirin **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="30f63-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="30f63-422">Tıklayarak silmek için bir albümü seçmek **silmek** ve yeni görünüm yüklendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="30f63-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="30f63-423">![Albüm silme](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "albümü silme")</span><span class="sxs-lookup"><span data-stu-id="30f63-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="30f63-424">*Albüm silme*</span><span class="sxs-lookup"><span data-stu-id="30f63-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="30f63-425">Görev 3-HTTP POST silme eylem yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="30f63-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="30f63-426">Bu görevde, kullanıcı tıkladığında çağrılacak Delete eylem yöntemini HTTP POST sürümü gerçekleştireceksiniz **silmek** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="30f63-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="30f63-427">Yöntemi veritabanında albüm silmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="30f63-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="30f63-428">Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="30f63-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="30f63-429">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="30f63-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="30f63-430">Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="30f63-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="30f63-431">Değiştir **HTTP POST silme** eylem yöntemini aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="30f63-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="30f63-432">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex5 işleme silme HTTP POST silme eylem*)</span><span class="sxs-lookup"><span data-stu-id="30f63-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="30f63-433">Görev 4 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="30f63-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="30f63-434">Bu görevde, sınayacak **StoreManager Delete** görünüm sayfası albüm silmenize olanak sağlar ve StoreManager dizin görünümüne yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="30f63-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="30f63-435">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="30f63-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="30f63-436">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="30f63-436">The project starts in the Home page.</span></span> <span data-ttu-id="30f63-437">URL'ye değiştirin **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="30f63-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="30f63-438">Tıklayarak silmek için bir albümü seçmek **silin.**</span><span class="sxs-lookup"><span data-stu-id="30f63-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="30f63-439">Silme işlemini onaylamak **silmek** düğmesi:</span><span class="sxs-lookup"><span data-stu-id="30f63-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="30f63-440">![Albüm silme](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "albümü silme")</span><span class="sxs-lookup"><span data-stu-id="30f63-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="30f63-441">*Albüm silme*</span><span class="sxs-lookup"><span data-stu-id="30f63-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="30f63-442">İçinde görünmediğinden albümü silindiğini doğrulayın **dizin** sayfası.</span><span class="sxs-lookup"><span data-stu-id="30f63-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="30f63-443">Alıştırma 6: Doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="30f63-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="30f63-444">Şu anda kullandığınız var oluşturma ve düzenleme forms her türlü doğrulama gerçekleştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="30f63-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="30f63-445">Kullanıcı fiyat alanında gerekli alan boş veya harflerini yazın ayrılsa karşılaşırsınız ilk hata veritabanından olacaktır.</span><span class="sxs-lookup"><span data-stu-id="30f63-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="30f63-446">Veri ek açıklamaları model sınıfınıza ekleyerek uygulamaya doğrulama ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="30f63-447">ASP.NET MVC, zorlama ve kullanıcılara uygun ileti görüntüleme ilgilenebilmek ve veri ek açıklamaları model özelliklerinizi uygulanan istediğiniz kuralları açıklayan izin verin.</span><span class="sxs-lookup"><span data-stu-id="30f63-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="30f63-448">Görev 1 - veri ek açıklamaları ekleme</span><span class="sxs-lookup"><span data-stu-id="30f63-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="30f63-449">Bu görevde, doğrulama iletisi uygun olduğunda veri ek açıklamaları oluşturma ve düzenleme sayfa albüm modeline görüntülemek ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="30f63-450">Basit bir Model sınıfı için bir veri ek açıklama ekleme yalnızca ekleyerek işlenir bir **kullanarak** bildirimi **System.ComponentModel.DataAnnotation**, sonra yerleştirerek bir **[gerekli]** uygun özellikleri özniteliği.</span><span class="sxs-lookup"><span data-stu-id="30f63-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="30f63-451">Aşağıdaki örnek yapacağı **adı** özelliği görünümünde gerekli bir alan.</span><span class="sxs-lookup"><span data-stu-id="30f63-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="30f63-452">Varlık veri modeli oluşturulduğu bu uygulama gibi durumlarda biraz daha karmaşık budur.</span><span class="sxs-lookup"><span data-stu-id="30f63-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="30f63-453">Veri ek açıklamaları model sınıflarına doğrudan eklediyseniz, modeli veritabanından güncelleştir varsa bunların üzerine.</span><span class="sxs-lookup"><span data-stu-id="30f63-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="30f63-454">Bunun yerine, yapabileceğiniz ek açıklamalar tutmak için mevcut olur ve modeli ile ilişkili meta verileri kısmi sınıflarını kullanımını sınıflarını kullanarak **[MetadataType]** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="30f63-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="30f63-455">Açık **başlamak** çözüm bulunan **kaynak/Ex6-AddingValidation/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="30f63-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="30f63-456">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="30f63-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="30f63-457">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="30f63-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="30f63-458">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="30f63-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="30f63-459">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="30f63-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="30f63-460">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="30f63-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="30f63-461">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="30f63-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="30f63-462">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="30f63-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="30f63-463">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="30f63-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="30f63-464">Açık **Album.cs** gelen **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="30f63-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="30f63-465">Değiştir **Album.cs** aşağıdaki gibi görünüyor. böylece vurgulanmış kodu ile içerik:</span><span class="sxs-lookup"><span data-stu-id="30f63-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="30f63-466">Satır **[DisplayFormat(ConvertEmptyStringToNull=false)]** veri alanı veri kaynağında güncelleştirildiğinde modelden boş dizelerin bir null değerine dönüştürülüp olmaz olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="30f63-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="30f63-467">Veri ek açıklamasını alanları doğrular önce Entity Framework null değerler modele atarken bu ayar bir özel durum kaçının.</span><span class="sxs-lookup"><span data-stu-id="30f63-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="30f63-468">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex6 albüm meta verileri parçalı sınıf*)</span><span class="sxs-lookup"><span data-stu-id="30f63-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="30f63-469">Bu **albüm** kısmi sınıfına sahip bir **MetadataType** işaret özniteliği **AlbumMetaData** veri ek açıklamaları için sınıf.</span><span class="sxs-lookup"><span data-stu-id="30f63-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="30f63-470">Bunlar, albüm model öğesine açıklama eklemek için kullandığınız veri ek açıklamasını öznitelikleri bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="30f63-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="30f63-471">Gerekli - özelliği gerekli bir alan olduğunu gösterir</span><span class="sxs-lookup"><span data-stu-id="30f63-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="30f63-472">DisplayName - form alanlarını ve doğrulama iletileri kullanılacak metni tanımlar</span><span class="sxs-lookup"><span data-stu-id="30f63-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="30f63-473">DisplayFormat - veri alanları nasıl görüntülenir ve biçimlendirilmiş belirtir.</span><span class="sxs-lookup"><span data-stu-id="30f63-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="30f63-474">StringLength - tanımlayan bir dize alanı için en fazla uzunluk</span><span class="sxs-lookup"><span data-stu-id="30f63-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="30f63-475">Aralık - sayısal bir alan için bir maksimum ve minimum değeri verir</span><span class="sxs-lookup"><span data-stu-id="30f63-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="30f63-476">ScaffoldColumn - sağlayan alanları Düzenleyicisi formlarda gizleme</span><span class="sxs-lookup"><span data-stu-id="30f63-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="30f63-477">Görev 2 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="30f63-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="30f63-478">Bu görevde, oluşturma ve düzenleme sayfaları alanları doğrulamak son görevde seçilen görünen adları kullanarak test.</span><span class="sxs-lookup"><span data-stu-id="30f63-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="30f63-479">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="30f63-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="30f63-480">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="30f63-480">The project starts in the Home page.</span></span> <span data-ttu-id="30f63-481">URL'ye değiştirin **oluşturma/StoreManager/**.</span><span class="sxs-lookup"><span data-stu-id="30f63-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="30f63-482">Görünen adları parçalı sınıf Listedekilerin eşleştiğinden emin olun (gibi **albüm resim URL'si** yerine **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="30f63-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="30f63-483">Tıklatın **oluşturma**, formu doldurarak olmadan.</span><span class="sxs-lookup"><span data-stu-id="30f63-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="30f63-484">Karşılık gelen doğrulama iletilerini alma doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="30f63-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="30f63-485">![Oluştur sayfası alanlarında doğrulanmış](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Oluştur sayfası alanlarında doğrulandı")</span><span class="sxs-lookup"><span data-stu-id="30f63-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="30f63-486">*Doğrulanmış alanlarında Oluştur sayfası*</span><span class="sxs-lookup"><span data-stu-id="30f63-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="30f63-487">Aynı ile oluşur doğrulayabilirsiniz **Düzenle** sayfası.</span><span class="sxs-lookup"><span data-stu-id="30f63-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="30f63-488">URL'ye değiştirin **/StoreManager/Edit/1** ve görünen adları parçalı sınıf Listedekilerin eşleştiğinden emin olun (gibi **albüm resim URL'si** yerine **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="30f63-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="30f63-489">Boş **başlık** ve **fiyat** alanları ve tıklatın **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="30f63-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="30f63-490">Karşılık gelen doğrulama iletilerini alma doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="30f63-490">Verify that you get the corresponding validation messages.</span></span>

    ![Düzenleme sayfasını doğrulanmış alanları](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="30f63-492">*Düzenleme sayfasını doğrulanmış alanları*</span><span class="sxs-lookup"><span data-stu-id="30f63-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="30f63-493">Alıştırma 7: İstemci tarafında örtük jQuery kullanma</span><span class="sxs-lookup"><span data-stu-id="30f63-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="30f63-494">Bu alıştırmada, istemci tarafında MVC 4 örtük jQuery doğrulamasını etkinleştirmek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="30f63-495">Örtük jQuery veri ajax önek JavaScript intrusively verme satır içi istemci komut dosyalarını yerine sunucunuzda eylem yöntemleri çağırmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="30f63-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="30f63-496">Görev 1 - etkinleştirme örtük jQuery önce uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="30f63-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="30f63-497">Bu görevde, hem doğrulama modeli karşılaştırmak için jQuery dahil olmak üzere önce uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="30f63-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="30f63-498">Açık **başlamak** çözüm bulunan **kaynak/Ex7-UnobtrusivejQueryValidation/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="30f63-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="30f63-499">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="30f63-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="30f63-500">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="30f63-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="30f63-501">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="30f63-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="30f63-502">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="30f63-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="30f63-503">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="30f63-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="30f63-504">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="30f63-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="30f63-505">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="30f63-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="30f63-506">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="30f63-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="30f63-507">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="30f63-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="30f63-508">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="30f63-508">The project starts in the Home page.</span></span> <span data-ttu-id="30f63-509">Gözat **/StoreManager/oluşturma** tıklatıp **oluşturma** doğrulama iletileri almak doğrulamak için formu doldurarak olmadan:</span><span class="sxs-lookup"><span data-stu-id="30f63-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="30f63-510">![İstemci doğrulama devre dışı](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "istemci doğrulama devre dışı")</span><span class="sxs-lookup"><span data-stu-id="30f63-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="30f63-511">*İstemci doğrulama devre dışı*</span><span class="sxs-lookup"><span data-stu-id="30f63-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="30f63-512">Tarayıcıda, HTML kaynak kodunu açın:</span><span class="sxs-lookup"><span data-stu-id="30f63-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="30f63-513">Görev 2 - etkinleştirme örtük istemci doğrulama</span><span class="sxs-lookup"><span data-stu-id="30f63-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="30f63-514">Bu görevde, jQuery etkinleştirecek **örtük istemci doğrulama** gelen **Web.config** dosya, varsayılan olarak tüm yeni ASP.NET MVC 4 projelerini false değerini alır.</span><span class="sxs-lookup"><span data-stu-id="30f63-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="30f63-515">Ayrıca, gerekli jQuery örtük istemci doğrulaması iş yapmak için başvurular komutlar ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="30f63-516">Açık **Web.Config** dosya proje kök dizininde ve olduğundan emin olun **ClientValidationEnabled** ve **UnobtrusiveJavaScriptEnabled** anahtarları değerler için ayarlanır**true**.</span><span class="sxs-lookup"><span data-stu-id="30f63-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="30f63-517">Aynı sonuçları almak için Global.asax.cs kodu tarafından istemci doğrulaması da etkinleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="30f63-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="30f63-518">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="30f63-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="30f63-519">Ayrıca, özel davranışı sağlamak için tüm Denetleyicisi'nde ClientValidationEnabled özniteliği atayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="30f63-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="30f63-520">Açık **Create.cshtml** adresindeki **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="30f63-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="30f63-521">Aşağıdaki komut dosyalarını emin olun **jquery.validate** ve **jquery.validate.unobtrusive**, görünüm yoluyla başvurulan &quot; **~/bundles/jqueryval** &quot; paket.</span><span class="sxs-lookup"><span data-stu-id="30f63-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="30f63-522">Bu jQuery kitaplıklar, MVC 4 yeni projelerinde dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="30f63-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="30f63-523">Daha fazla kitaplıklarında bulabilirsiniz **/komut dosyası** , projenin klasör.</span><span class="sxs-lookup"><span data-stu-id="30f63-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="30f63-524">Bu doğrulama yapmak için kitaplıkları iş, jQuery framework kitaplığına bir başvuru eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="30f63-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="30f63-525">Bu başvuru sayfasına eklendiğinden beri  **\_Layout.cshtml** dosyası gerektirmeyen belirli bu görünümde eklemek.</span><span class="sxs-lookup"><span data-stu-id="30f63-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="30f63-526">Görev 3 - uygulama kullanarak örtük jQuery doğrulama çalıştırma</span><span class="sxs-lookup"><span data-stu-id="30f63-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="30f63-527">Bu görevde, sınayacak **StoreManager** şablonu gerçekleştiren kullanıcı yeni albümü oluşturduğunda, jQuery kitaplıkları kullanarak istemci tarafı doğrulama görünüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="30f63-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="30f63-528">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="30f63-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="30f63-529">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="30f63-529">The project starts in the Home page.</span></span> <span data-ttu-id="30f63-530">Gözat **/StoreManager/oluşturma** tıklatıp **oluşturma** doğrulama iletileri almak doğrulamak için formu doldurarak olmadan:</span><span class="sxs-lookup"><span data-stu-id="30f63-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="30f63-531">![İstemci doğrulama etkin jQuery ile](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "etkin jQuery ile istemci doğrulama")</span><span class="sxs-lookup"><span data-stu-id="30f63-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="30f63-532">*Etkin jQuery ile istemci doğrulama*</span><span class="sxs-lookup"><span data-stu-id="30f63-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="30f63-533">Tarayıcıda oluşturma görünüm için kaynak kodunu açın:</span><span class="sxs-lookup"><span data-stu-id="30f63-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="30f63-534">Her istemci doğrulama kuralı için örtük jQuery verilerle bir öznitelik ekler-val -*rulename*=&quot;*ileti*&quot;.</span><span class="sxs-lookup"><span data-stu-id="30f63-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="30f63-535">Etiketlerin listesini bu Unobtrusive aşağıdadır jQuery istemci doğrulama gerçekleştirmek için html giriş alanında ekler:</span><span class="sxs-lookup"><span data-stu-id="30f63-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="30f63-536">Veri val</span><span class="sxs-lookup"><span data-stu-id="30f63-536">Data-val</span></span>
   > - <span data-ttu-id="30f63-537">Veri val numarası</span><span class="sxs-lookup"><span data-stu-id="30f63-537">Data-val-number</span></span>
   > - <span data-ttu-id="30f63-538">Veri val aralığı</span><span class="sxs-lookup"><span data-stu-id="30f63-538">Data-val-range</span></span>
   > - <span data-ttu-id="30f63-539">Veri val aralığı min / val aralığı maksimum veri</span><span class="sxs-lookup"><span data-stu-id="30f63-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="30f63-540">Veri val gerekiyor</span><span class="sxs-lookup"><span data-stu-id="30f63-540">Data-val-required</span></span>
   > - <span data-ttu-id="30f63-541">Veri val uzunluğu</span><span class="sxs-lookup"><span data-stu-id="30f63-541">Data-val-length</span></span>
   > - <span data-ttu-id="30f63-542">Verileri-val-uzunluk-max / verileri-val-uzunluk-min</span><span class="sxs-lookup"><span data-stu-id="30f63-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="30f63-543">Tüm veri değerleri ile model doldurulur **veri ek açıklamasını**.</span><span class="sxs-lookup"><span data-stu-id="30f63-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="30f63-544">Ardından, sunucu tarafında çalışan tüm mantığı istemci tarafında çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="30f63-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="30f63-545">Örneğin, fiyat özniteliği aşağıdaki veri ek açıklamasını modele sahiptir:</span><span class="sxs-lookup"><span data-stu-id="30f63-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="30f63-546">Örtük jQuery kullandıktan sonra oluşturulan kodu verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="30f63-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="30f63-547">Özet</span><span class="sxs-lookup"><span data-stu-id="30f63-547">Summary</span></span>

<span data-ttu-id="30f63-548">Bu uygulamalı Laboratuvar tamamlayarak kullanıcılar aşağıdakileri kullanarak veritabanında depolanan verileri değiştirmek etkinleştirme öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="30f63-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="30f63-549">Dizin oluşturma, düzenleme, silme gibi denetleyici eylemleri</span><span class="sxs-lookup"><span data-stu-id="30f63-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="30f63-550">ASP.NET MVC'ın yapı iskelesi özelliği için bir HTML tablosunda özelliklerini görüntüleme</span><span class="sxs-lookup"><span data-stu-id="30f63-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="30f63-551">Kullanıcı artırmak için özel HTML Yardımcıları deneyimi</span><span class="sxs-lookup"><span data-stu-id="30f63-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="30f63-552">HTTP GET veya POST HTTP çağrıları tepki eylem yöntemleri</span><span class="sxs-lookup"><span data-stu-id="30f63-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="30f63-553">Paylaşılan Düzenleyici şablonu oluşturma ve düzenleme gibi benzer görünüm şablonları için</span><span class="sxs-lookup"><span data-stu-id="30f63-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="30f63-554">Aşağı açılan listeler gibi form öğeleri</span><span class="sxs-lookup"><span data-stu-id="30f63-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="30f63-555">Model doğrulama için veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="30f63-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="30f63-556">İstemci tarafı jQuery örtük kitaplığını kullanarak doğrulama</span><span class="sxs-lookup"><span data-stu-id="30f63-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="30f63-557">Ek A: Yükleme Web Visual Studio Express 2012 için</span><span class="sxs-lookup"><span data-stu-id="30f63-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="30f63-558">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="30f63-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="30f63-559">Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="30f63-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="30f63-560">Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="30f63-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="30f63-561">Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK'sı Web</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="30f63-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="30f63-562">Tıklayın **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="30f63-562">Click on **Install Now**.</span></span> <span data-ttu-id="30f63-563">Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="30f63-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="30f63-564">Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="30f63-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="30f63-565">![Visual Studio Express yükleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "yükleme Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="30f63-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="30f63-566">*Visual Studio Express yükleme*</span><span class="sxs-lookup"><span data-stu-id="30f63-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="30f63-567">Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="30f63-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşulları kabul ediliyor](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="30f63-569">*Lisans koşulları kabul ediliyor*</span><span class="sxs-lookup"><span data-stu-id="30f63-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="30f63-570">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="30f63-570">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="30f63-572">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="30f63-572">*Installation progress*</span></span>
6. <span data-ttu-id="30f63-573">Yükleme tamamlandığında tıklatın **son**.</span><span class="sxs-lookup"><span data-stu-id="30f63-573">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="30f63-575">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="30f63-575">*Installation completed*</span></span>
7. <span data-ttu-id="30f63-576">Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="30f63-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="30f63-577">Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.</span><span class="sxs-lookup"><span data-stu-id="30f63-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web döşemeye](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="30f63-579">*VS Express Web döşemeye*</span><span class="sxs-lookup"><span data-stu-id="30f63-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="30f63-580">Ek B: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="30f63-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="30f63-581">Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip.</span><span class="sxs-lookup"><span data-stu-id="30f63-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="30f63-582">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="30f63-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="30f63-583">![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="30f63-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="30f63-584">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="30f63-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="30f63-585">***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="30f63-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="30f63-586">İmleci, burada kod eklemek istediğiniz yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="30f63-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="30f63-587">(Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="30f63-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="30f63-588">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="30f63-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="30f63-589">Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="30f63-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="30f63-590">İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="30f63-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="30f63-591">![Kod parçacığında adını yazmaya başlayın](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="30f63-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="30f63-592">*Kod parçacığında adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="30f63-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="30f63-593">![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="30f63-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="30f63-594">*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="30f63-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="30f63-595">![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")</span><span class="sxs-lookup"><span data-stu-id="30f63-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="30f63-596">*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*</span><span class="sxs-lookup"><span data-stu-id="30f63-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="30f63-597">***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1.</span><span class="sxs-lookup"><span data-stu-id="30f63-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="30f63-598">Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="30f63-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="30f63-599">Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.</span><span class="sxs-lookup"><span data-stu-id="30f63-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="30f63-600">Tıklayarak ilgili kod parçacığında listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="30f63-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="30f63-601">![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")</span><span class="sxs-lookup"><span data-stu-id="30f63-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="30f63-602">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*</span><span class="sxs-lookup"><span data-stu-id="30f63-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="30f63-603">![Tıklayarak ilgili kod parçacığında listeden çekme](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "tıklayarak ilgili kod parçacığında listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="30f63-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="30f63-604">*Tıklayarak ilgili kod parçacığında listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="30f63-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
