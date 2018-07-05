---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 yardımcılar, formlar ve doğrulama | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 modelleri ve veri erişim uygulamalı laboratuvarı, size alınan yükleniyor ve veritabanından veri görüntüleme. Bu uygulamalı laboratuvarda, ekler...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: f2eb624e72d6f52d1694b5753ee2b1f8117c2851
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376743"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="489f7-104">ASP.NET MVC 4 yardımcılar, formlar ve doğrulama</span><span class="sxs-lookup"><span data-stu-id="489f7-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="489f7-105">Tarafından [Team Web Kampları](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="489f7-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="489f7-106">Eğitim Seti Web Kampları indirin</span><span class="sxs-lookup"><span data-stu-id="489f7-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="489f7-107">İçinde **ASP.NET MVC 4 modelleri ve verilere erişim** Hands-on-Lab, yükleme ve veritabanından veri görüntüleme olmuştur.</span><span class="sxs-lookup"><span data-stu-id="489f7-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="489f7-108">Bu uygulamalı laboratuvarda için ekleyeceksiniz **müzik Store** uygulama bu verileri düzenleme olanağı.</span><span class="sxs-lookup"><span data-stu-id="489f7-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="489f7-109">Bu hedefe aklınızda Albümler oluşturma, okuma, güncelleştirme ve silme (CRUD) eylemleri destekleyen denetleyici önce oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="489f7-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="489f7-110">ASP.NET MVC'nin iskele kurma özelliği ile HTML tablosu halinde albümleri özelliklerini görüntülemek için yararlanarak bir dizin görünümünün şablonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="489f7-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="489f7-111">Bu görünümü geliştirmek için uzun açıklamaları kısaltabilir özel bir HTML Yardımcısı ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="489f7-112">Daha sonra düzenleme ve oluşturma olanak tanıyan görünümleri bırakmalar gibi form öğelerin yardımıyla veritabanında albümleri alter ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="489f7-113">Son olarak, kullanıcıların albümü silme sağlar ve ayrıca, bunları yanlış veri girişi doğrulayarak girmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="489f7-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="489f7-114">Bu uygulamalı laboratuvarı temel bilgiye sahip olduğunuz varsayılır **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="489f7-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="489f7-115">Kullanmadıysanız, **ASP.NET MVC** gitmenizi öneririz önce **ASP.NET MVC ile ilgili temel bilgiler** Hands-on-Lab.</span><span class="sxs-lookup"><span data-stu-id="489f7-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="489f7-116">Bu Laboratuvar, küçük değişiklikler kaynak klasördeki sağlanan örnek bir Web uygulamasına uygulayarak daha önce açıklanan yeni özellikler ve iyileştirmeler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="489f7-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="489f7-117">Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [Microsoft-Web/WebCampTrainingKit yayınlar](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="489f7-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="489f7-118">Bu Laboratuvar için belirli proje kullanılabilir [ASP.NET MVC 4 yardımcılar, formlar ve doğrulama](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="489f7-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="489f7-119">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="489f7-119">Objectives</span></span>

<span data-ttu-id="489f7-120">Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="489f7-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="489f7-121">CRUD işlemlerini desteklemek için bir denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="489f7-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="489f7-122">HTML tablosu halinde varlık özelliklerini görüntülemek için bir dizin görünümü oluştur</span><span class="sxs-lookup"><span data-stu-id="489f7-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="489f7-123">Bir özel HTML Yardımcısı Ekle</span><span class="sxs-lookup"><span data-stu-id="489f7-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="489f7-124">Oluşturma ve özelleştirme Görünümü Düzenle</span><span class="sxs-lookup"><span data-stu-id="489f7-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="489f7-125">HTTP GET veya POST HTTP çağrıları için react eylem yöntemleri birbirinden ayırt</span><span class="sxs-lookup"><span data-stu-id="489f7-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="489f7-126">Ekleme ve özelleştirme Görünüm Oluştur</span><span class="sxs-lookup"><span data-stu-id="489f7-126">Add and customize a Create View</span></span>
- <span data-ttu-id="489f7-127">Tanıtıcı bir varlığı silme</span><span class="sxs-lookup"><span data-stu-id="489f7-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="489f7-128">Kullanıcı girişini doğrulama</span><span class="sxs-lookup"><span data-stu-id="489f7-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="489f7-129">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="489f7-129">Prerequisites</span></span>

<span data-ttu-id="489f7-130">Bu laboratuvarı tamamlamak için aşağıdakiler olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="489f7-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="489f7-131">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya üst (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="489f7-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="489f7-132">Kurulum</span><span class="sxs-lookup"><span data-stu-id="489f7-132">Setup</span></span>

<span data-ttu-id="489f7-133">**Kod parçacıkları yükleniyor**</span><span class="sxs-lookup"><span data-stu-id="489f7-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="489f7-134">Kolaylık olması için bu Laboratuvar yöneteceğiniz kodun çoğu Visual Studio kod parçacıkları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="489f7-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="489f7-135">Kod parçacıklarını çalıştırmak yüklenecek **.\Source\Setup\CodeSnippets.vsi** dosya.</span><span class="sxs-lookup"><span data-stu-id="489f7-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="489f7-136">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istediğiniz konusunda bilgi sahibi değilseniz, bu belge, ek başvurabilir &quot; [ek B: kullanarak kod parçacıklarına](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="489f7-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="489f7-137">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="489f7-137">Exercises</span></span>

<span data-ttu-id="489f7-138">Bu uygulamalı laboratuvarı ayarlama aşağıdaki alıştırmalar yapın:</span><span class="sxs-lookup"><span data-stu-id="489f7-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="489f7-139">Store Yöneticisi denetleyicisi ve onun dizini görünümünü oluşturma</span><span class="sxs-lookup"><span data-stu-id="489f7-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="489f7-140">Bir HTML Yardımcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="489f7-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="489f7-141">Düzenleme görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="489f7-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="489f7-142">Bir oluşturma görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="489f7-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="489f7-143">İşleme silme</span><span class="sxs-lookup"><span data-stu-id="489f7-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="489f7-144">Doğrulama Ekleme</span><span class="sxs-lookup"><span data-stu-id="489f7-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="489f7-145">İstemci tarafında örtük jQuery kullanma</span><span class="sxs-lookup"><span data-stu-id="489f7-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="489f7-146">Her bir alıştırma olarak sunulduğu bir **son** elde alıştırmalar tamamladıktan sonra ortaya çıkan çözüm içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="489f7-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="489f7-147">Çalışma alıştırmaları ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="489f7-148">Bu laboratuvarı tamamlamak için tahmini süre: **60 dakika**</span><span class="sxs-lookup"><span data-stu-id="489f7-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="489f7-149">Alıştırma 1: Store Yöneticisi denetleyicisi ve onun dizini görünümünü oluşturma</span><span class="sxs-lookup"><span data-stu-id="489f7-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="489f7-150">Bu alıştırmada, veritabanını ve son olarak ASP.NET MVC'nin yapı iskelesi yararlanarak bir dizin görünümünün şablonu oluşturulurken albümleri listesini döndürmek için dizin eylem yöntemini Özelleştir CRUD işlemlerini desteklemek için yeni bir denetleyici nasıl oluşturulacağını öğreneceksiniz. HTML tablosu halinde albümleri özelliklerini görüntülemek için özellik.</span><span class="sxs-lookup"><span data-stu-id="489f7-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="489f7-151">Görev 1 - StoreManagerController oluşturma</span><span class="sxs-lookup"><span data-stu-id="489f7-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="489f7-152">Bu görevde, adlı yeni bir denetleyici oluşturacaksınız **StoreManagerController** CRUD işlemlerini desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="489f7-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="489f7-153">Açık **başlamak** çözüm bulunan **kaynak/Ex1-CreatingTheStoreManagerController/başlangıç/** klasör.</span><span class="sxs-lookup"><span data-stu-id="489f7-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="489f7-154">Bazı eksik NuGet paketleri indirmeniz gerekecek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="489f7-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="489f7-155">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="489f7-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="489f7-156">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="489f7-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="489f7-157">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="489f7-158">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="489f7-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="489f7-159">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="489f7-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="489f7-160">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="489f7-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="489f7-161">Yeni bir denetleyici ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-161">Add a new controller.</span></span> <span data-ttu-id="489f7-162">Bunu yapmak için sağ **denetleyicileri** klasör Çözüm Gezgini'nde, seçme içinde **Ekle** ardından **denetleyicisi** komutu.</span><span class="sxs-lookup"><span data-stu-id="489f7-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="489f7-163">Değişiklik **denetleyicisi** **adı** için **StoreManagerController** seçeneği emin **boşokuma/yazmaeylemleriileMVCdenetleyicisi**seçilir.</span><span class="sxs-lookup"><span data-stu-id="489f7-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="489f7-164">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="489f7-164">Click **Add**.</span></span>

    <span data-ttu-id="489f7-165">![Ekleme denetleyicisi iletişim](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "ekleme denetleyicisi iletişim")</span><span class="sxs-lookup"><span data-stu-id="489f7-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="489f7-166">*Denetleyici Ekle iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="489f7-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="489f7-167">Yeni bir denetleyici sınıfı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="489f7-167">A new Controller class is generated.</span></span> <span data-ttu-id="489f7-168">Okuma/yazma, kişiler, saptama yöntemleri için Eylemler eklemek için belirtilen beri yaygın CRUD eylemleri uygulama belirli mantığı eklemek isteyen doldurulur TODO yorumları ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="489f7-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="489f7-169">Görev 2 - StoreManager dizini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="489f7-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="489f7-170">Bu görevde, veritabanından albümleri listesini içeren bir görünümü döndürülecek StoreManager dizin eylem yönteminin özelleştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="489f7-171">StoreManagerController sınıfında, aşağıdaki ekleyin *kullanarak* yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="489f7-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="489f7-172">(Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex1 MvcMusicStore kullanarak*)</span><span class="sxs-lookup"><span data-stu-id="489f7-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="489f7-173">Bir alan ekleyerek **StoreManagerController** örneği tutacak **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="489f7-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="489f7-174">(Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="489f7-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="489f7-175">Albümleri listesini içeren bir görünüme dönmek için StoreManagerController dizin eylemi uygulayın.</span><span class="sxs-lookup"><span data-stu-id="489f7-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="489f7-176">Denetleyici eylem mantıksal StoreController'ın dizin eylemini daha önce yazılmış çok benzer olacaktır.</span><span class="sxs-lookup"><span data-stu-id="489f7-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="489f7-177">LINQ tarz ve sanatçı bilgilerini görüntülemek için de dahil olmak üzere tüm Albümler almak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="489f7-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="489f7-178">(Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex1 StoreManagerController dizin*)</span><span class="sxs-lookup"><span data-stu-id="489f7-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="489f7-179">Görev 3 - dizin görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="489f7-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="489f7-180">Bu görevde, dizin görünümünün şablonu tarafından döndürülen albümleri listesini görüntülemek için oluşturacağınız **StoreManager** denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="489f7-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="489f7-181">Yeni Görünüm şablonu oluşturmadan önce projeyi oluşturmalısınız böylece **Ekle iletişim kutusunu görüntüle** bildiği **albüm** kullanılacak sınıfı.</span><span class="sxs-lookup"><span data-stu-id="489f7-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="489f7-182">Seçin **yapı | Derleme MvcMusicStore** Projeyi derlemek için.</span><span class="sxs-lookup"><span data-stu-id="489f7-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="489f7-183">İçinde sağ **dizin** eylem yöntemini seçip alt **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="489f7-184">Bu getirir **Görünüm Ekle** iletişim.</span><span class="sxs-lookup"><span data-stu-id="489f7-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="489f7-185">![Görünüm Ekle](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="489f7-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="489f7-186">*Index yöntemi içinden bir görünümle ekleme*</span><span class="sxs-lookup"><span data-stu-id="489f7-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="489f7-187">Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **dizin**.</span><span class="sxs-lookup"><span data-stu-id="489f7-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="489f7-188">Seçin **kesin türü belirtilmiş görünüm oluşturmak** seçeneğini belirtin ve **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır.</span><span class="sxs-lookup"><span data-stu-id="489f7-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="489f7-189">Seçin **listesi** gelen **İskele şablon** açılır.</span><span class="sxs-lookup"><span data-stu-id="489f7-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="489f7-190">Bırakın **görünüm altyapısı** için **Razor** ve diğer alanları varsayılan değer ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="489f7-191">![Bir dizini görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "dizini görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="489f7-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="489f7-192">*Bir dizini görünümü ekleme*</span><span class="sxs-lookup"><span data-stu-id="489f7-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="489f7-193">Görev 4 - dizin görünümünün iskele özelleştirme</span><span class="sxs-lookup"><span data-stu-id="489f7-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="489f7-194">Bu görevde, Basit Görünüm şablonu istediğiniz alanları görüntülemek için ASP.NET MVC iskele kurma özelliği ile oluşturulan uyum sağlar.</span><span class="sxs-lookup"><span data-stu-id="489f7-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="489f7-195">**Yapı iskelesi** içinde ASP.NET MVC desteği albüm modelinde tüm alanları listeler basit bir görünüm şablonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="489f7-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="489f7-196">**Yapı iskelesi** türü kesin belirlenmiş bir görünüm üzerinde kullanmaya başlamak için hızlı bir yol sunar: görünüm şablonu el ile yazmak zorunda kalmak yerine hızlı bir şekilde iskele kurma özelliği bir varsayılan şablon oluşturur ve oluşturulan kodun daha sonra değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="489f7-197">Oluşturulan kodu gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="489f7-197">Review the code created.</span></span> <span data-ttu-id="489f7-198">Oluşturulan alanlar listesi aşağıdaki bir parçası olacak HTML tablosu **yapı İskelesi** tablosal verileri görüntülemek için kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="489f7-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="489f7-199">Değiştirin **&lt;tablo&gt;** yalnızca görüntülemek için aşağıdaki kod ile kod **Tarz**, **sanatçının**, **albümbaşlığı**, ve **fiyat** alanları.</span><span class="sxs-lookup"><span data-stu-id="489f7-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="489f7-200">Bu siler **AlbumId** ve **albüm resim URL'si** sütunları.</span><span class="sxs-lookup"><span data-stu-id="489f7-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="489f7-201">Ayrıca, kendi bağlantılı sınıf özelliklerini görüntülemek için GenreId ve ArtistId sütunları değiştirirse **Artist.Name** ve **Genre.Name**ve kaldırır **ayrıntıları** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="489f7-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="489f7-202">Aşağıdaki tanımlamalar değiştirir.</span><span class="sxs-lookup"><span data-stu-id="489f7-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="489f7-203">Görev 5 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="489f7-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="489f7-204">Bu görevde, sınayacak **StoreManager** **dizin** görünüm şablonu albümleri önceki adımları tasarımını göre bir listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="489f7-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="489f7-205">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="489f7-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="489f7-206">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="489f7-206">The project starts in the Home page.</span></span> <span data-ttu-id="489f7-207">URL'yi **/StoreManager** gösteren albümleri listesini görüntülendiğini doğrulamak için kendi **başlık**, **sanatçının** ve **Tarz**.</span><span class="sxs-lookup"><span data-stu-id="489f7-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="489f7-208">![Albümleri biri listeyi gözden geçireceğiniz](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "albümleri listesini gözatma")</span><span class="sxs-lookup"><span data-stu-id="489f7-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="489f7-209">*Gözatma albümleri listesi*</span><span class="sxs-lookup"><span data-stu-id="489f7-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="489f7-210">Alıştırma 2: bir HTML Yardımcısı ekleme</span><span class="sxs-lookup"><span data-stu-id="489f7-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="489f7-211">Bir olası sorun StoreManager dizin sayfası vardır: Başlık ve sanatçı adı özellikleri hem de olabilir tablo biçimlendirmeyi devre dışı throw yetecek kadar uzun.</span><span class="sxs-lookup"><span data-stu-id="489f7-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="489f7-212">Bu alıştırmada, metnin kesin bir özel HTML Yardımcısı ekleme öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="489f7-213">Aşağıdaki resimde, bir küçük tarayıcı boyutu kullandığınızda biçimi nedeniyle metnin uzunluğunu nasıl değiştirilir görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="489f7-214">![Albümleri listeyi gözden geçireceğiniz değil metin kesilmiş](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "albümleri listeyi gözden geçireceğiniz değil metin kesiliyor")</span><span class="sxs-lookup"><span data-stu-id="489f7-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="489f7-215">*Albümleri listeyi gözden geçireceğiniz metin kesilmiş değil*</span><span class="sxs-lookup"><span data-stu-id="489f7-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="489f7-216">Görev 1 - HTML Yardımcısı genişletme</span><span class="sxs-lookup"><span data-stu-id="489f7-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="489f7-217">Bu görevde, yeni bir yöntem ekleyeceksiniz **Truncate** için **HTML** ASP.NET MVC görünümleri içinde kullanıma sunulan nesne.</span><span class="sxs-lookup"><span data-stu-id="489f7-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="489f7-218">Bunu yapmak için uygular bir **genişletme yöntemi** için yerleşik **System.Web.Mvc.HtmlHelper** ASP.NET MVC tarafından sağlanan sınıfı.</span><span class="sxs-lookup"><span data-stu-id="489f7-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="489f7-219">Hakkında daha fazla bilgi edinmek için **genişletme yöntemleri**, lütfen bu msdn makalesine bakın.</span><span class="sxs-lookup"><span data-stu-id="489f7-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="489f7-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="489f7-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="489f7-221">Açık **başlamak** çözüm bulunan **kaynak/Ex2-AddingAnHTMLHelper/başlangıç/** klasör.</span><span class="sxs-lookup"><span data-stu-id="489f7-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="489f7-222">Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="489f7-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="489f7-223">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="489f7-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="489f7-224">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="489f7-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="489f7-225">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="489f7-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="489f7-226">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="489f7-227">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="489f7-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="489f7-228">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="489f7-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="489f7-229">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="489f7-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="489f7-230">StoreManager'ın dizini görünümünü açın.</span><span class="sxs-lookup"><span data-stu-id="489f7-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="489f7-231">Bunu yapmak için Çözüm Gezgini'nde genişletin **görünümleri** klasörü, ardından **StoreManager** açın **Index.cshtml** dosya.</span><span class="sxs-lookup"><span data-stu-id="489f7-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="489f7-232">Aşağıdaki kodu ekleyin <strong>@model</strong> tanımlamak için yönergesi <strong>Truncate</strong> yardımcı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="489f7-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="489f7-233">Görev 2 - sayfasında kesiliyor metin</span><span class="sxs-lookup"><span data-stu-id="489f7-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="489f7-234">Bu görevde kullanacağınız **Truncate** truncate şablonu görüntüleme metni için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="489f7-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="489f7-235">StoreManager'ın dizini görünümünü açın.</span><span class="sxs-lookup"><span data-stu-id="489f7-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="489f7-236">Bunu yapmak için Çözüm Gezgini'nde genişletin **görünümleri** klasörü, ardından **StoreManager** açın **Index.cshtml** dosya.</span><span class="sxs-lookup"><span data-stu-id="489f7-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="489f7-237">Gösteren satırları değiştirin **sanatçı adı** ve albüm **başlık**.</span><span class="sxs-lookup"><span data-stu-id="489f7-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="489f7-238">Bunu yapmak için aşağıdaki satırları değiştirin.</span><span class="sxs-lookup"><span data-stu-id="489f7-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="489f7-239">Görev 3 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="489f7-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="489f7-240">Bu görevde, sınayacak **StoreManager** **dizin** görünüm şablonu albüm başlık ve sanatçı adı keser.</span><span class="sxs-lookup"><span data-stu-id="489f7-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="489f7-241">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="489f7-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="489f7-242">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="489f7-242">The project starts in the Home page.</span></span> <span data-ttu-id="489f7-243">URL'yi **/StoreManager** bu uzun doğrulamak için metinleri **başlık** ve **sanatçının** sütun kesilir.</span><span class="sxs-lookup"><span data-stu-id="489f7-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="489f7-244">![Kesilmiş başlıklar ve sanatçıların adları](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "kesilmiş başlıklar ve sanatçıların adları")</span><span class="sxs-lookup"><span data-stu-id="489f7-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="489f7-245">*Kesilmiş başlıklar ve sanatçının adları*</span><span class="sxs-lookup"><span data-stu-id="489f7-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="489f7-246">Alıştırma 3: düzenleme görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="489f7-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="489f7-247">Bu alıştırmada, albüm düzenlemek Mağaza yöneticileri izin vermek için bir formun nasıl oluşturulacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="489f7-248">Kullanıcıların göz **/StoreManager/Edit/id** URL (**kimliği** düzenlemek için albümü benzersiz kimliği olan), bu nedenle sunucunun bir HTTP GET çağrı yapma.</span><span class="sxs-lookup"><span data-stu-id="489f7-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="489f7-249">Denetleyici eylem yöntemi Düzenle veritabanından uygun albümü, oluşturun bir **StoreManagerViewModel** nesne (bir liste sanatçıların ve türleri ile birlikte) kapsüllemek ve ardından bunu bir şablonu görüntüle geçirmek devre dışı HTML sayfasının, kullanıcıya geri işleyin.</span><span class="sxs-lookup"><span data-stu-id="489f7-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="489f7-250">Bu sayfa içerecek bir **&lt;form&gt;** metin kutuları ve albüm özelliklerini düzenlemek için açılan öğe.</span><span class="sxs-lookup"><span data-stu-id="489f7-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="489f7-251">Kullanıcı albüm form değerlerini güncelleştirir ve tıkladığında sonra **Kaydet** düğmesi, değişiklikleri aracılığıyla gönderilir bir HTTP POST geri çağırma **/StoreManager/Edit/id**. ASP.NET MVC URL son çağrının olduğu gibi aynı kalsa da, bir HTTP POST ve bu nedenle farklı bir düzen eylem yöntemini yürütür. Bu süre tanımlayan (bir düzenlenmiş **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="489f7-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="489f7-252">Görev 1 - HTTP-GET düzenleme eylem yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="489f7-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="489f7-253">Bu görevde, HTTP GET sürüm uygun albümü veritabanından alınacak düzenleme eylem yönteminin yanı sıra, tüm türleri ve sanatçıların listesini uygular.</span><span class="sxs-lookup"><span data-stu-id="489f7-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="489f7-254">Bu verileri içine paketi **StoreManagerViewModel** ardından Yanıtla işlemek için bir görünüm şablonu geçirilir son adımda tanımlanan nesne.</span><span class="sxs-lookup"><span data-stu-id="489f7-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="489f7-255">Açık **başlamak** çözüm bulunan **kaynak/Ex3-CreatingTheEditView/başlangıç/** klasör.</span><span class="sxs-lookup"><span data-stu-id="489f7-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="489f7-256">Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="489f7-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="489f7-257">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="489f7-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="489f7-258">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="489f7-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="489f7-259">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="489f7-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="489f7-260">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="489f7-261">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="489f7-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="489f7-262">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="489f7-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="489f7-263">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="489f7-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="489f7-264">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="489f7-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="489f7-265">Bunu yapmak için genişletme **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="489f7-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="489f7-266">Değiştirin **HTTP GET Düzenle** uygun almak için aşağıdaki kodu eylem yöntemine **albüm** yanı sıra **türleri** ve **Sanatçılar**listeler.</span><span class="sxs-lookup"><span data-stu-id="489f7-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="489f7-267">(Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - eylemini Ex3 StoreManagerController HTTP GET Düzenle*)</span><span class="sxs-lookup"><span data-stu-id="489f7-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="489f7-268">Kullanmakta olduğunuz **System.Web.Mvc** **SelectList** sanatçıların ve türleri yerine **System.Collections.Generic** listesi.</span><span class="sxs-lookup"><span data-stu-id="489f7-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="489f7-269">**SelectList** HTML bırakmalar doldurmak ve geçerli seçimi gibi şeyleri yönetmek için temiz bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="489f7-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="489f7-270">Örnekleme ve bu denetleyici eylemini ViewModel nesnelere sonraki ayarlama düzenleme formu senaryo temizleyici hale getirir.</span><span class="sxs-lookup"><span data-stu-id="489f7-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="489f7-271">Görev 2 - düzenleme görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="489f7-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="489f7-272">Bu görevde, daha sonra Albüm özellikleri görüntüleyecek bir görünümü düzenleme şablonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="489f7-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="489f7-273">Düzenleme görünümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="489f7-273">Create the Edit View.</span></span> <span data-ttu-id="489f7-274">İçinde Bunu yapmak için sağ **Düzenle** eylem yöntemini seçip alt **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="489f7-275">Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="489f7-276">Denetleyin **kesin türü belirtilmiş görünüm oluşturmak** onay kutusunu seçip **albüm (MvcMusicStore.Models)** gelen **görüntülemek veri sınıfı** açılır.</span><span class="sxs-lookup"><span data-stu-id="489f7-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="489f7-277">Seçin **Düzenle** gelen **İskele şablon** açılır.</span><span class="sxs-lookup"><span data-stu-id="489f7-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="489f7-278">Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="489f7-279">![Düzenleme görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "düzenleme görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="489f7-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="489f7-280">*Düzenleme görünümü ekleme*</span><span class="sxs-lookup"><span data-stu-id="489f7-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="489f7-281">Görev 3 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="489f7-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="489f7-282">Bu görevde, sınayacak **StoreManager** **Düzenle** görünüm sayfası parametre olarak geçirilen albümü özelliklerin değerlerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="489f7-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="489f7-283">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="489f7-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="489f7-284">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="489f7-284">The project starts in the Home page.</span></span> <span data-ttu-id="489f7-285">URL'yi **/StoreManager/Edit/1** geçirilen albümü özelliklerin değerlerini görüntülendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="489f7-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="489f7-286">![Albüm düzenleme görünümünü gözatma](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "albüm düzenleme görünümünü gözatma")</span><span class="sxs-lookup"><span data-stu-id="489f7-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="489f7-287">*Albüm düzenleme görünümünü gözatma*</span><span class="sxs-lookup"><span data-stu-id="489f7-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="489f7-288">Görev 4 - açılan listeler albüm Düzenleyicisi şablon uygulama</span><span class="sxs-lookup"><span data-stu-id="489f7-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="489f7-289">Sanatçıların ve türleri listesinden seçebilmeniz için bu görevde, açılan listeler son görevde oluşturduğunuz görünüm şablonu ekler.</span><span class="sxs-lookup"><span data-stu-id="489f7-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="489f7-290">Tümünü Değiştir **albüm** fieldset kodu şu kodla:</span><span class="sxs-lookup"><span data-stu-id="489f7-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="489f7-291">Bir **Html.DropDownList** Yardımcısı, sanatçıların ve tür seçmek için açılır listeleri işlemek için eklendi.</span><span class="sxs-lookup"><span data-stu-id="489f7-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="489f7-292">Geçirilen parametreleri **Html.DropDownList** şunlardır:</span><span class="sxs-lookup"><span data-stu-id="489f7-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="489f7-293">Form alanı adını (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="489f7-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="489f7-294">**SelectList** aşağı açılan değer.</span><span class="sxs-lookup"><span data-stu-id="489f7-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="489f7-295">Görev 5 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="489f7-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="489f7-296">Bu görevde, sınayacak **StoreManager** **Düzenle** görünümü sayfasında görüntüler sanatçı ve Tarz kimliği metin alanları yerine açılan listeler.</span><span class="sxs-lookup"><span data-stu-id="489f7-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="489f7-297">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="489f7-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="489f7-298">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="489f7-298">The project starts in the Home page.</span></span> <span data-ttu-id="489f7-299">URL'yi **/StoreManager/Edit/1** açılan listeler sanatçı ve Tarz kimliği metin alanları yerine görüntülediğini doğrulamak üzere.</span><span class="sxs-lookup"><span data-stu-id="489f7-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="489f7-300">![Albüm düzenleme görünümü ile açılan listeler gözatma](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "gözatma albüm görünümü açılan listeler ile Düzenle")</span><span class="sxs-lookup"><span data-stu-id="489f7-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="489f7-301">*Albüm düzenleme görünümü açılır menüleri kullanarak bu sefer gözatma*</span><span class="sxs-lookup"><span data-stu-id="489f7-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="489f7-302">Görev 6 - HTTP-POST Düzenle eylem yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="489f7-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="489f7-303">Düzenleme görünümü beklendiği gibi görüntüler, albümü yaptığınız değişiklikleri kaydetmek için HTTP POST eylemini Düzenle yöntemi uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="489f7-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="489f7-304">Gerekirse Visual Studio penceresine dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="489f7-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="489f7-305">Açık **StoreManagerController** gelen **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="489f7-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="489f7-306">Değiştirin **HTTP POST Düzenle** eylem yöntemi kodu şu kodla (değiştirilmesi gereken yöntemini iki parametre alan Aşırı yüklenen sürümü olduğunu unutmayın):</span><span class="sxs-lookup"><span data-stu-id="489f7-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="489f7-307">(Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - eylemini Ex3 StoreManagerController HTTP POST Düzenle*)</span><span class="sxs-lookup"><span data-stu-id="489f7-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="489f7-308">Kullanıcı tıkladığında bu yöntem yürütülecek **Kaydet** görünümün düğme ve bir HTTP veritabanında kalıcı hale getirmek için POST form değerlerinin sunucuya geri gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="489f7-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="489f7-309">Dekoratörün **[HttpPost]** yöntemi bu HTTP POST senaryoları için kullanılması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="489f7-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="489f7-310">Yöntem alır bir **albüm** nesne.</span><span class="sxs-lookup"><span data-stu-id="489f7-310">The method takes an **Album** object.</span></span> <span data-ttu-id="489f7-311">ASP.NET MVC otomatik olarak oluşturur albüm nesne deftere nakledilen &lt;form&gt; değerleri.</span><span class="sxs-lookup"><span data-stu-id="489f7-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="489f7-312">Yöntemi, aşağıdaki adımları gerçekleştireceksiniz:</span><span class="sxs-lookup"><span data-stu-id="489f7-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="489f7-313">Model geçerliyse:</span><span class="sxs-lookup"><span data-stu-id="489f7-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="489f7-314">Değiştirilen bir nesnesi olarak işaretlemek için bağlam albüm girişi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="489f7-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="489f7-315">Değişiklikleri kaydetmek ve dizin görünümüne yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="489f7-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="489f7-316">Model geçerli değilse ViewBag ile doldurulur **GenreId** ve **ArtistId**, izin vermek için alınan albümü nesnenin görünümüyle döndürecektir herhangi bir gerekli güncelleştirme gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="489f7-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="489f7-317">Görev 7 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="489f7-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="489f7-318">Bu görevde, sınayacak **StoreManager düzenleme** görünüm sayfası gerçekten veritabanında güncelleştirilmiş albüm verileri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="489f7-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="489f7-319">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="489f7-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="489f7-320">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="489f7-320">The project starts in the Home page.</span></span> <span data-ttu-id="489f7-321">URL'yi **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="489f7-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="489f7-322">Albüm başlığı değiştirmek **yük** tıklayın **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="489f7-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="489f7-323">Albüm başlık gerçekten albümleri listesinde değiştiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="489f7-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="489f7-324">![Albüm güncelleştirme](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "albüm güncelleştiriliyor")</span><span class="sxs-lookup"><span data-stu-id="489f7-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="489f7-325">*Albüm güncelleştiriliyor*</span><span class="sxs-lookup"><span data-stu-id="489f7-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="489f7-326">Alıştırma 4: bir oluşturma görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="489f7-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="489f7-327">Şimdi **StoreManagerController** destekler **Düzenle** özelliği, bu alıştırmada depolamak için bir görünüm oluştur şablonu ekleme öğreneceksiniz yöneticilerini yeni Albümler uygulamaya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="489f7-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="489f7-328">Düzenleme işlevsellikle yaptığınız gibi iki farklı yöntemle içinde oluşturma senaryosu uygular **StoreManagerController** sınıfı:</span><span class="sxs-lookup"><span data-stu-id="489f7-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="489f7-329">Bir eylem yöntemi, boş bir form görüntülenir, depolama yöneticilerinin ilk kez ziyaret ettiğinizde **/StoreManager/Create** URL'si.</span><span class="sxs-lookup"><span data-stu-id="489f7-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="489f7-330">İkinci bir eylem yöntemi burada mağaza yöneticisi tıkladığında senaryo işleyecek **Kaydet** düğmesi, formda ve değerlerin geri gönderir **/StoreManager/Create** olarak bir HTTP POST URL'si.</span><span class="sxs-lookup"><span data-stu-id="489f7-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="489f7-331">Görev 1 - HTTP GET oluşturma eylemi yöntemini uygulama</span><span class="sxs-lookup"><span data-stu-id="489f7-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="489f7-332">Bu görevde, tüm türleri ve sanatçıların listesini almak için bu verileri içine paketini oluşturma eylem yöntemi GET HTTP sürümü gerçekleştireceksiniz bir **StoreManagerViewModel** bir görünüm şablonu için geçirilecek nesne.</span><span class="sxs-lookup"><span data-stu-id="489f7-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="489f7-333">Açık **başlamak** çözüm bulunan **kaynak/Ex4-AddingACreateView/başlangıç/** klasör.</span><span class="sxs-lookup"><span data-stu-id="489f7-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="489f7-334">Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="489f7-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="489f7-335">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="489f7-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="489f7-336">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="489f7-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="489f7-337">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="489f7-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="489f7-338">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="489f7-339">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="489f7-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="489f7-340">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="489f7-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="489f7-341">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="489f7-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="489f7-342">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="489f7-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="489f7-343">Bunu yapmak için genişletme **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="489f7-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="489f7-344">Değiştirin **Oluştur** eylem yöntemi kodu şu kodla:</span><span class="sxs-lookup"><span data-stu-id="489f7-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="489f7-345">(Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex4 StoreManagerController HTTP GET Oluştur eylemi*)</span><span class="sxs-lookup"><span data-stu-id="489f7-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="489f7-346">Görev 2 - oluşturma görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="489f7-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="489f7-347">Bu görevde, yeni (boş) albüm form görüntüler Görünüm Oluştur şablona ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="489f7-348">İçinde sağ **Oluştur** eylem yöntemini seçip alt **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="489f7-349">Bu Görünüm Ekle iletişim kutusu getirir.</span><span class="sxs-lookup"><span data-stu-id="489f7-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="489f7-350">Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="489f7-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="489f7-351">Seçin **kesin türü belirtilmiş görünüm oluşturmak** seçeneğini işaretleyip **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır ve **Oluştur** gelen **İskele şablon** açılır.</span><span class="sxs-lookup"><span data-stu-id="489f7-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="489f7-352">Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="489f7-353">![Bir oluşturma görünümü ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "ekleme-a-Oluştur-view.png")</span><span class="sxs-lookup"><span data-stu-id="489f7-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="489f7-354">*Oluşturma görünümü ekleme*</span><span class="sxs-lookup"><span data-stu-id="489f7-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="489f7-355">Güncelleştirme **GenreId** ve **ArtistId** alanlar aşağıda gösterildiği gibi açılan listesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="489f7-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="489f7-356">Görev 3 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="489f7-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="489f7-357">Bu görevde, sınayacak **StoreManager** **Oluştur** görünüm sayfası boş bir albüm form görüntüler.</span><span class="sxs-lookup"><span data-stu-id="489f7-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="489f7-358">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="489f7-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="489f7-359">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="489f7-359">The project starts in the Home page.</span></span> <span data-ttu-id="489f7-360">URL'yi **/StoreManager/oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="489f7-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="489f7-361">Yeni albüm özelliklerini doldurmak için boş bir form görüntülendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="489f7-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="489f7-362">![Boş bir formla görünümü oluşturmak](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "boş bir formla Görünüm Oluştur")</span><span class="sxs-lookup"><span data-stu-id="489f7-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="489f7-363">*Boş bir formla görünümü oluşturma*</span><span class="sxs-lookup"><span data-stu-id="489f7-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="489f7-364">Görev 4 - HTTP-POST uygulama oluşturma eylem yöntemi</span><span class="sxs-lookup"><span data-stu-id="489f7-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="489f7-365">Bu görevde bir kullanıcı tıkladığında çağrılır oluşturma eylem yöntemine HTTP POST sürümünü gerçekleştireceksiniz **Kaydet** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="489f7-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="489f7-366">Yöntemi veritabanına yeni albümü kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="489f7-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="489f7-367">Gerekirse Visual Studio penceresine dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="489f7-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="489f7-368">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="489f7-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="489f7-369">Bunu yapmak için genişletme **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="489f7-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="489f7-370">Değiştirin **HTTP POST oluşturma** eylem yöntemi kodu şu kodla:</span><span class="sxs-lookup"><span data-stu-id="489f7-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="489f7-371">(Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex4 StoreManagerController HTTP - POST Oluştur eylemi*)</span><span class="sxs-lookup"><span data-stu-id="489f7-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="489f7-372">Oluşturma eylemini önceki düzenleme eylem yöntemine oldukça benzer, ancak nesne değiştirilmiş olarak ayarlamak yerine, bunu bağlamına ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="489f7-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="489f7-373">Görev 5 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="489f7-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="489f7-374">Bu görevde, sınayacak **StoreManager oluşturma** görünümü sayfasında, yeni albümü oluşturmanıza olanak sağlar ve ardından StoreManager dizin görünümüne yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="489f7-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="489f7-375">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="489f7-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="489f7-376">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="489f7-376">The project starts in the Home page.</span></span> <span data-ttu-id="489f7-377">URL'yi **/StoreManager/oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="489f7-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="489f7-378">Tüm form alanlarını, aşağıdaki şekilde benzeyen yeni bir albümü için verilerle doldurun:</span><span class="sxs-lookup"><span data-stu-id="489f7-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="489f7-379">![Albüm oluşturma](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "albüm oluşturma")</span><span class="sxs-lookup"><span data-stu-id="489f7-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="489f7-380">*Albüm oluşturma*</span><span class="sxs-lookup"><span data-stu-id="489f7-380">*Creating an Album*</span></span>
3. <span data-ttu-id="489f7-381">Yeni oluşturduğunuz yeni albümü içerir StoreManager dizini görünümü yeniden yönlendirilen doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="489f7-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="489f7-382">![Oluşturulan yeni albümü](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "oluşturulan yeni albümü")</span><span class="sxs-lookup"><span data-stu-id="489f7-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="489f7-383">*Oluşturulan yeni albümü*</span><span class="sxs-lookup"><span data-stu-id="489f7-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="489f7-384">Alıştırma 5: Silme işleme</span><span class="sxs-lookup"><span data-stu-id="489f7-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="489f7-385">Albümleri silme olanağı henüz uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="489f7-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="489f7-386">Bu alıştırmada hakkında olacaktır budur.</span><span class="sxs-lookup"><span data-stu-id="489f7-386">This is what this exercise will be about.</span></span> <span data-ttu-id="489f7-387">İçinde iki ayrı yöntemlerini kullanarak silme senaryosunu önce uygular gibi **StoreManagerController** sınıfı:</span><span class="sxs-lookup"><span data-stu-id="489f7-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="489f7-388">Bir eylem yöntemi bir doğrulama formu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="489f7-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="489f7-389">İkinci bir eylem yöntemi form gönderme işleyecek</span><span class="sxs-lookup"><span data-stu-id="489f7-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="489f7-390">Görev 1 - GET HTTP Delete eylem yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="489f7-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="489f7-391">Bu görevde, albüm bilgi almak için Delete eylem yöntemini HTTP GET sürümünü uygular.</span><span class="sxs-lookup"><span data-stu-id="489f7-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="489f7-392">Açık **başlamak** çözüm bulunan **kaynak/Ex5-HandlingDeletion/başlangıç/** klasör.</span><span class="sxs-lookup"><span data-stu-id="489f7-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="489f7-393">Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="489f7-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="489f7-394">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="489f7-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="489f7-395">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="489f7-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="489f7-396">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="489f7-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="489f7-397">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="489f7-398">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="489f7-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="489f7-399">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="489f7-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="489f7-400">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="489f7-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="489f7-401">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="489f7-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="489f7-402">Bunu yapmak için genişletme **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="489f7-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="489f7-403">Silme denetleyici eylemi tam olarak önceki Store ayrıntıları denetleyici eylemini aynıdır: Bu sorgular **albüm** kullanarak veritabanı nesne **kimliği** döndürür ve URL içinde sağlanan uygun **görünümü**.</span><span class="sxs-lookup"><span data-stu-id="489f7-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="489f7-404">Bunu yapmak için HTTP GET değiştirin **Sil** eylem yöntemi kodu şu kodla:</span><span class="sxs-lookup"><span data-stu-id="489f7-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="489f7-405">(Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex5 işleme silme HTTP GET silme eylemi*)</span><span class="sxs-lookup"><span data-stu-id="489f7-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="489f7-406">İçinde sağ **Sil** eylem yöntemini seçip alt **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="489f7-407">Bu Görünüm Ekle iletişim kutusu getirir.</span><span class="sxs-lookup"><span data-stu-id="489f7-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="489f7-408">Görünüm Ekle iletişim kutusunda, Görünüm adı olduğundan emin olun **Sil**.</span><span class="sxs-lookup"><span data-stu-id="489f7-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="489f7-409">Seçin **kesin türü belirtilmiş görünüm oluşturmak** seçeneğini işaretleyip **albüm (MvcMusicStore.Models)** gelen **Model sınıfı** açılır.</span><span class="sxs-lookup"><span data-stu-id="489f7-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="489f7-410">Seçin **Sil** gelen **İskele şablon** açılır.</span><span class="sxs-lookup"><span data-stu-id="489f7-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="489f7-411">Diğer alanları varsayılan değerlerine bırakın ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="489f7-412">![Delete görünüm ekleme](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "silme görünüm ekleme")</span><span class="sxs-lookup"><span data-stu-id="489f7-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="489f7-413">*Delete görünüm ekleme*</span><span class="sxs-lookup"><span data-stu-id="489f7-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="489f7-414">Tüm alanları modelden Sil şablon gösterir.</span><span class="sxs-lookup"><span data-stu-id="489f7-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="489f7-415">Yalnızca albüm başlık gösterilir.</span><span class="sxs-lookup"><span data-stu-id="489f7-415">You will show only the album's title.</span></span> <span data-ttu-id="489f7-416">Bunu yapmak için görünümün içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="489f7-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="489f7-417">Görev 2 - uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="489f7-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="489f7-418">Bu görevde, sınayacak **StoreManager** **Sil** görünüm sayfası onay silme form görüntüler.</span><span class="sxs-lookup"><span data-stu-id="489f7-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="489f7-419">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="489f7-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="489f7-420">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="489f7-420">The project starts in the Home page.</span></span> <span data-ttu-id="489f7-421">URL'yi **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="489f7-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="489f7-422">Tıklayarak silmek için bir albümü seçmek **Sil** ve yeni görüntüyü karşıya yüklendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="489f7-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="489f7-423">![Albüm silme](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "albüm siliniyor")</span><span class="sxs-lookup"><span data-stu-id="489f7-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="489f7-424">*Albüm siliniyor*</span><span class="sxs-lookup"><span data-stu-id="489f7-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="489f7-425">Görev 3-HTTP-POST silme eylem yöntemi uygulama</span><span class="sxs-lookup"><span data-stu-id="489f7-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="489f7-426">Bu görevde, kullanıcı tıkladığında çağrılır Delete eylem yöntemini HTTP POST sürümünü gerçekleştireceksiniz **Sil** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="489f7-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="489f7-427">Yöntemi, veritabanındaki albümü silmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="489f7-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="489f7-428">Gerekirse Visual Studio penceresine dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="489f7-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="489f7-429">Açık **StoreManagerController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="489f7-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="489f7-430">Bunu yapmak için genişletme **denetleyicileri** klasörü ve çift **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="489f7-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="489f7-431">Değiştirin **HTTP POST Sil** eylem yöntemi kodu şu kodla:</span><span class="sxs-lookup"><span data-stu-id="489f7-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="489f7-432">(Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex5 işleme silme HTTP POST silme eylemi*)</span><span class="sxs-lookup"><span data-stu-id="489f7-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="489f7-433">Görev 4 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="489f7-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="489f7-434">Bu görevde, sınayacak **StoreManager silme** görünüm sayfası albüm silmenize olanak sağlar ve ardından StoreManager dizin görünümüne yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="489f7-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="489f7-435">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="489f7-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="489f7-436">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="489f7-436">The project starts in the Home page.</span></span> <span data-ttu-id="489f7-437">URL'yi **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="489f7-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="489f7-438">Tıklayarak silmek için bir albümü seçmek **silin.**</span><span class="sxs-lookup"><span data-stu-id="489f7-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="489f7-439">Silme işlemini onaylamak **Sil** düğmesi:</span><span class="sxs-lookup"><span data-stu-id="489f7-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="489f7-440">![Albüm silme](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "albüm siliniyor")</span><span class="sxs-lookup"><span data-stu-id="489f7-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="489f7-441">*Albüm siliniyor*</span><span class="sxs-lookup"><span data-stu-id="489f7-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="489f7-442">İçinde görünmediğinden albümü silindiğini doğrulayın **dizin** sayfası.</span><span class="sxs-lookup"><span data-stu-id="489f7-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="489f7-443">Alıştırma 6: Doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="489f7-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="489f7-444">Şu anda prosedürleriniz var oluşturma ve düzenleme form doğrulama herhangi bir türden gerçekleştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="489f7-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="489f7-445">Fiyat alanında harflerini yazın veya gerekli alanın boş kullanıcı ayrılsa erişmenizi sağlayacak ilk hata veritabanından olacaktır.</span><span class="sxs-lookup"><span data-stu-id="489f7-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="489f7-446">Veri ek açıklamaları model sınıfınızın ekleyerek uygulamaya doğrulama ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="489f7-447">Veri ek açıklamaları model özelliklerinizi uygulanan istediğiniz kuralları açıklayan izin ve ASP.NET MVC zorlama ve kullanıcılara uygun iletisini gösteren ilgileniriz.</span><span class="sxs-lookup"><span data-stu-id="489f7-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="489f7-448">Görev 1 - veri ek açıklamaları ekleme</span><span class="sxs-lookup"><span data-stu-id="489f7-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="489f7-449">Bu görevde, uygun olduğunda doğrulama iletileri oluşturma ve düzenleme sayfası oluşturacak albüm modeline veri ek açıklamaları görüntülemek ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="489f7-450">Basit bir Model sınıfı için veri ek açıklama ekleme yalnızca ekleyerek gerçekleştirilir bir **kullanarak** bildirimi **System.ComponentModel.DataAnnotation**, sonra yerleştirerek bir **[gerekli]** uygun özellikleri özniteliği.</span><span class="sxs-lookup"><span data-stu-id="489f7-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="489f7-451">Aşağıdaki örnekte yapacağınız **adı** görünümünde gerekli bir alan özelliği.</span><span class="sxs-lookup"><span data-stu-id="489f7-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="489f7-452">Varlık veri modeli oluşturulduğu biraz daha karmaşık durumlarda bu uygulama gibi budur.</span><span class="sxs-lookup"><span data-stu-id="489f7-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="489f7-453">Veri ek açıklamaları model sınıflarına doğrudan eklediyseniz, veritabanı modelden güncelleştirirseniz bunlar üzerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="489f7-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="489f7-454">Bunun yerine, yapabileceğiniz hangi ek açıklamalar tutacak yer alır ve modelle ilişkili meta verileri kısmi sınıflarının sınıflarını kullanarak **[MetadataType]** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="489f7-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="489f7-455">Açık **başlamak** çözüm bulunan **kaynak/Ex6-AddingValidation/başlangıç/** klasör.</span><span class="sxs-lookup"><span data-stu-id="489f7-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="489f7-456">Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="489f7-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="489f7-457">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="489f7-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="489f7-458">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="489f7-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="489f7-459">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="489f7-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="489f7-460">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="489f7-461">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="489f7-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="489f7-462">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="489f7-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="489f7-463">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="489f7-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="489f7-464">Açık **Album.cs** gelen **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="489f7-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="489f7-465">Değiştirin **Album.cs** içerik ile vurgulanmış kodu, böylece aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="489f7-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="489f7-466">Satır **[DisplayFormat(ConvertEmptyStringToNull=false)]** veri alanı veri kaynağında güncelleştirildiğinde modelinden boş dizeler null değerine dönüştürülüp olmaz olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="489f7-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="489f7-467">Veri ek açıklama alanları doğrular önce Entity Framework modele null değerleri atarken bu ayar bir özel durum uğraşmasına gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="489f7-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="489f7-468">(Kod parçacığını - *ASP.NET MVC 4 Yardımcılar ve formlar ve doğrulama - Ex6 albüm meta verileri kısmi sınıf*)</span><span class="sxs-lookup"><span data-stu-id="489f7-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="489f7-469">Bu **albüm** kısmi sınıf sahip bir **MetadataType** işaret özniteliği **AlbumMetaData** için veri ek açıklama sınıfı.</span><span class="sxs-lookup"><span data-stu-id="489f7-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="489f7-470">Albüm model öğesine açıklama eklemek için kullanmakta olduğunuz veri ek açıklama öznitelikleri bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="489f7-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="489f7-471">Gereklidir - gerekli bir alan özelliği belirtir</span><span class="sxs-lookup"><span data-stu-id="489f7-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="489f7-472">DisplayName - form alanlarını ve doğrulama iletilerinin kullanılacak metni tanımlar</span><span class="sxs-lookup"><span data-stu-id="489f7-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="489f7-473">-DisplayFormat veri alanlarını nasıl görüntülenir ve biçimlendirilmiş belirtir.</span><span class="sxs-lookup"><span data-stu-id="489f7-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="489f7-474">StringLength - tanımlayan bir dize alanı için en fazla uzunluk</span><span class="sxs-lookup"><span data-stu-id="489f7-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="489f7-475">Range - sayısal bir alan için bir maksimum ve minimum değer verir</span><span class="sxs-lookup"><span data-stu-id="489f7-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="489f7-476">ScaffoldColumn - sağlayan alanları Düzenleyicisi formlarda gizleme</span><span class="sxs-lookup"><span data-stu-id="489f7-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="489f7-477">Görev 2 - uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="489f7-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="489f7-478">Bu görevde, oluşturma ve düzenleme sayfaları alanları doğrulamak son görevde seçilen görünen adlarını kullanarak test eder.</span><span class="sxs-lookup"><span data-stu-id="489f7-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="489f7-479">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="489f7-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="489f7-480">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="489f7-480">The project starts in the Home page.</span></span> <span data-ttu-id="489f7-481">URL'yi **/StoreManager/oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="489f7-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="489f7-482">Görünen adlarını kısmi sınıf dışındaki eşleştiğinden emin olun (gibi **albüm resim URL'si** yerine **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="489f7-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="489f7-483">Tıklayın **Oluştur**, form doldurma olmadan.</span><span class="sxs-lookup"><span data-stu-id="489f7-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="489f7-484">Karşılık gelen doğrulama iletilerini aldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="489f7-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="489f7-485">![Oluştur sayfasında alanlarını doğrulandı](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Oluştur sayfasında alanlarını doğrulandı")</span><span class="sxs-lookup"><span data-stu-id="489f7-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="489f7-486">*Doğrulanmış alanları oluşturma sayfası*</span><span class="sxs-lookup"><span data-stu-id="489f7-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="489f7-487">Aynı ile gerçekleşir doğrulayabilirsiniz **Düzenle** sayfası.</span><span class="sxs-lookup"><span data-stu-id="489f7-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="489f7-488">URL'yi **/StoreManager/Edit/1** ve görünen adlarını kısmi sınıf dışındaki eşleştiğinden emin olun (gibi **albüm resim URL'si** yerine **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="489f7-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="489f7-489">Boş **başlık** ve **fiyat** alanları ve tıklatın **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="489f7-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="489f7-490">Karşılık gelen doğrulama iletilerini aldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="489f7-490">Verify that you get the corresponding validation messages.</span></span>

    ![Doğrulanmış alanları düzenleme sayfası](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="489f7-492">*Doğrulanmış alanları düzenleme sayfası*</span><span class="sxs-lookup"><span data-stu-id="489f7-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="489f7-493">Alıştırma 7: İstemci tarafında örtük jQuery kullanma</span><span class="sxs-lookup"><span data-stu-id="489f7-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="489f7-494">Bu alıştırmada, istemci tarafındaki MVC 4 örtük jQuery doğrulamasını etkinleştirmek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="489f7-495">Örtük jQuery JavaScript veri ajax önek intrusively yayan satır içi istemci betiklerini yerine sunucu eylem yöntemlerini çağırmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="489f7-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="489f7-496">Görev 1 - etkinleştirme örtük jQuery önce uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="489f7-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="489f7-497">Bu görevde, iki doğrulama modeli karşılaştırmak için jQuery dahil olmak üzere önce uygulamayı çalışır.</span><span class="sxs-lookup"><span data-stu-id="489f7-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="489f7-498">Açık **başlamak** çözüm bulunan **kaynak/Ex7-UnobtrusivejQueryValidation/başlangıç/** klasör.</span><span class="sxs-lookup"><span data-stu-id="489f7-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="489f7-499">Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="489f7-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="489f7-500">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="489f7-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="489f7-501">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="489f7-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="489f7-502">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="489f7-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="489f7-503">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="489f7-504">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="489f7-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="489f7-505">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="489f7-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="489f7-506">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="489f7-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="489f7-507">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="489f7-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="489f7-508">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="489f7-508">The project starts in the Home page.</span></span> <span data-ttu-id="489f7-509">Gözat **/StoreManager/Create** tıklatıp **Oluştur** doğrulama iletilerinin aldığını doğrulamak için form doldurma olmadan:</span><span class="sxs-lookup"><span data-stu-id="489f7-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="489f7-510">![İstemci doğrulamasını devre dışı](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "istemci doğrulama devre dışı")</span><span class="sxs-lookup"><span data-stu-id="489f7-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="489f7-511">*İstemci doğrulamasını devre dışı*</span><span class="sxs-lookup"><span data-stu-id="489f7-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="489f7-512">HTML kaynak kodu tarayıcıda açın:</span><span class="sxs-lookup"><span data-stu-id="489f7-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="489f7-513">Görev 2 - örtük istemci etkinleştirme doğrulama</span><span class="sxs-lookup"><span data-stu-id="489f7-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="489f7-514">Bu görevde, jQuery olanak sağlayacak **örtük istemci doğrulama** gelen **Web.config** dosya, varsayılan olarak tüm yeni ASP.NET MVC 4 projeleri false değerini alır.</span><span class="sxs-lookup"><span data-stu-id="489f7-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="489f7-515">Ayrıca, gerekli komut dosyası başvuruları jQuery örtük istemci doğrulama iş yapma ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="489f7-516">Açık **Web.Config** dosya proje kök dizininde ve emin **ClientValidationEnabled** ve **UnobtrusiveJavaScriptEnabled** anahtarları değerleri içinayarlanmış**true**.</span><span class="sxs-lookup"><span data-stu-id="489f7-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="489f7-517">İstemci doğrulama kodu aynı sonuçları elde etmek için Global.asax.cs tarafından da etkinleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="489f7-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="489f7-518">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="489f7-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="489f7-519">Ayrıca, özel bir davranış sağlamak için tüm Denetleyicisi'nde ClientValidationEnabled özniteliği atayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="489f7-520">Açık **Create.cshtml** adresindeki **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="489f7-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="489f7-521">Aşağıdaki komut dosyalarını emin **jquery.validate** ve **jquery.validate.unobtrusive**, görüntüleme yoluyla içinde başvurulan &quot; **~/bundles/jqueryval** &quot; paket.</span><span class="sxs-lookup"><span data-stu-id="489f7-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="489f7-522">Bu jQuery kitaplıklar yeni MVC 4 projeleri dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="489f7-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="489f7-523">Daha fazla kitaplıklarında bulabilirsiniz **/komut dosyası** , proje klasörü.</span><span class="sxs-lookup"><span data-stu-id="489f7-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="489f7-524">Bu doğrulama yapmak için kitaplıklar çalışma, jQuery framework kitaplığına bir başvuru eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="489f7-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="489f7-525">Bu başvuru zaten eklendiğinden beri  **\_Layout.cshtml** dosyası gerektirmeyen belirli bu görünümde eklemek.</span><span class="sxs-lookup"><span data-stu-id="489f7-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="489f7-526">Görev 3 - uygulamayı kullanarak örtük jQuery doğrulaması çalışan</span><span class="sxs-lookup"><span data-stu-id="489f7-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="489f7-527">Bu görevde, sınayacak **StoreManager** şablon kullanıcı yeni albümü oluşturduğunda, jQuery kitaplıklarını kullanarak istemci tarafı doğrulama gerçekleştirir görünüm oluşturma.</span><span class="sxs-lookup"><span data-stu-id="489f7-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="489f7-528">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="489f7-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="489f7-529">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="489f7-529">The project starts in the Home page.</span></span> <span data-ttu-id="489f7-530">Gözat **/StoreManager/Create** tıklatıp **Oluştur** doğrulama iletilerinin aldığını doğrulamak için form doldurma olmadan:</span><span class="sxs-lookup"><span data-stu-id="489f7-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="489f7-531">![İstemci doğrulama etkin jQuery ile](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "etkin jQuery ile istemci doğrulaması")</span><span class="sxs-lookup"><span data-stu-id="489f7-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="489f7-532">*Etkin jQuery ile istemci doğrulaması*</span><span class="sxs-lookup"><span data-stu-id="489f7-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="489f7-533">Oluştur görünümünün için kaynak kodu tarayıcıda açın:</span><span class="sxs-lookup"><span data-stu-id="489f7-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="489f7-534">Her istemci doğrulama kuralı için örtük jQuery verilerle bir öznitelik ekler-val -*rulename*=&quot;*ileti*&quot;.</span><span class="sxs-lookup"><span data-stu-id="489f7-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="489f7-535">Etiketlerin listesini bu Unobtrusive aşağıdadır jQuery istemci doğrulama gerçekleştirmek için html giriş alanına ekler:</span><span class="sxs-lookup"><span data-stu-id="489f7-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="489f7-536">Veri val</span><span class="sxs-lookup"><span data-stu-id="489f7-536">Data-val</span></span>
   > - <span data-ttu-id="489f7-537">Veri val numarası</span><span class="sxs-lookup"><span data-stu-id="489f7-537">Data-val-number</span></span>
   > - <span data-ttu-id="489f7-538">Veri val aralığı</span><span class="sxs-lookup"><span data-stu-id="489f7-538">Data-val-range</span></span>
   > - <span data-ttu-id="489f7-539">Veri val aralığı dakika / val aralığı maksimum veri</span><span class="sxs-lookup"><span data-stu-id="489f7-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="489f7-540">Veri val gerekiyor</span><span class="sxs-lookup"><span data-stu-id="489f7-540">Data-val-required</span></span>
   > - <span data-ttu-id="489f7-541">Veri val uzunluğu</span><span class="sxs-lookup"><span data-stu-id="489f7-541">Data-val-length</span></span>
   > - <span data-ttu-id="489f7-542">Veri-val-uzunluk-max / veri val uzunluğu dakika</span><span class="sxs-lookup"><span data-stu-id="489f7-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="489f7-543">Tüm veri değerleri ile model doldurulur **veri ek açıklama**.</span><span class="sxs-lookup"><span data-stu-id="489f7-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="489f7-544">Ardından, sunucu tarafında çalışan tüm mantığı, istemci tarafında çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="489f7-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="489f7-545">Örneğin, fiyat özniteliği aşağıdaki veri ek açıklama modele sahiptir:</span><span class="sxs-lookup"><span data-stu-id="489f7-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="489f7-546">Örtük jQuery kullandıktan sonra oluşturulan kod şöyledir:</span><span class="sxs-lookup"><span data-stu-id="489f7-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="489f7-547">Özet</span><span class="sxs-lookup"><span data-stu-id="489f7-547">Summary</span></span>

<span data-ttu-id="489f7-548">Bu uygulamalı laboratuvarı tamamlayarak aşağıdaki kullanarak veritabanında depolanan verileri değiştirme olanağı tanıma öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="489f7-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="489f7-549">Dizin oluşturma, düzenleme, silme gibi denetleyici eylemleri</span><span class="sxs-lookup"><span data-stu-id="489f7-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="489f7-550">ASP.NET MVC'nin iskele kurma özelliği ile HTML tablosu halinde özelliklerini görüntülemek için</span><span class="sxs-lookup"><span data-stu-id="489f7-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="489f7-551">Kullanıcı geliştirmek için özel HTML Yardımcıları deneyimi</span><span class="sxs-lookup"><span data-stu-id="489f7-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="489f7-552">HTTP GET veya POST HTTP çağrıları için react eylem yöntemleri</span><span class="sxs-lookup"><span data-stu-id="489f7-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="489f7-553">Benzer bir görünüm şablonları oluşturma ve düzenleme gibi paylaşılan Düzenleyici şablonu</span><span class="sxs-lookup"><span data-stu-id="489f7-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="489f7-554">Açılan listeler gibi form öğeleri</span><span class="sxs-lookup"><span data-stu-id="489f7-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="489f7-555">Model doğrulama için veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="489f7-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="489f7-556">JQuery örtük kitaplığını kullanarak istemci tarafı doğrulama</span><span class="sxs-lookup"><span data-stu-id="489f7-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="489f7-557">Ek A: Visual Studio'yu Web için Express 2012</span><span class="sxs-lookup"><span data-stu-id="489f7-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="489f7-558">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="489f7-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="489f7-559">Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="489f7-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="489f7-560">Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="489f7-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="489f7-561">Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="489f7-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="489f7-562">Tıklayarak **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="489f7-562">Click on **Install Now**.</span></span> <span data-ttu-id="489f7-563">Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="489f7-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="489f7-564">Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="489f7-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="489f7-565">![Visual Studio Express yükleyin](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Visual Studio Express'i yükle")</span><span class="sxs-lookup"><span data-stu-id="489f7-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="489f7-566">*Visual Studio Express yükleyin*</span><span class="sxs-lookup"><span data-stu-id="489f7-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="489f7-567">Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="489f7-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşullarını kabul etme](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="489f7-569">*Lisans koşullarını kabul etme*</span><span class="sxs-lookup"><span data-stu-id="489f7-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="489f7-570">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="489f7-570">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="489f7-572">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="489f7-572">*Installation progress*</span></span>
6. <span data-ttu-id="489f7-573">Yükleme tamamlandığında, tıklayın **son**.</span><span class="sxs-lookup"><span data-stu-id="489f7-573">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="489f7-575">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="489f7-575">*Installation completed*</span></span>
7. <span data-ttu-id="489f7-576">Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="489f7-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="489f7-577">Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.</span><span class="sxs-lookup"><span data-stu-id="489f7-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Web kutucuğu için VS Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="489f7-579">*Web kutucuğu için VS Express*</span><span class="sxs-lookup"><span data-stu-id="489f7-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="489f7-580">Ek B: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="489f7-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="489f7-581">Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır.</span><span class="sxs-lookup"><span data-stu-id="489f7-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="489f7-582">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="489f7-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="489f7-583">![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="489f7-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="489f7-584">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="489f7-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="489f7-585">***Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="489f7-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="489f7-586">Kod eklemesini istediğiniz imleci yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="489f7-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="489f7-587">(Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="489f7-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="489f7-588">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="489f7-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="489f7-589">Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="489f7-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="489f7-590">İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="489f7-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="489f7-591">![Kod parçacığı adını yazmaya başlayın](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "kod parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="489f7-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="489f7-592">*Kod parçacığı adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="489f7-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="489f7-593">![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="489f7-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="489f7-594">*Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="489f7-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="489f7-595">![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")</span><span class="sxs-lookup"><span data-stu-id="489f7-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="489f7-596">*Yeniden Tab tuşuna basın ve kod parçacığı genişletir*</span><span class="sxs-lookup"><span data-stu-id="489f7-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="489f7-597">***Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1.</span><span class="sxs-lookup"><span data-stu-id="489f7-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="489f7-598">Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="489f7-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="489f7-599">Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.</span><span class="sxs-lookup"><span data-stu-id="489f7-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="489f7-600">Tıklayarak ilgili kod parçacığı listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="489f7-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="489f7-601">![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")</span><span class="sxs-lookup"><span data-stu-id="489f7-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="489f7-602">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*</span><span class="sxs-lookup"><span data-stu-id="489f7-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="489f7-603">![Tıklayarak ilgili kod parçacığını listesinden çekme](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "tıklayarak ilgili kod parçacığı listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="489f7-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="489f7-604">*Tıklayarak ilgili kod parçacığı listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="489f7-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
