---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: "ASP.NET MVC 4 Yardımcıları, formlar ve doğrulama | Microsoft Docs"
author: rick-anderson
description: "ASP.NET MVC 4 modelleri ve veri erişim uygulamalı Laboratuvar, alınan yükleniyor ve veritabanındaki verileri görüntüleme. Bu uygulamalı Laboratuvar için ekleyeceksiniz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 925d659f42496045089ba056e194ac977c37a8de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="5dcc5-104">ASP.NET MVC 4 Yardımcıları, formlar ve doğrulama</span><span class="sxs-lookup"><span data-stu-id="5dcc5-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>
====================
<span data-ttu-id="5dcc5-105">tarafından [Web Camps ekibi](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="5dcc5-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="5dcc5-106">İçinde **ASP.NET MVC 4 modelleri ve veri erişimi** uygulamalı Laboratuvar yükleniyor ve veritabanındaki verileri görüntüleme olmuştur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-106">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="5dcc5-107">Bu uygulamalı laboratuar ortamında, ekleyecek **müzik deposu** uygulama bu verileri düzenleme özelliği.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-107">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>
> 
> <span data-ttu-id="5dcc5-108">Bu hedefi dikkate alarak, albümleri oluşturma, okuma, güncelleştirme ve silme (CRUD) eylemleri destekleyecek denetleyicisi ilk oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-108">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="5dcc5-109">Bir HTML tablosunda albümleri özelliklerini görüntülemek için ASP.NET MVC'ın iskele özelliği yararlanarak bir dizin görünümünün şablonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-109">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="5dcc5-110">Bu görünüm geliştirmek için uzun açıklamaları keser özel bir HTML Yardımcısı ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-110">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>
> 
> <span data-ttu-id="5dcc5-111">Daha sonra düzenleme ve oluşturma sağlayan görünümleri bırakmalar gibi form öğelerin yardımıyla veritabanında albümleri alter ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-111">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>
> 
> <span data-ttu-id="5dcc5-112">Son olarak, albümü silme kullanıcılar sağlar ve ayrıca, bunları kendi girişi doğrulama tarafından yanlış veri geçmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-112">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="5dcc5-113">Bu uygulamalı Laboratuvar temel bilgiye sahip varsayar **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-113">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="5dcc5-114">Değil kullandıysanız **ASP.NET MVC** gitmenizi öneririz önce **ASP.NET MVC Temelleri** uygulamalı Laboratuvar.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-114">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>
> 
> 
> <span data-ttu-id="5dcc5-115">Bu Laboratuvar, kaynak klasöre sağlanan örnek bir Web uygulamasına küçük değişiklikler uygulayarak daha önce açıklanan yeni özellikleri ve geliştirmeleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-115">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="5dcc5-116">Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="5dcc5-116">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="5dcc5-117">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="5dcc5-117">Objectives</span></span>

<span data-ttu-id="5dcc5-118">Bu uygulamalı laboratuar ortamında, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-118">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="5dcc5-119">CRUD işlemleri desteklemek için bir denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="5dcc5-119">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="5dcc5-120">Bir HTML tablosunda varlık özellikleri görüntülemek için bir dizin Görünüm Oluştur</span><span class="sxs-lookup"><span data-stu-id="5dcc5-120">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="5dcc5-121">Özel bir HTML Yardımcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-121">Add a custom HTML helper</span></span>
- <span data-ttu-id="5dcc5-122">Oluşturma ve düzenleme görünümü özelleştirme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-122">Create and customize an Edit View</span></span>
- <span data-ttu-id="5dcc5-123">HTTP GET veya POST HTTP çağrıları tepki eylem yöntemleri birbirinden</span><span class="sxs-lookup"><span data-stu-id="5dcc5-123">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="5dcc5-124">Ekleme ve Görünüm Oluştur özelleştirme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-124">Add and customize a Create View</span></span>
- <span data-ttu-id="5dcc5-125">Tanıtıcı bir varlığı silme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-125">Handle the deletion of an entity</span></span>
- <span data-ttu-id="5dcc5-126">Kullanıcı girişini doğrulama</span><span class="sxs-lookup"><span data-stu-id="5dcc5-126">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="5dcc5-127">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="5dcc5-127">Prerequisites</span></span>

<span data-ttu-id="5dcc5-128">Bu laboratuvarı tamamlamak için aşağıdaki öğeleri sahip olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-128">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="5dcc5-129">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="5dcc5-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="5dcc5-130">Kurulum</span><span class="sxs-lookup"><span data-stu-id="5dcc5-130">Setup</span></span>

<span data-ttu-id="5dcc5-131">**Kod parçacıkları yükleme**</span><span class="sxs-lookup"><span data-stu-id="5dcc5-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="5dcc5-132">Kolaylık olması için bu Laboratuvar yönetme kod çoğunu Visual Studio kod parçacıkları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="5dcc5-133">Çalıştırma kod parçacıkları yüklemek için **.\Source\Setup\CodeSnippets.vsi** dosya.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="5dcc5-134">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek B: kullanarak kod parçacıkları](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="5dcc5-135">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="5dcc5-135">Exercises</span></span>

<span data-ttu-id="5dcc5-136">Aşağıdaki alıştırmada bu uygulamalı laboratuarı olun:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-136">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="5dcc5-137">Mağaza Yöneticisi denetleyicisi ve onun dizini görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="5dcc5-137">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="5dcc5-138">Bir HTML Yardımcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-138">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="5dcc5-139">Düzenleme görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="5dcc5-139">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="5dcc5-140">Oluştur görünümünün ekleme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-140">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="5dcc5-141">İşleme silme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-141">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="5dcc5-142">Doğrulama Ekleme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-142">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="5dcc5-143">İstemci tarafında örtük jQuery kullanma</span><span class="sxs-lookup"><span data-stu-id="5dcc5-143">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="5dcc5-144">Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="5dcc5-145">Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="5dcc5-146">Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**</span><span class="sxs-lookup"><span data-stu-id="5dcc5-146">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="5dcc5-147">Alıştırma 1: mağaza yöneticisi denetleyicisi ve onun dizini görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="5dcc5-147">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="5dcc5-148">Bu alıştırmada, CRUD işlemleri desteklemek için veritabanını ve son olarak ASP.NET MVC'nin yapı iskelesi yararlanarak bir dizin görünümünün şablonu oluşturma albümleri listesini döndürmek için dizin eylem yöntemini Özelleştir yeni bir denetleyici oluşturma öğreneceksiniz. bir HTML tablosunda albümleri özelliklerini görüntülemek için özellik.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-148">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="5dcc5-149">Görev 1 - StoreManagerController oluşturma</span><span class="sxs-lookup"><span data-stu-id="5dcc5-149">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="5dcc5-150">Bu görevde adlı yeni bir denetleyici oluşturacak **StoreManagerController** CRUD işlemleri desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-150">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="5dcc5-151">Açık **başlamak** çözüm bulunan **kaynak/Ex1-CreatingTheStoreManagerController/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-151">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

    1. <span data-ttu-id="5dcc5-152">Bazı eksik NuGet paketlerini indirmek gerekir devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5dcc5-153">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5dcc5-154">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5dcc5-155">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5dcc5-156">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5dcc5-157">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5dcc5-158">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5dcc5-159">Yeni denetleyici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-159">Add a new controller.</span></span> <span data-ttu-id="5dcc5-160">Bunu yapmak için sağ **denetleyicileri** Klasör Seç Çözüm Gezgini içinde **Ekle** ve ardından **denetleyicisi** komutu.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-160">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="5dcc5-161">Değişiklik **denetleyicisi** **adı** için **StoreManagerController** ve seçeneği emin olun **boşokuma/yazmaeylemleriileMVCdenetleyicisi**seçilir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-161">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="5dcc5-162">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-162">Click **Add**.</span></span>

    <span data-ttu-id="5dcc5-163">![Ekle denetleyicisi iletişim](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Ekle denetleyicisi iletişim")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-163">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="5dcc5-164">*Denetleyici Ekle iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-164">*Add Controller Dialog*</span></span>

    <span data-ttu-id="5dcc5-165">Yeni bir denetleyici sınıfı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-165">A new Controller class is generated.</span></span> <span data-ttu-id="5dcc5-166">Okuma/yazma, kişiler, saplama yöntemleri için Eylemler eklemek için belirtilen beri genel CRUD eylemleri uygulama belirli mantığı eklemek isteyen doldurulur Yapılacaklar açıklamaları ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-166">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="5dcc5-167">Görev 2 - StoreManager dizinini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-167">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="5dcc5-168">Bu görevde, veritabanından albümleri listesiyle birlikte bir görünüme dönmek için StoreManager dizin eylem yönteminin özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-168">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="5dcc5-169">Aşağıdaki StoreManagerController sınıfında ekleyin *kullanarak* yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-169">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="5dcc5-170">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex1 MvcMusicStore kullanarak*)</span><span class="sxs-lookup"><span data-stu-id="5dcc5-170">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="5dcc5-171">Bir alan eklemek **StoreManagerController** örneği tutmak için **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="5dcc5-171">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="5dcc5-172">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="5dcc5-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="5dcc5-173">Albümleri listesiyle birlikte bir görünüme dönmek için StoreManagerController dizin eylemi uygulayın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-173">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="5dcc5-174">Denetleyici eylem mantığı StoreController'ın dizin eylem için daha önce yazılmış çok benzer.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-174">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="5dcc5-175">LINQ tarz ve sanatçı bilgilerini görüntülemek için de dahil olmak üzere tüm albümleri almak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-175">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="5dcc5-176">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex1 StoreManagerController dizin*)</span><span class="sxs-lookup"><span data-stu-id="5dcc5-176">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="5dcc5-177">Görev 3 - dizin görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="5dcc5-177">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="5dcc5-178">Bu görevde tarafından döndürülen albümleri listesini görüntülemek için dizin görünümünün şablonu oluşturacak **StoreManager** denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-178">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="5dcc5-179">Yeni Görünüm şablonu oluşturmadan önce projeyi derlemek böylece **Ekle iletişim kutusunu görüntüle** bildiği **albüm** kullanılacak sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-179">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="5dcc5-180">Seçin **yapı | MvcMusicStore derleme** Projeyi derlemek için.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-180">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="5dcc5-181">İçinde sağ **dizin** eylem yöntemi ve select **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-181">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="5dcc5-182">Bu getirir **Görünüm Ekle** iletişim.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-182">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="5dcc5-183">![Görünüm Ekle](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-183">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="5dcc5-184">*Dizin yöntemi içinden bir görünümle ekleme*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-184">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="5dcc5-185">Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **dizin**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-185">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="5dcc5-186">Seçin **kesin türü belirtilmiş görünüm oluşturma** seçeneğini ve **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-186">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="5dcc5-187">Seçin **listesi** gelen **İskele şablonu** açılır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-187">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="5dcc5-188">Bırakın **görünüm altyapısı** için **Razor** ve diğer alanları varsayılan ile değer ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-188">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="5dcc5-189">![Bir dizini görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "bir dizini görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-189">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="5dcc5-190">*Bir dizini görünümü ekleme*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-190">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="5dcc5-191">Görev 4 - dizin görünümünün iskele özelleştirme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-191">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="5dcc5-192">Bu görevde, istediğiniz alanları görüntülemek için ASP.NET MVC yapı iskelesi özelliğiyle oluşturulmuş basit görünüm şablonu ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-192">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="5dcc5-193">**İskele** ASP.NET MVC içinde desteği albüm modelindeki tüm alanları listeleyen basit bir görünüm şablon oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-193">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="5dcc5-194">**İskele** kesin türü belirtilmiş bir görünüm üzerinde çalışmaya başlamak için hızlı bir yol sunar: görünüm şablonu el ile yazmak yerine hızlı bir şekilde iskele varsayılan şablon oluşturur ve oluşturulan kod daha sonra değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-194">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="5dcc5-195">Oluşturulan kod gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-195">Review the code created.</span></span> <span data-ttu-id="5dcc5-196">Alan listesinin oluşturulmasını aşağıdaki parçası olacak HTML tablo **İskele** tablo veri görüntülemek için kullanma.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-196">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="5dcc5-197">Değiştir  **&lt;tablo&gt;**  yalnızca görüntülemek için aşağıdaki kodu koduyla **Tarz**, **sanatçı**, **albüm başlığı**, ve **fiyat** alanları.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-197">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="5dcc5-198">Bu siler **AlbumId** ve **albüm resim URL'si** sütun.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-198">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="5dcc5-199">Ayrıca, kullanıcıların bağlı sınıf özelliklerini görüntülemek için GenreId ve ArtistId sütunları değiştirir **Artist.Name** ve **Genre.Name**ve kaldırır **ayrıntıları** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-199">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="5dcc5-200">Aşağıdaki açıklamaları değiştirin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-200">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="5dcc5-201">Görev 5 - Uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="5dcc5-201">Task 5 - Running the Application</span></span>

<span data-ttu-id="5dcc5-202">Bu görevde, sınayacak **StoreManager** **dizin** şablonu görüntüleme önceki adımları tasarımını göre Albümler listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-202">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="5dcc5-203">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-203">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5dcc5-204">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-204">The project starts in the Home page.</span></span> <span data-ttu-id="5dcc5-205">URL'ye değiştirin **/StoreManager** gösteren albümleri listesini görüntülendiğini doğrulamak için kendi **başlık**, **sanatçı** ve **Tarz**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-205">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="5dcc5-206">![Albümleri listesi gözatma](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "albümleri listesi gözatma")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-206">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="5dcc5-207">*Albümleri listesi gözatma*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-207">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="5dcc5-208">Alıştırma 2: bir HTML Yardımcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-208">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="5dcc5-209">StoreManager dizin sayfası olası bir sorunu var: Başlık ve sanatçı ad özellikleri her ikisi de olabilir tablosunu biçimlendirme kapalı throw yetecek kadar uzun.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-209">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="5dcc5-210">Bu alıştırmada, metnin kesmek için özel bir HTML Yardımcısı eklemek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-210">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="5dcc5-211">Aşağıdaki şekilde, bir küçük tarayıcı boyutu kullandığınızda biçimi metnin uzunluğu nedeniyle nasıl değiştirilir görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-211">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="5dcc5-212">![İle albümleri listesi gözatma değil metin kesilmiş](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "ile albümleri listesi gözatma değil metin kesildi")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-212">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="5dcc5-213">*İle albümleri listesi gözatma metin kesilmiş değil*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-213">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="5dcc5-214">Görev 1 - HTML Yardımcısı genişletme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-214">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="5dcc5-215">Bu görevde, yeni bir yöntem ekleyecek **Truncate** için **HTML** ASP.NET MVC görünümler içinde sunulan nesne.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-215">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="5dcc5-216">Bunu yapmak için gerçekleştireceksiniz bir **genişletme yöntemi** yerleşik için **System.Web.Mvc.HtmlHelper** ASP.NET MVC tarafından sağlanan sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-216">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="5dcc5-217">Hakkında daha fazla bilgi edinmek için **genişletme yöntemleri**, lütfen bu msdn makalesine bakın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-217">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="5dcc5-218">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dcc5-218">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="5dcc5-219">Açık **başlamak** çözüm bulunan **kaynak/Ex2-AddingAnHTMLHelper/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-219">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="5dcc5-220">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-220">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5dcc5-221">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-221">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5dcc5-222">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-222">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5dcc5-223">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-223">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5dcc5-224">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-224">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5dcc5-225">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-225">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5dcc5-226">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-226">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5dcc5-227">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-227">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5dcc5-228">StoreManager'ın dizini görünümünü açın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-228">Open StoreManager's Index View.</span></span> <span data-ttu-id="5dcc5-229">Bunu yapmak için Çözüm Gezgini'nde genişletin **görünümleri** klasörü, sonra **StoreManager** açarak **Index.cshtml** dosya.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-229">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="5dcc5-230">Aşağıdaki kodu ekleyin  **@model**  tanımlamak için yönergesi **Truncate** yardımcı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-230">Add the following code below the **@model** directive to define the **Truncate** helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="5dcc5-231">Görev 2 - metnin sayfadaki kesiliyor</span><span class="sxs-lookup"><span data-stu-id="5dcc5-231">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="5dcc5-232">Bu görevde, kullanacağınız **Truncate** şablonu görüntüleme metni kesmek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-232">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="5dcc5-233">StoreManager'ın dizini görünümünü açın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-233">Open StoreManager's Index View.</span></span> <span data-ttu-id="5dcc5-234">Bunu yapmak için Çözüm Gezgini'nde genişletin **görünümleri** klasörü, sonra **StoreManager** açarak **Index.cshtml** dosya.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-234">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="5dcc5-235">Göster satırları değiştirmek **sanatçı adı** ve albüm **başlık**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-235">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="5dcc5-236">Bunu yapmak için aşağıdaki satırları değiştirin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-236">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="5dcc5-237">Görev 3 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="5dcc5-237">Task 3 - Running the Application</span></span>

<span data-ttu-id="5dcc5-238">Bu görevde, sınayacak **StoreManager** **dizin** şablonu görüntüleme albüm başlık ve sanatçı adı tamsayıya dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-238">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="5dcc5-239">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-239">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5dcc5-240">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-240">The project starts in the Home page.</span></span> <span data-ttu-id="5dcc5-241">URL'ye değiştirin **/StoreManager** bu uzun doğrulamak için metinleri **başlık** ve **sanatçı** sütun kesilir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-241">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="5dcc5-242">![Kesilen başlıkları ve Sanatçılar adları](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "kesilmiş başlıkları ve Sanatçılar adları")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-242">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="5dcc5-243">*Kesilmiş başlıklarını ve sanatçı adları*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-243">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="5dcc5-244">Alıştırma 3: düzenleme görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="5dcc5-244">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="5dcc5-245">Bu alıştırmada, albüm düzenlemek Mağaza yöneticileri izin vermek için bir formun nasıl oluşturulacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-245">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="5dcc5-246">Gözat **/StoreManager/Edit/id** URL (**kimliği** düzenlemek için albümü benzersiz kimliğini olan), böylece sunucusuna bir HTTP GET çağrısı yapma.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-246">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="5dcc5-247">Denetleyici Düzenle eylem yöntemi uygun albüm veritabanından, oluşturma bir **StoreManagerViewModel** nesne (listesini Sanatçılar ve türler ile birlikte) kapsüllemek ve daha sonra bir görünüm şablonuna geçirin için HTML sayfası kullanıcıya geri işlenemiyor.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-247">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="5dcc5-248">Bu sayfayı içerecek bir  **&lt;form&gt;**  metin kutuları ve albüm özelliklerini düzenlemek için bırakmalar öğe.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-248">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="5dcc5-249">Kullanıcı albüm form değerleri güncelleştirir ve tıklar sonra **kaydetmek** düğmesi, değişiklikleri aracılığıyla gönderilir bir HTTP POST geri çağrı **/StoreManager/Edit/id**. URL son çağrı olduğu gibi aynı kalsa da, ASP.NET MVC, bir HTTP POST ve bu nedenle farklı bir düzen eylem yöntemi yürütülmeden bu kez tanımlar (bir donatılmış ile **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="5dcc5-249">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="5dcc5-250">Görev 1 - HTTP GET düzenleme eylem yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="5dcc5-250">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="5dcc5-251">Bu görevde, uygun albüm veritabanından almak için düzenleme eylem yöntemini HTTP GET sürümü yanı sıra, tüm türler ve Sanatçılar listesini gerçekleştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-251">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="5dcc5-252">Bu veriler içine paketi **StoreManagerViewModel** ardından yanıtı işlemek için bir şablonu görüntüleme geçirilecektir son adımda tanımlanan nesne.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-252">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="5dcc5-253">Açık **başlamak** çözüm bulunan **kaynak/Ex3-CreatingTheEditView/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-253">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="5dcc5-254">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-254">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5dcc5-255">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-255">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5dcc5-256">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-256">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5dcc5-257">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-257">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5dcc5-258">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-258">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5dcc5-259">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-259">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5dcc5-260">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-260">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5dcc5-261">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-261">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5dcc5-262">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-262">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="5dcc5-263">Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-263">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="5dcc5-264">Değiştir **HTTP GET Düzenle** uygun almak için aşağıdaki kodu eylem yöntemiyle **albüm** yanı sıra **türler** ve **Sanatçılar**listeler.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-264">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="5dcc5-265">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex3 StoreManagerController HTTP GET Düzenle eylem*)</span><span class="sxs-lookup"><span data-stu-id="5dcc5-265">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="5dcc5-266">Kullanmakta olduğunuz **System.Web.Mvc** **SelectList** Sanatçılar ve yerine türler için **System.Collections.Generic** listesi.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-266">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="5dcc5-267">**SelectList** HTML bırakmalar doldurmak ve geçerli seçim gibi şeyleri yönetmek için temiz bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-267">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="5dcc5-268">Örnek oluşturma ve bu denetleyici eylemini ViewModel nesneleri sonraki ayarlama düzenleme form senaryo temizleyici hale getirir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-268">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="5dcc5-269">Görev 2 - düzenleme görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="5dcc5-269">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="5dcc5-270">Bu görevde, daha sonra Albüm özellikleri görüntüleyecek bir görünümü Düzenle şablonu oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-270">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="5dcc5-271">Düzenleme görünümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-271">Create the Edit View.</span></span> <span data-ttu-id="5dcc5-272">İçinde Bunu yapmak için sağ **Düzenle** eylem yöntemi ve select **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-272">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="5dcc5-273">Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-273">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="5dcc5-274">Denetleyin **kesin türü belirtilmiş görünüm oluşturmak** onay kutusunu seçip **albüm (MvcMusicStore.Models)** gelen **görüntülemek veri sınıfı** açılır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-274">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="5dcc5-275">Seçin **Düzenle** gelen **İskele şablonu** açılır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-275">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="5dcc5-276">Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-276">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="5dcc5-277">![Düzenleme görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "düzenleme görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-277">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="5dcc5-278">*Düzenleme görünümü ekleme*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-278">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="5dcc5-279">Görev 3 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="5dcc5-279">Task 3 - Running the Application</span></span>

<span data-ttu-id="5dcc5-280">Bu görevde, sınayacak **StoreManager** **Düzenle** görünüm sayfası parametre olarak geçirilen albüm özelliklerin değerlerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-280">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="5dcc5-281">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-281">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5dcc5-282">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-282">The project starts in the Home page.</span></span> <span data-ttu-id="5dcc5-283">URL'ye değiştirin **/StoreManager/Edit/1** geçirilen albüm özelliklerin değerlerini görüntülendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-283">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="5dcc5-284">![Albüm düzenleme görünümü gözatma](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "albüm düzenleme görünümü gözatma")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-284">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="5dcc5-285">*Albüm düzenleme görünümü gözatma*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-285">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="5dcc5-286">Görev 4 - aşağı açılan listeler albüm Düzenleyicisi şablonu uygulama</span><span class="sxs-lookup"><span data-stu-id="5dcc5-286">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="5dcc5-287">Sanatçılar ve türler listesinden seçebilmeniz için bu görevde, aşağı açılan listeler son görevde oluşturulan görünüm şablona ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-287">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="5dcc5-288">Tüm değiştirmek **albüm** fieldset kodu aşağıdakilerle:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-288">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="5dcc5-289">Bir **Html.DropDownList** Yardımcısı, Sanatçılar ve türler seçmek için aşağı açılan listeler işlemek için eklendi.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-289">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="5dcc5-290">Parametreleri geçirilen **Html.DropDownList** şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-290">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="5dcc5-291">Form alanı adını (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="5dcc5-291">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="5dcc5-292">**SelectList** aşağı açılan değer.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-292">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="5dcc5-293">Görev 5 - Uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="5dcc5-293">Task 5 - Running the Application</span></span>

<span data-ttu-id="5dcc5-294">Bu görevde, sınayacak **StoreManager** **Düzenle** görünüm sayfası aşağı açılan listeler sanatçı ve Tarz kimliği metin alanları yerine görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-294">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="5dcc5-295">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-295">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5dcc5-296">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-296">The project starts in the Home page.</span></span> <span data-ttu-id="5dcc5-297">URL'ye değiştirin **/StoreManager/Edit/1** aşağı açılan listeler sanatçı ve Tarz kimliği metin alanları yerine görüntülediğini doğrulamak üzere.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-297">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="5dcc5-298">![Albüm Görünümü Düzenle açılan listeleri ile tarama](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "gözatma albüm düzenleme görünümü açılan listeleri")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-298">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="5dcc5-299">*Albüm düzenleme görünümü, bu kez bırakmalar gözatma*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-299">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="5dcc5-300">Görev 6 - HTTP POST Düzenle eylem yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="5dcc5-300">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="5dcc5-301">Görünümü Düzenle beklendiği gibi görüntüler, albümü yapılan değişiklikleri kaydetmek için HTTP POST Düzenle eylem yöntemi uygulamak için gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-301">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="5dcc5-302">Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-302">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="5dcc5-303">Açık **StoreManagerController** gelen **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-303">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="5dcc5-304">Değiştir **HTTP POST Düzenle** eylem yöntemini aşağıdaki kodla (değiştirilmelidir yöntemi iki parametre alan aşırı yüklenmiş sürümü olduğunu unutmayın):</span><span class="sxs-lookup"><span data-stu-id="5dcc5-304">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="5dcc5-305">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex3 StoreManagerController HTTP POST Düzenle eylem*)</span><span class="sxs-lookup"><span data-stu-id="5dcc5-305">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="5dcc5-306">Bu yöntem kullanıcı tıklattığında yürütülür **kaydetmek** görünümünün düğmesi ve bir HTTP veritabanında kalıcı hale getirmek için POST form değerlerinin sunucu gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-306">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="5dcc5-307">Oluşturma öğesi **[HttpPost]** yöntemi bu HTTP POST senaryoları için kullanılması gerektiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-307">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="5dcc5-308">Yöntem alır bir **albüm** nesnesi.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-308">The method takes an **Album** object.</span></span> <span data-ttu-id="5dcc5-309">ASP.NET MVC otomatik olarak oluşturacak albüm nesne gönderilen &lt;form&gt; değerleri.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-309">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="5dcc5-310">Yöntemi, aşağıdaki adımları gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-310">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="5dcc5-311">Model geçerliyse:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-311">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="5dcc5-312">Değiştirilen bir nesne olarak işaretlemek için bağlamda albüm giriş güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-312">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="5dcc5-313">Değişiklikleri kaydetmek ve dizin görünümüne yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-313">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="5dcc5-314">Model geçerli değilse, Görünüm Paketi ile doldurulacaktır **GenreId** ve **ArtistId**, izin vermek için alınan albüm nesnenin görünümüyle döndürecektir gerekli herhangi bir güncelleştirme gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-314">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="5dcc5-315">7 - uygulama çalışan görev</span><span class="sxs-lookup"><span data-stu-id="5dcc5-315">Task 7 - Running the Application</span></span>

<span data-ttu-id="5dcc5-316">Bu görevde, sınayacak **StoreManager düzenleme** görünüm sayfası veritabanında gerçekten güncelleştirilmiş albüm verileri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-316">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="5dcc5-317">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-317">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5dcc5-318">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-318">The project starts in the Home page.</span></span> <span data-ttu-id="5dcc5-319">URL'ye değiştirin **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-319">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="5dcc5-320">Albüm başlığını değiştirme **yük** ve tıklayın **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-320">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="5dcc5-321">Albüm başlık gerçekte Albümler listesinde değiştiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-321">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="5dcc5-322">![Albüm güncelleştirme](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "albüm güncelleştiriliyor")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-322">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="5dcc5-323">*Albüm güncelleştiriliyor*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-323">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="5dcc5-324">Alıştırma 4: oluşturma görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-324">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="5dcc5-325">Şimdi **StoreManagerController** destekleyen **Düzenle** özelliği, depolama sağlamak için Create VIEW şablon eklemek nasıl öğrenin Bu alıştırmada yöneticileri uygulamaya yeni Albümler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-325">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="5dcc5-326">Düzenleme işlevselliği ile yaptığınız gibi içinde iki ayrı yöntemlerini kullanarak oluşturma senaryosu gerçekleştireceksiniz **StoreManagerController** sınıfı:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-326">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="5dcc5-327">Bir eylem yöntemi, boş bir form görüntülenir, Mağaza yöneticileri ilk ziyaret ettiğinizde **/StoreManager/oluşturma** URL.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-327">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="5dcc5-328">İkinci bir eylem yöntemi, burada mağaza yöneticisi tıklar senaryo işleyecek **kaydetmek** düğmesini formda ve değerleri yeniden gönderir **/StoreManager/oluşturma** URL bir HTTP POST olarak.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-328">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="5dcc5-329">Görev 1 - HTTP GET oluşturma eylem yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="5dcc5-329">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="5dcc5-330">Bu görevde, tüm türler ve Sanatçılar listesini almak için içine bu veri paketini oluşturma eylem yönteminin HTTP GET sürümünü uygulayacak bir **StoreManagerViewModel** sonra şablonu görüntüleme için geçirilen nesne.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-330">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="5dcc5-331">Açık **başlamak** çözüm bulunan **kaynak/Ex4-AddingACreateView/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-331">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="5dcc5-332">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-332">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5dcc5-333">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-333">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5dcc5-334">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-334">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5dcc5-335">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-335">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5dcc5-336">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-336">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5dcc5-337">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-337">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5dcc5-338">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-338">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5dcc5-339">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-339">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5dcc5-340">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-340">Open **StoreManagerController** class.</span></span> <span data-ttu-id="5dcc5-341">Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-341">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="5dcc5-342">Değiştir **oluşturma** eylem yöntemini aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-342">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="5dcc5-343">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex4 StoreManagerController HTTP GET oluşturma eylem*)</span><span class="sxs-lookup"><span data-stu-id="5dcc5-343">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="5dcc5-344">Görev 2 - oluşturma görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-344">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="5dcc5-345">Bu görevde, yeni (boş) albüm form görüntüler Create VIEW şablon ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-345">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="5dcc5-346">İçinde sağ **oluşturma** eylem yöntemi ve select **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-346">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="5dcc5-347">Bu Görünüm Ekle iletişim kutusunu getirir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-347">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="5dcc5-348">Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-348">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="5dcc5-349">Seçin **kesin türü belirtilmiş görünüm oluşturma** seçeneğini ve **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır ve **Oluştur** gelen **İskele şablonu** açılır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-349">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="5dcc5-350">Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-350">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="5dcc5-351">![Oluştur görünümünün ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "ekleme-a-create-view.png")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-351">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="5dcc5-352">*Oluştur görünümünün ekleme*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-352">*Adding the Create View*</span></span>
3. <span data-ttu-id="5dcc5-353">Güncelleştirme **GenreId** ve **ArtistId** alanları aşağıda gösterildiği gibi açılır listesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-353">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="5dcc5-354">Görev 3 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="5dcc5-354">Task 3 - Running the Application</span></span>

<span data-ttu-id="5dcc5-355">Bu görevde, sınayacak **StoreManager** **oluşturma** görünüm sayfası boş bir albüm formu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-355">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="5dcc5-356">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-356">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5dcc5-357">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-357">The project starts in the Home page.</span></span> <span data-ttu-id="5dcc5-358">URL'ye değiştirin **oluşturma/StoreManager/**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-358">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="5dcc5-359">Yeni albüm özellikleri doldurmak için boş bir form görüntülendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-359">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="5dcc5-360">![Boş bir formla görünümü oluşturma](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "boş bir formla Görünüm Oluştur")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-360">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="5dcc5-361">*Boş bir formla görünümü oluşturma*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-361">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="5dcc5-362">Görev 4 - HTTP POST uygulama oluşturma eylemini yöntemi</span><span class="sxs-lookup"><span data-stu-id="5dcc5-362">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="5dcc5-363">Bu görevde, bir kullanıcı tıkladığında çağrılacak oluşturma eylem yöntemine HTTP POST sürümü gerçekleştireceksiniz **kaydetmek** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-363">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="5dcc5-364">Yöntem yeni albümü veritabanında kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-364">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="5dcc5-365">Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-365">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="5dcc5-366">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-366">Open **StoreManagerController** class.</span></span> <span data-ttu-id="5dcc5-367">Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-367">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="5dcc5-368">Değiştir **HTTP POST oluşturmak** eylem yöntemini aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-368">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="5dcc5-369">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex4 StoreManagerController HTTP POST Oluştur eylemi*)</span><span class="sxs-lookup"><span data-stu-id="5dcc5-369">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="5dcc5-370">Oluşturma eylemini önceki düzenleme eylem yöntemine oldukça benzer, ancak nesne değiştirilmiş olarak ayarlamak yerine, bu bağlamda ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-370">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="5dcc5-371">Görev 5 - Uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="5dcc5-371">Task 5 - Running the Application</span></span>

<span data-ttu-id="5dcc5-372">Bu görevde, sınayacak **StoreManager oluşturma** görünüm sayfası, yeni albümü oluşturmanıza olanak tanır ve StoreManager dizin görünümüne yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-372">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="5dcc5-373">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-373">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5dcc5-374">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-374">The project starts in the Home page.</span></span> <span data-ttu-id="5dcc5-375">URL'ye değiştirin **oluşturma/StoreManager/**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-375">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="5dcc5-376">Tüm form alanlarını, aşağıdaki şekilde bir gibi yeni bir albüm için verilerle doldurun:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-376">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="5dcc5-377">![Albüm oluşturma](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "albüm oluşturma")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-377">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="5dcc5-378">*Albüm oluşturma*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-378">*Creating an Album*</span></span>
3. <span data-ttu-id="5dcc5-379">Yeni oluşturduğunuz yeni albümü içeren StoreManager dizini görünümünü yönlendirilirsiniz doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-379">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="5dcc5-380">![Oluşturulan yeni albümü](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "oluşturulan yeni albümü")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-380">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="5dcc5-381">*Oluşturulan yeni albümü*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-381">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="5dcc5-382">Alıştırma 5: Silme işleme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-382">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="5dcc5-383">Albümleri silme becerisine henüz uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-383">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="5dcc5-384">Bu alıştırmada hakkında olacaktır budur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-384">This is what this exercise will be about.</span></span> <span data-ttu-id="5dcc5-385">İçinde iki ayrı yöntemlerini kullanarak silme senaryoyu önce gerçekleştireceksiniz gibi **StoreManagerController** sınıfı:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-385">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="5dcc5-386">Bir eylem yöntemi bir onay form görüntülenir</span><span class="sxs-lookup"><span data-stu-id="5dcc5-386">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="5dcc5-387">İkinci bir eylem yöntemi form gönderme işleyecek</span><span class="sxs-lookup"><span data-stu-id="5dcc5-387">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="5dcc5-388">Görev 1 - HTTP GET silme eylem yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="5dcc5-388">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="5dcc5-389">Bu görevde, albüm bilgileri almak için Delete eylem yöntemini HTTP GET sürümü gerçekleştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-389">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="5dcc5-390">Açık **başlamak** çözüm bulunan **kaynak/Ex5-HandlingDeletion/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-390">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="5dcc5-391">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-391">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5dcc5-392">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-392">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5dcc5-393">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-393">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5dcc5-394">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-394">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5dcc5-395">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-395">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5dcc5-396">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-396">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5dcc5-397">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-397">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5dcc5-398">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-398">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5dcc5-399">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-399">Open **StoreManagerController** class.</span></span> <span data-ttu-id="5dcc5-400">Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-400">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="5dcc5-401">Delete denetleyici eylemi tam olarak önceki deposu ayrıntıları denetleyici eylemi aynıdır: Bu sorgular **albüm** kullanarak veritabanını nesnesinin **kimliği** sağlanan URL ve döndürür uygun **Görünüm**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-401">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="5dcc5-402">Bunu yapmak için HTTP GET yerini **silmek** eylem yöntemini aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-402">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="5dcc5-403">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex5 işleme silme HTTP GET silme eylem*)</span><span class="sxs-lookup"><span data-stu-id="5dcc5-403">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="5dcc5-404">İçinde sağ **silmek** eylem yöntemi ve select **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-404">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="5dcc5-405">Bu Görünüm Ekle iletişim kutusunu getirir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-405">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="5dcc5-406">Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **silmek**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-406">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="5dcc5-407">Seçin **kesin türü belirtilmiş görünüm oluşturma** seçeneğini ve **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-407">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="5dcc5-408">Seçin **silmek** gelen **İskele şablonu** açılır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-408">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="5dcc5-409">Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-409">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="5dcc5-410">![Delete görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Delete görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-410">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="5dcc5-411">*Delete görünümü ekleme*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-411">*Adding a Delete View*</span></span>
6. <span data-ttu-id="5dcc5-412">Delete şablonu modelden tüm alanları gösterir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-412">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="5dcc5-413">Yalnızca albüm başlık gösterilir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-413">You will show only the album's title.</span></span> <span data-ttu-id="5dcc5-414">Bunu yapmak için görünümü içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-414">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="5dcc5-415">Görev 2 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="5dcc5-415">Task 2 - Running the Application</span></span>

<span data-ttu-id="5dcc5-416">Bu görevde, sınayacak **StoreManager** **silmek** görünüm sayfası onay silme form görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-416">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="5dcc5-417">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-417">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5dcc5-418">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-418">The project starts in the Home page.</span></span> <span data-ttu-id="5dcc5-419">URL'ye değiştirin **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-419">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="5dcc5-420">Tıklayarak silmek için bir albümü seçmek **silmek** ve yeni görünüm yüklendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-420">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="5dcc5-421">![Albüm silme](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "albümü silme")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-421">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="5dcc5-422">*Albüm silme*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-422">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="5dcc5-423">Görev 3-HTTP POST silme eylem yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="5dcc5-423">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="5dcc5-424">Bu görevde, kullanıcı tıkladığında çağrılacak Delete eylem yöntemini HTTP POST sürümü gerçekleştireceksiniz **silmek** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-424">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="5dcc5-425">Yöntemi veritabanında albüm silmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-425">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="5dcc5-426">Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-426">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="5dcc5-427">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-427">Open **StoreManagerController** class.</span></span> <span data-ttu-id="5dcc5-428">Bunu yapmak için sırasıyla **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-428">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="5dcc5-429">Değiştir **HTTP POST silme** eylem yöntemini aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-429">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="5dcc5-430">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex5 işleme silme HTTP POST silme eylem*)</span><span class="sxs-lookup"><span data-stu-id="5dcc5-430">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="5dcc5-431">Görev 4 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="5dcc5-431">Task 4 - Running the Application</span></span>

<span data-ttu-id="5dcc5-432">Bu görevde, sınayacak **StoreManager Delete** görünüm sayfası albüm silmenize olanak sağlar ve StoreManager dizin görünümüne yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-432">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="5dcc5-433">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-433">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5dcc5-434">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-434">The project starts in the Home page.</span></span> <span data-ttu-id="5dcc5-435">URL'ye değiştirin **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-435">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="5dcc5-436">Tıklayarak silmek için bir albümü seçmek **silin.**</span><span class="sxs-lookup"><span data-stu-id="5dcc5-436">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="5dcc5-437">Silme işlemini onaylamak **silmek** düğmesi:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-437">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="5dcc5-438">![Albüm silme](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "albümü silme")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-438">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="5dcc5-439">*Albüm silme*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-439">*Deleting an Album*</span></span>
3. <span data-ttu-id="5dcc5-440">İçinde görünmediğinden albümü silindiğini doğrulayın **dizin** sayfası.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-440">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="5dcc5-441">Alıştırma 6: Doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-441">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="5dcc5-442">Şu anda kullandığınız var oluşturma ve düzenleme forms her türlü doğrulama gerçekleştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-442">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="5dcc5-443">Kullanıcı fiyat alanında gerekli alan boş veya harflerini yazın ayrılsa karşılaşırsınız ilk hata veritabanından olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-443">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="5dcc5-444">Veri ek açıklamaları model sınıfınıza ekleyerek uygulamaya doğrulama ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-444">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="5dcc5-445">ASP.NET MVC, zorlama ve kullanıcılara uygun ileti görüntüleme ilgilenebilmek ve veri ek açıklamaları model özelliklerinizi uygulanan istediğiniz kuralları açıklayan izin verin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-445">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="5dcc5-446">Görev 1 - veri ek açıklamaları ekleme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-446">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="5dcc5-447">Bu görevde, doğrulama iletisi uygun olduğunda veri ek açıklamaları oluşturma ve düzenleme sayfa albüm modeline görüntülemek ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-447">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="5dcc5-448">Basit bir Model sınıfı için bir veri ek açıklama ekleme yalnızca ekleyerek işlenir bir **kullanarak** bildirimi **System.ComponentModel.DataAnnotation**, sonra yerleştirerek bir **[gerekli]**uygun özellikleri özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-448">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="5dcc5-449">Aşağıdaki örnek yapacağı **adı** özelliği görünümünde gerekli bir alan.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-449">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="5dcc5-450">Varlık veri modeli oluşturulduğu bu uygulama gibi durumlarda biraz daha karmaşık budur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-450">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="5dcc5-451">Veri ek açıklamaları model sınıflarına doğrudan eklediyseniz, modeli veritabanından güncelleştir varsa bunların üzerine.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-451">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="5dcc5-452">Bunun yerine, yapabileceğiniz ek açıklamalar tutmak için mevcut olur ve modeli ile ilişkili meta verileri kısmi sınıflarını kullanımını sınıflarını kullanarak **[MetadataType]** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-452">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="5dcc5-453">Açık **başlamak** çözüm bulunan **kaynak/Ex6-AddingValidation/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-453">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="5dcc5-454">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-454">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5dcc5-455">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-455">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5dcc5-456">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-456">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5dcc5-457">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-457">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5dcc5-458">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-458">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5dcc5-459">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-459">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5dcc5-460">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-460">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5dcc5-461">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-461">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5dcc5-462">Açık **Album.cs** gelen **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-462">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="5dcc5-463">Değiştir **Album.cs** aşağıdaki gibi görünüyor. böylece vurgulanmış kodu ile içerik:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-463">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="5dcc5-464">Satır **[DisplayFormat(ConvertEmptyStringToNull=false)]** veri alanı veri kaynağında güncelleştirildiğinde modelden boş dizelerin bir null değerine dönüştürülüp olmaz olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-464">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="5dcc5-465">Veri ek açıklamasını alanları doğrular önce Entity Framework null değerler modele atarken bu ayar bir özel durum kaçının.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-465">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="5dcc5-466">(Kod parçacığını - *ASP.NET MVC 4 yardımcıları ve formlar ve doğrulama - Ex6 albüm meta verileri parçalı sınıf*)</span><span class="sxs-lookup"><span data-stu-id="5dcc5-466">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="5dcc5-467">Bu **albüm** kısmi sınıfına sahip bir **MetadataType** işaret özniteliği **AlbumMetaData** veri ek açıklamaları için sınıf.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-467">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="5dcc5-468">Bunlar, albüm model öğesine açıklama eklemek için kullandığınız veri ek açıklamasını öznitelikleri bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-468">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="5dcc5-469">Gerekli - özelliği gerekli bir alan olduğunu gösterir</span><span class="sxs-lookup"><span data-stu-id="5dcc5-469">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="5dcc5-470">DisplayName - form alanlarını ve doğrulama iletileri kullanılacak metni tanımlar</span><span class="sxs-lookup"><span data-stu-id="5dcc5-470">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="5dcc5-471">DisplayFormat - veri alanları nasıl görüntülenir ve biçimlendirilmiş belirtir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-471">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="5dcc5-472">StringLength - tanımlayan bir dize alanı için en fazla uzunluk</span><span class="sxs-lookup"><span data-stu-id="5dcc5-472">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="5dcc5-473">Aralık - sayısal bir alan için bir maksimum ve minimum değeri verir</span><span class="sxs-lookup"><span data-stu-id="5dcc5-473">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="5dcc5-474">ScaffoldColumn - sağlayan alanları Düzenleyicisi formlarda gizleme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-474">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="5dcc5-475">Görev 2 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="5dcc5-475">Task 2 - Running the Application</span></span>

<span data-ttu-id="5dcc5-476">Bu görevde, oluşturma ve düzenleme sayfaları alanları doğrulamak son görevde seçilen görünen adları kullanarak test.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-476">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="5dcc5-477">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-477">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5dcc5-478">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-478">The project starts in the Home page.</span></span> <span data-ttu-id="5dcc5-479">URL'ye değiştirin **oluşturma/StoreManager/**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-479">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="5dcc5-480">Görünen adları parçalı sınıf Listedekilerin eşleştiğinden emin olun (gibi **albüm resim URL'si** yerine **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="5dcc5-480">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="5dcc5-481">Tıklatın **oluşturma**, formu doldurarak olmadan.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-481">Click **Create**, without filling the form.</span></span> <span data-ttu-id="5dcc5-482">Karşılık gelen doğrulama iletilerini alma doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-482">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="5dcc5-483">![Oluştur sayfası alanlarında doğrulanmış](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Oluştur sayfası alanlarında doğrulandı")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-483">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="5dcc5-484">*Doğrulanmış alanlarında Oluştur sayfası*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-484">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="5dcc5-485">Aynı ile oluşur doğrulayabilirsiniz **Düzenle** sayfası.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-485">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="5dcc5-486">URL'ye değiştirin **/StoreManager/Edit/1** ve görünen adları parçalı sınıf Listedekilerin eşleştiğinden emin olun (gibi **albüm resim URL'si** yerine **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="5dcc5-486">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="5dcc5-487">Boş **başlık** ve **fiyat** alanları ve tıklatın **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-487">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="5dcc5-488">Karşılık gelen doğrulama iletilerini alma doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-488">Verify that you get the corresponding validation messages.</span></span>

    ![Düzenleme sayfasını doğrulanmış alanları](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="5dcc5-490">*Düzenleme sayfasını doğrulanmış alanları*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-490">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="5dcc5-491">Alıştırma 7: İstemci tarafında örtük jQuery kullanma</span><span class="sxs-lookup"><span data-stu-id="5dcc5-491">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="5dcc5-492">Bu alıştırmada, istemci tarafında MVC 4 örtük jQuery doğrulamasını etkinleştirmek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-492">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="5dcc5-493">Örtük jQuery veri ajax önek JavaScript intrusively verme satır içi istemci komut dosyalarını yerine sunucunuzda eylem yöntemleri çağırmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-493">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="5dcc5-494">Görev 1 - etkinleştirme örtük jQuery önce uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="5dcc5-494">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="5dcc5-495">Bu görevde, hem doğrulama modeli karşılaştırmak için jQuery dahil olmak üzere önce uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-495">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="5dcc5-496">Açık **başlamak** çözüm bulunan **kaynak/Ex7-UnobtrusivejQueryValidation/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-496">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="5dcc5-497">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-497">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5dcc5-498">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-498">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5dcc5-499">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-499">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5dcc5-500">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-500">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5dcc5-501">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-501">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5dcc5-502">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-502">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5dcc5-503">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-503">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5dcc5-504">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-504">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5dcc5-505">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-505">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="5dcc5-506">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-506">The project starts in the Home page.</span></span> <span data-ttu-id="5dcc5-507">Gözat **/StoreManager/oluşturma** tıklatıp **oluşturma** doğrulama iletileri almak doğrulamak için formu doldurarak olmadan:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-507">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="5dcc5-508">![İstemci doğrulama devre dışı](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "istemci doğrulama devre dışı")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-508">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="5dcc5-509">*İstemci doğrulama devre dışı*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-509">*Client validation disabled*</span></span>
4. <span data-ttu-id="5dcc5-510">Tarayıcıda, HTML kaynak kodunu açın:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-510">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="5dcc5-511">Görev 2 - etkinleştirme örtük istemci doğrulama</span><span class="sxs-lookup"><span data-stu-id="5dcc5-511">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="5dcc5-512">Bu görevde, jQuery etkinleştirecek **örtük istemci doğrulama** gelen **Web.config** dosya, varsayılan olarak tüm yeni ASP.NET MVC 4 projelerini false değerini alır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-512">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="5dcc5-513">Ayrıca, gerekli jQuery örtük istemci doğrulaması iş yapmak için başvurular komutlar ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-513">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="5dcc5-514">Açık **Web.Config** dosya proje kök dizininde ve olduğundan emin olun **ClientValidationEnabled** ve **UnobtrusiveJavaScriptEnabled** anahtarları değerler için ayarlanır**true**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-514">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="5dcc5-515">Aynı sonuçları almak için Global.asax.cs kodu tarafından istemci doğrulaması da etkinleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-515">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="5dcc5-516">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="5dcc5-516">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="5dcc5-517">Ayrıca, özel davranışı sağlamak için tüm Denetleyicisi'nde ClientValidationEnabled özniteliği atayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-517">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="5dcc5-518">Açık **Create.cshtml** adresindeki **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-518">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="5dcc5-519">Aşağıdaki komut dosyalarını emin olun **jquery.validate** ve **jquery.validate.unobtrusive**, görünüm yoluyla başvurulan &quot; **~/bundles/jqueryval** &quot; paket.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-519">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="5dcc5-520">Bu jQuery kitaplıklar, MVC 4 yeni projelerinde dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-520">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="5dcc5-521">Daha fazla kitaplıklarında bulabilirsiniz **/komut dosyası** , projenin klasör.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-521">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="5dcc5-522">Bu doğrulama yapmak için kitaplıkları iş, jQuery framework kitaplığına bir başvuru eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-522">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="5dcc5-523">Bu başvuru sayfasına eklendiğinden beri  **\_Layout.cshtml** dosyası gerektirmeyen belirli bu görünümde eklemek.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-523">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="5dcc5-524">Görev 3 - uygulama kullanarak örtük jQuery doğrulama çalıştırma</span><span class="sxs-lookup"><span data-stu-id="5dcc5-524">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="5dcc5-525">Bu görevde, sınayacak **StoreManager** şablonu gerçekleştiren kullanıcı yeni albümü oluşturduğunda, jQuery kitaplıkları kullanarak istemci tarafı doğrulama görünüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-525">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="5dcc5-526">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-526">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="5dcc5-527">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-527">The project starts in the Home page.</span></span> <span data-ttu-id="5dcc5-528">Gözat **/StoreManager/oluşturma** tıklatıp **oluşturma** doğrulama iletileri almak doğrulamak için formu doldurarak olmadan:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-528">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="5dcc5-529">![İstemci doğrulama etkin jQuery ile](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "etkin jQuery ile istemci doğrulama")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-529">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="5dcc5-530">*Etkin jQuery ile istemci doğrulama*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-530">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="5dcc5-531">Tarayıcıda oluşturma görünüm için kaynak kodunu açın:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-531">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > <span data-ttu-id="5dcc5-532">Her istemci doğrulama kuralı için örtük jQuery verilerle bir öznitelik ekler-val -*rulename*=&quot;*ileti*&quot;.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-532">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="5dcc5-533">Etiketlerin listesini bu Unobtrusive aşağıdadır jQuery istemci doğrulama gerçekleştirmek için html giriş alanında ekler:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-533">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
    > 
    > - <span data-ttu-id="5dcc5-534">Veri val</span><span class="sxs-lookup"><span data-stu-id="5dcc5-534">Data-val</span></span>
    > - <span data-ttu-id="5dcc5-535">Data-val-number</span><span class="sxs-lookup"><span data-stu-id="5dcc5-535">Data-val-number</span></span>
    > - <span data-ttu-id="5dcc5-536">Veri val aralığı</span><span class="sxs-lookup"><span data-stu-id="5dcc5-536">Data-val-range</span></span>
    > - <span data-ttu-id="5dcc5-537">Veri val aralığı min / val aralığı maksimum veri</span><span class="sxs-lookup"><span data-stu-id="5dcc5-537">Data-val-range-min / Data-val-range-max</span></span>
    > - <span data-ttu-id="5dcc5-538">Veri val gerekiyor</span><span class="sxs-lookup"><span data-stu-id="5dcc5-538">Data-val-required</span></span>
    > - <span data-ttu-id="5dcc5-539">Veri val uzunluğu</span><span class="sxs-lookup"><span data-stu-id="5dcc5-539">Data-val-length</span></span>
    > - <span data-ttu-id="5dcc5-540">Verileri-val-uzunluk-max / verileri-val-uzunluk-min</span><span class="sxs-lookup"><span data-stu-id="5dcc5-540">Data-val-length-max / Data-val-length-min</span></span>
    > 
    > <span data-ttu-id="5dcc5-541">Tüm veri değerleri ile model doldurulur **veri ek açıklamasını**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-541">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="5dcc5-542">Ardından, sunucu tarafında çalışan tüm mantığı istemci tarafında çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-542">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="5dcc5-543">Örneğin, fiyat özniteliği aşağıdaki veri ek açıklamasını modele sahiptir:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-543">For example, Price attribute has the following data annotation in the model:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
    > 
    > <span data-ttu-id="5dcc5-544">Örtük jQuery kullandıktan sonra oluşturulan kodu verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-544">After using Unobtrusive jQuery, the generated code is:</span></span>
    >  
    > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="5dcc5-545">Özet</span><span class="sxs-lookup"><span data-stu-id="5dcc5-545">Summary</span></span>

<span data-ttu-id="5dcc5-546">Bu uygulamalı Laboratuvar tamamlayarak kullanıcılar aşağıdakileri kullanarak veritabanında depolanan verileri değiştirmek etkinleştirme öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="5dcc5-546">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="5dcc5-547">Dizin oluşturma, düzenleme, silme gibi denetleyici eylemleri</span><span class="sxs-lookup"><span data-stu-id="5dcc5-547">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="5dcc5-548">ASP.NET MVC'ın yapı iskelesi özelliği için bir HTML tablosunda özelliklerini görüntüleme</span><span class="sxs-lookup"><span data-stu-id="5dcc5-548">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="5dcc5-549">Kullanıcı artırmak için özel HTML Yardımcıları deneyimi</span><span class="sxs-lookup"><span data-stu-id="5dcc5-549">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="5dcc5-550">HTTP GET veya POST HTTP çağrıları tepki eylem yöntemleri</span><span class="sxs-lookup"><span data-stu-id="5dcc5-550">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="5dcc5-551">Paylaşılan Düzenleyici şablonu oluşturma ve düzenleme gibi benzer görünüm şablonları için</span><span class="sxs-lookup"><span data-stu-id="5dcc5-551">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="5dcc5-552">Aşağı açılan listeler gibi form öğeleri</span><span class="sxs-lookup"><span data-stu-id="5dcc5-552">Form elements like drop-downs</span></span>
- <span data-ttu-id="5dcc5-553">Model doğrulama için veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="5dcc5-553">Data annotations for Model validation</span></span>
- <span data-ttu-id="5dcc5-554">İstemci tarafı jQuery örtük kitaplığını kullanarak doğrulama</span><span class="sxs-lookup"><span data-stu-id="5dcc5-554">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="5dcc5-555">Ek A: Yükleme Web Visual Studio Express 2012 için</span><span class="sxs-lookup"><span data-stu-id="5dcc5-555">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="5dcc5-556">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak  **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="5dcc5-556">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="5dcc5-557">Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-557">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="5dcc5-558">Git [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="5dcc5-558">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="5dcc5-559">Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; *Visual Studio Express 2012 için Windows Azure SDK'sı Web*&quot;.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-559">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="5dcc5-560">Tıklayın **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-560">Click on **Install Now**.</span></span> <span data-ttu-id="5dcc5-561">Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-561">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="5dcc5-562">Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-562">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="5dcc5-563">![Visual Studio Express yükleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "yükleme Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-563">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="5dcc5-564">*Visual Studio Express yükleme*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-564">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="5dcc5-565">Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-565">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşulları kabul ediliyor](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="5dcc5-567">*Lisans koşulları kabul ediliyor*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-567">*Accepting the license terms*</span></span>
5. <span data-ttu-id="5dcc5-568">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-568">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="5dcc5-570">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-570">*Installation progress*</span></span>
6. <span data-ttu-id="5dcc5-571">Yükleme tamamlandığında tıklatın **son**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-571">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="5dcc5-573">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-573">*Installation completed*</span></span>
7. <span data-ttu-id="5dcc5-574">Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-574">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="5dcc5-575">Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-575">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web döşemeye](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="5dcc5-577">*VS Express Web döşemeye*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-577">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="5dcc5-578">Ek B: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="5dcc5-578">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="5dcc5-579">Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-579">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="5dcc5-580">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-580">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="5dcc5-581">![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-581">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="5dcc5-582">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-582">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="5dcc5-583">***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="5dcc5-583">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="5dcc5-584">İmleci, burada kod eklemek istediğiniz yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-584">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="5dcc5-585">(Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-585">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="5dcc5-586">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-586">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="5dcc5-587">Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="5dcc5-587">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="5dcc5-588">İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-588">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="5dcc5-589">![Kod parçacığında adını yazmaya başlayın](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-589">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="5dcc5-590">*Kod parçacığında adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-590">*Start typing the snippet name*</span></span>

<span data-ttu-id="5dcc5-591">![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-591">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="5dcc5-592">*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-592">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="5dcc5-593">![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-593">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="5dcc5-594">*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-594">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="5dcc5-595">***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-595">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="5dcc5-596">Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-596">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="5dcc5-597">Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-597">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="5dcc5-598">Tıklayarak ilgili kod parçacığında listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="5dcc5-598">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="5dcc5-599">![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-599">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="5dcc5-600">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-600">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="5dcc5-601">![Tıklayarak ilgili kod parçacığında listeden çekme](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "tıklayarak ilgili kod parçacığında listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="5dcc5-601">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="5dcc5-602">*Tıklayarak ilgili kod parçacığında listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="5dcc5-602">*Pick the relevant snippet from the list, by clicking on it*</span></span>
