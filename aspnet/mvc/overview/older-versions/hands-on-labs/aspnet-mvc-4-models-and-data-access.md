---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 modelleri ve veri erişimi | Microsoft Docs
author: rick-anderson
description: 'Not: Bu uygulamalı laboratuvarı ASP.NET MVC temel bilgiye sahip olduğunu varsayar. ASP.NET MVC 4 gitmenizi öneririz önce ASP.NET MVC kullanmadıysanız...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 26896e6ee3c02e8f939296ecbfb8b7d500940765
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753412"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="b5405-104">ASP.NET MVC 4 modelleri ve veri erişimi</span><span class="sxs-lookup"><span data-stu-id="b5405-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="b5405-105">Tarafından [Team Web Kampları](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b5405-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="b5405-106">Eğitim Seti Web Kampları indirin</span><span class="sxs-lookup"><span data-stu-id="b5405-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="b5405-107">Bu uygulamalı laboratuvarı temel bilgiye sahip olduğunuz varsayılır **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="b5405-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="b5405-108">Kullanmadıysanız, **ASP.NET MVC** gitmenizi öneririz önce **ASP.NET MVC 4 Temelleri** Hands-on-Lab.</span><span class="sxs-lookup"><span data-stu-id="b5405-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="b5405-109">Bu Laboratuvar, küçük değişiklikler kaynak klasördeki sağlanan örnek bir Web uygulamasına uygulayarak daha önce açıklanan yeni özellikler ve iyileştirmeler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b5405-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="b5405-110">Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [Microsoft-Web/WebCampTrainingKit yayınlar](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="b5405-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="b5405-111">Bu Laboratuvar için belirli proje kullanılabilir [ASP.NET MVC 4 modelleri ve verilere erişim](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="b5405-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="b5405-112">İçinde **ASP.NET MVC ile ilgili temel bilgiler** uygulamalı Laboratuvar, sabit kodlanmış veri geçirerek denetleyicilerinden için görüntüleme şablonları.</span><span class="sxs-lookup"><span data-stu-id="b5405-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="b5405-113">Ancak gerçek bir Web uygulaması oluşturmak için gerçek bir veritabanı kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b5405-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="b5405-114">Bu uygulamalı laboratuvarı müzik Store uygulaması için gereken verileri almak ve depolamak için bir veritabanı altyapısının nasıl kullanıldığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="b5405-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="b5405-115">Bunu yapmaya yönelik var olan bir veritabanına başlamak ve varlık veri Modeli'ni oluşturmak.</span><span class="sxs-lookup"><span data-stu-id="b5405-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="b5405-116">Bu Laboratuvar karşılayacak mı **veritabanı ilk** yaklaşım yanı sıra **Code First** yaklaşım.</span><span class="sxs-lookup"><span data-stu-id="b5405-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="b5405-117">Ancak, ayrıca kullanabileceğiniz **modeli ilk** yaklaşımı, aynı araçları kullanarak model oluşturma ve ondan sonra veritabanını oluşturmak.</span><span class="sxs-lookup"><span data-stu-id="b5405-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="b5405-118">![Veritabanı ilk vs. Model ilk](aspnet-mvc-4-models-and-data-access/_static/image1.png "veritabanı ilk vs. İlk model")</span><span class="sxs-lookup"><span data-stu-id="b5405-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="b5405-119">*Veritabanı ilk vs. İlk model*</span><span class="sxs-lookup"><span data-stu-id="b5405-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="b5405-120">Model oluşturduktan sonra StoreController Store görünümleri sabit kodlanmış verilerin kullanmak yerine veritabanından alınan veri sağlamak için de uygun ayarlamalar yapar.</span><span class="sxs-lookup"><span data-stu-id="b5405-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="b5405-121">Görünüm şablonları için aynı Viewmodel'lar StoreController döndüren çünkü bu kez, veritabanından veri gelir ancak herhangi bir değişiklik görünümü şablonlarda yapmak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="b5405-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="b5405-122">**İlk kod yaklaşımı**</span><span class="sxs-lookup"><span data-stu-id="b5405-122">**The Code First Approach**</span></span>

<span data-ttu-id="b5405-123">İlk kod yaklaşımı genellikle framework ile bağlı sınıfları oluşturmadan kod modeli tanımlamak sağlıyor.</span><span class="sxs-lookup"><span data-stu-id="b5405-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="b5405-124">Kodda, ilk olarak, model nesneleri POCOs ile tanımlanan &quot;düz eski CLR nesnelerini&quot;.</span><span class="sxs-lookup"><span data-stu-id="b5405-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="b5405-125">POCOs hiçbir devralma ve arabirimler uygulayamaz basit düz sınıflardır.</span><span class="sxs-lookup"><span data-stu-id="b5405-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="b5405-126">Biz otomatik olarak veritabanı bunları oluşturabilir veya biz varolan veritabanını kullan ve koddan sınıf eşleme oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b5405-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="b5405-127">Bu yaklaşımı kullanmanın avantajları olduğundan POCOs sınıfları eşleme framework ile bağlı değil olarak Model (Bu örnekte, Entity Framework), Kalıcılık çerçeveden bağımsız olarak kalır.</span><span class="sxs-lookup"><span data-stu-id="b5405-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="b5405-128">ASP.NET MVC 4'te bu Laboratuvar temel alır ve müzik Store örnek uygulamanın bir sürümü yalnızca bu uygulamalı laboratuvarda gösterilen özellikler sığdırmak için simge durumuna küçültülmüş ve özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="b5405-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="b5405-129">Tüm keşfetmek istiyorsanız **müzik Store** öğretici uygulama içinde bulabilirsiniz [MVC müzik Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="b5405-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b5405-130">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b5405-130">Prerequisites</span></span>

<span data-ttu-id="b5405-131">Bu laboratuvarı tamamlamak için aşağıdakiler olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="b5405-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="b5405-132">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya üst (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="b5405-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b5405-133">Kurulum</span><span class="sxs-lookup"><span data-stu-id="b5405-133">Setup</span></span>

<span data-ttu-id="b5405-134">**Kod parçacıkları yükleniyor**</span><span class="sxs-lookup"><span data-stu-id="b5405-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="b5405-135">Kolaylık olması için bu Laboratuvar yöneteceğiniz kodun çoğu Visual Studio kod parçacıkları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b5405-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b5405-136">Kod parçacıklarını çalıştırmak yüklenecek **.\Source\Setup\CodeSnippets.vsi** dosya.</span><span class="sxs-lookup"><span data-stu-id="b5405-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b5405-137">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istediğiniz konusunda bilgi sahibi değilseniz, bu belge, ek başvurabilir &quot; [ek C: kullanarak kod parçacıklarına](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="b5405-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b5405-138">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="b5405-138">Exercises</span></span>

<span data-ttu-id="b5405-139">Bu uygulamalı Laboratuvar tarafından aşağıdaki çalışmaları oluşur:</span><span class="sxs-lookup"><span data-stu-id="b5405-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="b5405-140">Alıştırma 1: bir veritabanı ekleme</span><span class="sxs-lookup"><span data-stu-id="b5405-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="b5405-141">Alıştırma 2: Code First kullanarak veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="b5405-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="b5405-142">Alıştırma 3: parametrelerle veritabanını sorgulama</span><span class="sxs-lookup"><span data-stu-id="b5405-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="b5405-143">Her bir alıştırma olarak sunulduğu bir **son** elde alıştırmalar tamamladıktan sonra ortaya çıkan çözüm içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="b5405-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="b5405-144">Çalışma alıştırmaları ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b5405-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="b5405-145">Bu laboratuvarı tamamlamak için tahmini süre: **35 dakika**.</span><span class="sxs-lookup"><span data-stu-id="b5405-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="b5405-146">Alıştırma 1: bir veritabanı ekleme</span><span class="sxs-lookup"><span data-stu-id="b5405-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="b5405-147">Bu alıştırmada, bir veritabanı tabloları verilerini kullanmak MusicStore uygulamanın çözüme ekleyin öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="b5405-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="b5405-148">Veritabanı modeli ile oluşturulan ve çözüme sonra Görünüm şablonu sabit kodlanmış değerler yerine veritabanından alınan veri sağlamak için StoreController sınıfı değiştirir.</span><span class="sxs-lookup"><span data-stu-id="b5405-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="b5405-149">Görev 1 - veritabanı ekleme</span><span class="sxs-lookup"><span data-stu-id="b5405-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="b5405-150">Bu görevde, önceden oluşturulmuş bir veritabanı çözümü MusicStore uygulamanın ana tablolarla ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="b5405-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="b5405-151">Açık **başlamak** çözüm bulunan **kaynak/Ex1-AddingADatabaseDBFirst/başlangıç/** klasör.</span><span class="sxs-lookup"><span data-stu-id="b5405-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="b5405-152">Bazı eksik NuGet paketleri indirmeniz gerekecek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="b5405-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b5405-153">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="b5405-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b5405-154">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="b5405-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b5405-155">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="b5405-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b5405-156">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="b5405-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b5405-157">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="b5405-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b5405-158">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="b5405-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b5405-159">Ekleme **MvcMusicStore** veritabanı dosyası.</span><span class="sxs-lookup"><span data-stu-id="b5405-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="b5405-160">Bu uygulamalı laboratuvarda adlı önceden oluşturulmuş bir veritabanı kullanacağınız **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="b5405-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="b5405-161">Bunu yapmak için sağ **uygulama\_veri** klasörünü **Ekle** ve ardından **var olan öğe**.</span><span class="sxs-lookup"><span data-stu-id="b5405-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="b5405-162">Gözat **\Source\Assets** seçip **MvcMusicStore.mdf** dosya.</span><span class="sxs-lookup"><span data-stu-id="b5405-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="b5405-163">![Var olan bir öğe eklemeye](aspnet-mvc-4-models-and-data-access/_static/image2.png "var olan öğe ekleme")</span><span class="sxs-lookup"><span data-stu-id="b5405-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="b5405-164">*Var olan öğe ekleme*</span><span class="sxs-lookup"><span data-stu-id="b5405-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="b5405-165">![Veritabanı dosyası MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf veritabanı dosyası")</span><span class="sxs-lookup"><span data-stu-id="b5405-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="b5405-166">*MvcMusicStore.mdf veritabanı dosyası*</span><span class="sxs-lookup"><span data-stu-id="b5405-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="b5405-167">Veritabanı projeye eklendi.</span><span class="sxs-lookup"><span data-stu-id="b5405-167">The database has been added to the project.</span></span> <span data-ttu-id="b5405-168">Veritabanı içinde çözüm bile bulunur, sorgu ve farklı bir veritabanı sunucusu barındırılan olarak güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="b5405-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="b5405-169">![Çözüm Gezgini'nde MvcMusicStore veritabanı](aspnet-mvc-4-models-and-data-access/_static/image4.png "Çözüm Gezgini'nde MvcMusicStore veritabanı")</span><span class="sxs-lookup"><span data-stu-id="b5405-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="b5405-170">*Çözüm Gezgini'nde MvcMusicStore veritabanı*</span><span class="sxs-lookup"><span data-stu-id="b5405-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="b5405-171">Veritabanı bağlantısını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="b5405-171">Verify the connection to the database.</span></span> <span data-ttu-id="b5405-172">Bunu yapmak için çift **MvcMusicStore.mdf** ile bağlantı kurar.</span><span class="sxs-lookup"><span data-stu-id="b5405-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="b5405-173">![Bağlanmak için MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "için MvcMusicStore.mdf bağlanma")</span><span class="sxs-lookup"><span data-stu-id="b5405-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="b5405-174">*İçin MvcMusicStore.mdf bağlanma*</span><span class="sxs-lookup"><span data-stu-id="b5405-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="b5405-175">Görev 2 - bir veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="b5405-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="b5405-176">Bu görevde, önceki görevde eklenen veritabanıyla etkileşim kurmak için bir veri modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b5405-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="b5405-177">Veritabanı temsil edecek bir veri modeli oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b5405-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="b5405-178">Çözüm Gezgini sağ tıklatın Bunu yapmak için **modelleri** klasörünü **Ekle** ve ardından **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="b5405-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="b5405-179">İçinde **Yeni Öğe Ekle** iletişim kutusunda **veri** şablonu ve ardından **ADO.NET varlık veri modeli** öğesi.</span><span class="sxs-lookup"><span data-stu-id="b5405-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="b5405-180">Veri modeli adı için değiştirme **StoreDB.edmx** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="b5405-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="b5405-181">![StoreDB ADO.NET varlık veri modeli ekleme](aspnet-mvc-4-models-and-data-access/_static/image6.png "StoreDB ADO.NET varlık veri modeli ekleme")</span><span class="sxs-lookup"><span data-stu-id="b5405-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="b5405-182">*StoreDB ADO.NET varlık veri modeli ekleme*</span><span class="sxs-lookup"><span data-stu-id="b5405-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="b5405-183">**Varlık veri modeli Sihirbazı** görünür.</span><span class="sxs-lookup"><span data-stu-id="b5405-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="b5405-184">Bu sihirbaz modeli katmanı oluşturulmasını adım yönlendirecektir.</span><span class="sxs-lookup"><span data-stu-id="b5405-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="b5405-185">Model eklenen var olan veritabanı recentyl tabanlı oluşturulmalıdır. bu yana seçin **veritabanından Oluştur** tıklatıp **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="b5405-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="b5405-186">![Model içeriğinin seçme](aspnet-mvc-4-models-and-data-access/_static/image7.png "model içeriğinin seçme")</span><span class="sxs-lookup"><span data-stu-id="b5405-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="b5405-187">*Model içeriğinin seçme*</span><span class="sxs-lookup"><span data-stu-id="b5405-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="b5405-188">Bir veritabanından model oluşturma olduğundan, kullanılacak bağlantı belirtmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="b5405-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="b5405-189">Tıklayın **yeni bağlantı**.</span><span class="sxs-lookup"><span data-stu-id="b5405-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="b5405-190">Seçin **Microsoft SQL Server veritabanı dosyası** tıklatıp **devam**.</span><span class="sxs-lookup"><span data-stu-id="b5405-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="b5405-191">![Veri Kaynağı Seç](aspnet-mvc-4-models-and-data-access/_static/image8.png "veri kaynağı Seç")</span><span class="sxs-lookup"><span data-stu-id="b5405-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="b5405-192">*Veri kaynağı iletişim seçin*</span><span class="sxs-lookup"><span data-stu-id="b5405-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="b5405-193">Tıklayın **Gözat** veritabanını seçin **MvcMusicStore.mdf** bulunuyorsunuz **uygulama\_veri** klasörü ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="b5405-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="b5405-194">![Bağlantı özellikleri](aspnet-mvc-4-models-and-data-access/_static/image9.png "bağlantı özellikleri")</span><span class="sxs-lookup"><span data-stu-id="b5405-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="b5405-195">*Bağlantı özellikleri*</span><span class="sxs-lookup"><span data-stu-id="b5405-195">*Connection properties*</span></span>
6. <span data-ttu-id="b5405-196">Oluşturulan sınıfın varlık bağlantı dizesini aynı ada sahip, bu nedenle adını değiştirmek **MusicStoreEntities** tıklatıp **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="b5405-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="b5405-197">![Veri bağlantısı seçerek](aspnet-mvc-4-models-and-data-access/_static/image10.png "veri bağlantısını seçme")</span><span class="sxs-lookup"><span data-stu-id="b5405-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="b5405-198">*Veri bağlantısını seçme*</span><span class="sxs-lookup"><span data-stu-id="b5405-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="b5405-199">Veritabanı nesneleri seçin.</span><span class="sxs-lookup"><span data-stu-id="b5405-199">Choose the database objects to use.</span></span> <span data-ttu-id="b5405-200">Varlık modeli yalnızca veritabanı tablolarını kullanacak şekilde seçin **tabloları** seçenek ve emin **modelde yabancı anahtar sütunlarını içermesi** ve **Pluralize veya singularize oluşturulan Nesne adları** seçenekleri da seçilir.</span><span class="sxs-lookup"><span data-stu-id="b5405-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="b5405-201">Değiştirmek için Model Namespace **MvcMusicStore.Model** tıklatıp **son**.</span><span class="sxs-lookup"><span data-stu-id="b5405-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="b5405-202">![Veritabanı nesneleri seçme](aspnet-mvc-4-models-and-data-access/_static/image11.png "veritabanı nesneleri seçme")</span><span class="sxs-lookup"><span data-stu-id="b5405-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="b5405-203">*Veritabanı nesneleri seçme*</span><span class="sxs-lookup"><span data-stu-id="b5405-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5405-204">Bir güvenlik uyarısı iletişim kutusu gösteriliyorsa, tıklayın **Tamam** şablonu çalıştırın ve model varlıklar için sınıflar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b5405-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="b5405-205">Veritabanı için bir varlık diyagramda görünür, bu sırada her tablo veritabanına eşlenen ayrı bir sınıf oluşturulacak.</span><span class="sxs-lookup"><span data-stu-id="b5405-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="b5405-206">Örneğin, **albümleri** tabloda belirtildiği bir **albüm** sınıfı burada tablodaki her bir sütunun eşlemek için bir sınıf özelliği.</span><span class="sxs-lookup"><span data-stu-id="b5405-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="b5405-207">Bu, sorgulama ve veritabanındaki satırları temsil eden nesneleri ile çalışmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="b5405-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="b5405-208">![Varlık diyagramda](aspnet-mvc-4-models-and-data-access/_static/image12.png "varlık diyagramı")</span><span class="sxs-lookup"><span data-stu-id="b5405-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="b5405-209">*Varlık diyagramı*</span><span class="sxs-lookup"><span data-stu-id="b5405-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5405-210">T4 şablonları (.tt), varlık sınıfları üretmek için kod çalıştırın ve mevcut sınıfları aynı ada sahip üzerine yazar.</span><span class="sxs-lookup"><span data-stu-id="b5405-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="b5405-211">Bu örnekte, sınıfları &quot;albüm&quot;, &quot;Tarz&quot; ve &quot;sanatçının&quot; ile oluşturulan kodun üzerine yazıldı.</span><span class="sxs-lookup"><span data-stu-id="b5405-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="b5405-212">Görev 3 - uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="b5405-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="b5405-213">Model oluşturma kaldırıldı ancak bu görevde, kontrol **albüm**, **Tarz** ve **sanatçının** model sınıfları, projeye başarıyla kullanarak oluşturur Yeni veri modeli sınıfları.</span><span class="sxs-lookup"><span data-stu-id="b5405-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="b5405-214">Seçerek projenizi **derleme** menü öğesini ve ardından **MvcMusicStore yapı**.</span><span class="sxs-lookup"><span data-stu-id="b5405-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="b5405-215">![Proje derleme](aspnet-mvc-4-models-and-data-access/_static/image13.png "projesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="b5405-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="b5405-216">*Proje oluşturma*</span><span class="sxs-lookup"><span data-stu-id="b5405-216">*Building the project*</span></span>
2. <span data-ttu-id="b5405-217">Projeyi başarıyla derler.</span><span class="sxs-lookup"><span data-stu-id="b5405-217">The project builds successfully.</span></span> <span data-ttu-id="b5405-218">Neden hala çalışır?</span><span class="sxs-lookup"><span data-stu-id="b5405-218">Why does it still work?</span></span> <span data-ttu-id="b5405-219">Veritabanı tablolarını kaldırılan sınıflarda, kullandığınız özellikleri içeren alanlar olduğundan çalıştığını **albüm** ve **Tarz**.</span><span class="sxs-lookup"><span data-stu-id="b5405-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="b5405-220">![Derlemeler başarılı](aspnet-mvc-4-models-and-data-access/_static/image14.png "derlemeler başarılı oldu")</span><span class="sxs-lookup"><span data-stu-id="b5405-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="b5405-221">*Derlemeler başarılı oldu*</span><span class="sxs-lookup"><span data-stu-id="b5405-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="b5405-222">Tasarımcı varlıklar diyagramı biçiminde görüntülerken, bunlar gerçekten C# sınıfları.</span><span class="sxs-lookup"><span data-stu-id="b5405-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="b5405-223">Genişletin **StoreDB.edmx** Çözüm Gezgini'nde düğümünü ve ardından **StoreDB.tt**, yeni oluşturulan varlıkları görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="b5405-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="b5405-224">![Oluşturulan dosyaları](aspnet-mvc-4-models-and-data-access/_static/image15.png "oluşturulan dosyaları")</span><span class="sxs-lookup"><span data-stu-id="b5405-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="b5405-225">*Oluşturulan dosyalar*</span><span class="sxs-lookup"><span data-stu-id="b5405-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="b5405-226">Görev 4 - veritabanı sorgulama</span><span class="sxs-lookup"><span data-stu-id="b5405-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="b5405-227">Bu görevde, sabit kodlanmış veri kullanmak yerine, bu bilgileri almak için bu veritabanını sorgulamanızı böylece StoreController sınıfı güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="b5405-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="b5405-228">Açık **Controllers\StoreController.cs** ve şu alan örneği için Sınıf Ekle **MusicStoreEntities** adlı bir sınıf **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="b5405-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="b5405-229">(Kod parçacığını - *modeller ve veri erişimi - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="b5405-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="b5405-230">**MusicStoreEntities** sınıfı veritabanındaki her tablo için bir koleksiyon özelliği sunar.</span><span class="sxs-lookup"><span data-stu-id="b5405-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="b5405-231">Güncelleştirme **Gözat** tüm bir türe almak için eylem yöntemini **albümleri**.</span><span class="sxs-lookup"><span data-stu-id="b5405-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="b5405-232">(Kod parçacığını - *modeller ve veri erişimi - Ex1 Store Gözat*)</span><span class="sxs-lookup"><span data-stu-id="b5405-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="b5405-233">Bir özellik adında .NET, kullanmakta olduğunuz **LINQ** (kod veritabanında yürütün ve dönüş bu koleksiyonlara karşı-kesin türü belirtilmiş sorgu ifadeleri yazmak için dil ile tümleşik sorgu) nesneleri, programlama yapabilirsiniz karşı.</span><span class="sxs-lookup"><span data-stu-id="b5405-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="b5405-234">LINQ hakkında daha fazla bilgi için lütfen [msdn sitesine](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="b5405-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="b5405-235">Güncelleştirme **dizin** tüm türleri almak için eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b5405-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="b5405-236">(Kod parçacığını - *modeller ve veri erişimi - Ex1 Store dizini*)</span><span class="sxs-lookup"><span data-stu-id="b5405-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="b5405-237">Güncelleştirme **dizin** tüm türleri almak ve liste koleksiyon dönüştürmek için eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b5405-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="b5405-238">(Kod parçacığını - *modeller ve veri erişimi - Ex1 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="b5405-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="b5405-239">Görev 5 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b5405-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="b5405-240">Bu görevde, Store dizin sayfası yerine ' % s'belirtebilseydik olanları veritabanında depolanan türleri artık göstereceğini kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="b5405-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="b5405-241">Gerekmez, çünkü görünümü şablonu değiştirmek için **StoreController** şu veritabanından veri gelir ancak aynı varlıkları önceki gibi döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="b5405-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="b5405-242">Tuşuna basın ve çözümü yeniden **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b5405-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b5405-243">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="b5405-243">The project starts in the Home page.</span></span> <span data-ttu-id="b5405-244">Doğrulayın menüsünü **türleri** artık sabit kodlanmış bir listedir ve verileri doğrudan veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="b5405-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="b5405-246">*Veritabanından gözatma türleri*</span><span class="sxs-lookup"><span data-stu-id="b5405-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="b5405-247">Artık tüm Tarz için göz atın ve albümleri veritabanından doldurulur doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="b5405-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="b5405-248">![Veritabanından albümleri gözatma](aspnet-mvc-4-models-and-data-access/_static/image17.png "gözatma albümleri veritabanından")</span><span class="sxs-lookup"><span data-stu-id="b5405-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="b5405-249">*Veritabanından albümleri gözatma*</span><span class="sxs-lookup"><span data-stu-id="b5405-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="b5405-250">Alıştırma 2: Kod kullanarak veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="b5405-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="b5405-251">Bu alıştırmada MusicStore uygulama tablolarla bir veritabanı oluşturmak için ilk kod yaklaşımı kullanmayı ve kendi verilerine erişim öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="b5405-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="b5405-252">Model oluşturulduktan sonra Görünüm şablonu sabit kodlanmış değerler yerine veritabanından alınan veri sağlamak için StoreController değiştirir.</span><span class="sxs-lookup"><span data-stu-id="b5405-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="b5405-253">Alıştırma 1 tamamladıktan ve zaten veritabanı ilk yaklaşımı hakkında deneyimli olduğunuzu, artık başka bir işlem ile aynı sonuçları elde etmek nasıl öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="b5405-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="b5405-254">Alıştırma 1 ortak olan görevleri, okumayı kolaylaştırmak için işaretlenmiş.</span><span class="sxs-lookup"><span data-stu-id="b5405-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="b5405-255">Alıştırma 1 tamamlanmamış ancak ilk kod yaklaşımı öğrenmek istiyorsanız, bu çalışma başlayın ve konunun tam bir kapsamı edinin.</span><span class="sxs-lookup"><span data-stu-id="b5405-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="b5405-256">Görev 1 - doldurulurken örnek veriler</span><span class="sxs-lookup"><span data-stu-id="b5405-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="b5405-257">Code-First kullanarak başlangışta oluşturulduğunda bu görevde, örnek verilerle veritabanı doldurur.</span><span class="sxs-lookup"><span data-stu-id="b5405-257">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="b5405-258">Açık **başlamak** çözüm bulunan **kaynak/Ex2-CreatingADatabaseCodeFirst/başlangıç/** klasör.</span><span class="sxs-lookup"><span data-stu-id="b5405-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="b5405-259">Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="b5405-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b5405-260">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="b5405-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b5405-261">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="b5405-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b5405-262">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="b5405-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b5405-263">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="b5405-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b5405-264">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="b5405-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b5405-265">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="b5405-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b5405-266">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="b5405-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b5405-267">Ekleme **SampleData.cs** dosyasını **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="b5405-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="b5405-268">Bunu yapmak için sağ **modelleri** klasörünü **Ekle** ve ardından **var olan öğe**.</span><span class="sxs-lookup"><span data-stu-id="b5405-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="b5405-269">Gözat **\Source\Assets** seçip **SampleData.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="b5405-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="b5405-270">![Örnek veri kodu doldurmak](aspnet-mvc-4-models-and-data-access/_static/image18.png "örnek veri kodu Doldur")</span><span class="sxs-lookup"><span data-stu-id="b5405-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="b5405-271">*Örnek veri kodu Doldur*</span><span class="sxs-lookup"><span data-stu-id="b5405-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="b5405-272">Açık **Global.asax.cs** dosyasını açıp aşağıdaki *kullanarak* deyimleri.</span><span class="sxs-lookup"><span data-stu-id="b5405-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="b5405-273">(Kod parçacığını - *modeller ve veri erişimi - Ex2 genel Asax kullanımları*)</span><span class="sxs-lookup"><span data-stu-id="b5405-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="b5405-274">İçinde **uygulama\_Start()** yöntemi veritabanı Başlatıcı ayarlamak için aşağıdaki satırı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b5405-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="b5405-275">(Kod parçacığını - *modeller ve veri erişimi - Ex2 genel Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="b5405-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="b5405-276">Görev 2 - veritabanı bağlantısı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b5405-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="b5405-277">Projemizin için bir veritabanı zaten eklenmiş, yazacağınız **Web.config** bağlantı dizesini dosya.</span><span class="sxs-lookup"><span data-stu-id="b5405-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="b5405-278">Konumunda bir bağlantı dizesi Ekle **Web.config**. Bunu yapmak için açık **Web.config** proje kök ve bağlantı dizesi ile bu satırda DefaultConnection adlı Değiştir **&lt;connectionStrings&gt;** bölümü:</span><span class="sxs-lookup"><span data-stu-id="b5405-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="b5405-279">![Web.config dosyası konumu](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config dosyası konumu")</span><span class="sxs-lookup"><span data-stu-id="b5405-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="b5405-280">*Web.config dosyası konumu*</span><span class="sxs-lookup"><span data-stu-id="b5405-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="b5405-281">Görev 3 - modeli ile çalışma</span><span class="sxs-lookup"><span data-stu-id="b5405-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="b5405-282">Veritabanı bağlantısı zaten yapılandırdıysanız, veritabanı tablolarını modeliyle bağlayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="b5405-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="b5405-283">Bu görevde, Code First ile veritabanına bağlı bir sınıf oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b5405-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="b5405-284">Değiştirilmesi gereken gereği, mevcut bir POCO model sınıfı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="b5405-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="b5405-285">Alıştırma 1 tamamlandı, bu adım, bir sihirbaz tarafından gerçekleştirildiğini fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="b5405-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="b5405-286">Code First yaparak, el ile veri varlıkları için bağlantılı sınıfları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b5405-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="b5405-287">POCO model sınıfı açın **Tarz** gelen **modelleri** proje klasörü ve bir kimlik içerir</span><span class="sxs-lookup"><span data-stu-id="b5405-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="b5405-288">Adıyla bir int özelliğini kullanın **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="b5405-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="b5405-289">(Kod parçacığını - *modeller ve veri erişimi - Ex2 kod ilk Tarz*)</span><span class="sxs-lookup"><span data-stu-id="b5405-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="b5405-290">Kod öncelikli kurallar ile çalışmak için ' % s'sınıfı Tarz otomatik olarak algılanacak bir birincil anahtar özelliği olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b5405-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="b5405-291">Daha fazla bilgi bu kod öncelikli kurallar hakkında [msdn makalesi](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="b5405-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="b5405-292">Şimdi, POCO model sınıfı açmak **albüm** gelen **modelleri** proje klasörü ve yabancı anahtarlar eklemek, adlara sahip özellikler oluşturun **GenreId** ve  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="b5405-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="b5405-293">Bu sınıf zaten **GenreId** birincil anahtar.</span><span class="sxs-lookup"><span data-stu-id="b5405-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="b5405-294">(Kod parçacığını - *modeller ve veri erişimi - Ex2 kod ilk albüm*)</span><span class="sxs-lookup"><span data-stu-id="b5405-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="b5405-295">POCO model sınıfı açın **sanatçının** ve **ArtistId** özelliği.</span><span class="sxs-lookup"><span data-stu-id="b5405-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="b5405-296">(Kod parçacığını - *modeller ve veri erişimi - Ex2 kod ilk sanatçının*)</span><span class="sxs-lookup"><span data-stu-id="b5405-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="b5405-297">Sağ **modelleri** proje klasörü ve select **Ekle | Sınıf**.</span><span class="sxs-lookup"><span data-stu-id="b5405-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="b5405-298">Dosya adı **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="b5405-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="b5405-299">' A tıklayarak **Ekle.**</span><span class="sxs-lookup"><span data-stu-id="b5405-299">Then, click **Add.**</span></span>

    <span data-ttu-id="b5405-300">![Sınıf ekleme](aspnet-mvc-4-models-and-data-access/_static/image20.png "sınıf ekleme")</span><span class="sxs-lookup"><span data-stu-id="b5405-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="b5405-301">*Yeni bir öğe ekleme*</span><span class="sxs-lookup"><span data-stu-id="b5405-301">*Adding a new item*</span></span>

    <span data-ttu-id="b5405-302">![Bir class2 ekleme](aspnet-mvc-4-models-and-data-access/_static/image21.png "bir class2 ekleme")</span><span class="sxs-lookup"><span data-stu-id="b5405-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="b5405-303">*Sınıf ekleme*</span><span class="sxs-lookup"><span data-stu-id="b5405-303">*Adding a class*</span></span>
5. <span data-ttu-id="b5405-304">Az önce oluşturduğunuz, sınıfın açık **MusicStoreEntities.cs**ve ad alanlarını dahil **System.Data.Entity** ve **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="b5405-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="b5405-305">Genişletmek için sınıf bildirimini değiştirin **DbContext** sınıfı: Genel bildirmek **olan DB** ve geçersiz kılma **OnModelCreating** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b5405-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="b5405-306">Bu adımdan sonra modelinizi Entity Framework ile bağlayacak bir etki alanı sınıfı alırsınız.</span><span class="sxs-lookup"><span data-stu-id="b5405-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="b5405-307">Bunu yapabilmek için sınıf kodunu aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="b5405-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="b5405-308">(Kod parçacığını - *modeller ve veri erişimi - Ex2 kod ilk MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="b5405-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="b5405-309">Entity Framework ile **DbContext** ve **olan DB** POCO sınıfı Tarz sorgu mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="b5405-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="b5405-310">Genişleterek **OnModelCreating** yöntemi belirttiğinizden içinde **kod** tarzı bir veritabanı tablosuna nasıl eşlenir.</span><span class="sxs-lookup"><span data-stu-id="b5405-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="b5405-311">Bu msdn makalesinde DBContext olan DB hakkında daha fazla bilgi bulabilirsiniz: [bağlantı](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="b5405-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="b5405-312">Görev 4 - veritabanı sorgulama</span><span class="sxs-lookup"><span data-stu-id="b5405-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="b5405-313">Bu görevde, sabit kodlanmış veri kullanmak yerine, bu veritabanından alır, böylece StoreController sınıfı güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="b5405-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="b5405-314">Alıştırma 1 ortak görevdir.</span><span class="sxs-lookup"><span data-stu-id="b5405-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="b5405-315">Alıştırma 1 tamamladıysanız Bu adımlar, her iki yaklaşım aynı fark edeceksiniz (ilk veritabanı veya ilk kod).</span><span class="sxs-lookup"><span data-stu-id="b5405-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="b5405-316">Bunlar verileri nasıl modeliyle bağlı olarak farklıdır, ancak veri varlıklarına erişim henüz denetleyicisinden saydamdır.</span><span class="sxs-lookup"><span data-stu-id="b5405-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="b5405-317">Açık **Controllers\StoreController.cs** ve şu alan örneği için Sınıf Ekle **MusicStoreEntities** adlı bir sınıf **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="b5405-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="b5405-318">(Kod parçacığını - *modeller ve veri erişimi - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="b5405-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="b5405-319">**MusicStoreEntities** sınıfı veritabanındaki her tablo için bir koleksiyon özelliği sunar.</span><span class="sxs-lookup"><span data-stu-id="b5405-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="b5405-320">Güncelleştirme **Gözat** tüm bir türe almak için eylem yöntemini **albümleri**.</span><span class="sxs-lookup"><span data-stu-id="b5405-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="b5405-321">(Kod parçacığını - *modeller ve veri erişimi - Ex2 Store Gözat*)</span><span class="sxs-lookup"><span data-stu-id="b5405-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="b5405-322">Bir özellik adında .NET, kullanmakta olduğunuz **LINQ** (kod veritabanında yürütün ve dönüş bu koleksiyonlara karşı-kesin türü belirtilmiş sorgu ifadeleri yazmak için dil ile tümleşik sorgu) nesneleri, programlama yapabilirsiniz karşı.</span><span class="sxs-lookup"><span data-stu-id="b5405-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="b5405-323">LINQ hakkında daha fazla bilgi için lütfen [msdn sitesine](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="b5405-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="b5405-324">Güncelleştirme **dizin** tüm türleri almak için eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b5405-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="b5405-325">(Kod parçacığını - *modeller ve veri erişimi - Ex2 Store dizini*)</span><span class="sxs-lookup"><span data-stu-id="b5405-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="b5405-326">Güncelleştirme **dizin** tüm türleri almak ve liste koleksiyon dönüştürmek için eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b5405-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="b5405-327">(Kod parçacığını - *modeller ve veri erişimi - Ex2 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="b5405-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="b5405-328">Görev 5 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b5405-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="b5405-329">Bu görevde, Store dizin sayfası yerine ' % s'belirtebilseydik olanları veritabanında depolanan türleri artık göstereceğini kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="b5405-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="b5405-330">Gerekmez, çünkü görünümü şablonu değiştirmek için **StoreController** aynı döndürüyor **StoreIndexViewModel** önceki gibi ancak bu kez verileri veritabanından gelen.</span><span class="sxs-lookup"><span data-stu-id="b5405-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="b5405-331">Tuşuna basın ve çözümü yeniden **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b5405-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b5405-332">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="b5405-332">The project starts in the Home page.</span></span> <span data-ttu-id="b5405-333">Doğrulayın menüsünü **türleri** artık sabit kodlanmış bir listedir ve verileri doğrudan veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="b5405-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="b5405-335">*Veritabanından gözatma türleri*</span><span class="sxs-lookup"><span data-stu-id="b5405-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="b5405-336">Artık tüm Tarz için göz atın ve albümleri veritabanından doldurulur doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="b5405-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="b5405-337">![Veritabanından albümleri gözatma](aspnet-mvc-4-models-and-data-access/_static/image23.png "gözatma albümleri veritabanından")</span><span class="sxs-lookup"><span data-stu-id="b5405-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="b5405-338">*Veritabanından albümleri gözatma*</span><span class="sxs-lookup"><span data-stu-id="b5405-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="b5405-339">Alıştırma 3: parametrelerle veritabanını sorgulama</span><span class="sxs-lookup"><span data-stu-id="b5405-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="b5405-340">Bu alıştırmada, parametreleri kullanarak veritabanını sorgulama yapmayı ve sorgu sonucu şekillendirme kullanmayı öğreneceksiniz, numara veritabanı azaltan bir özellik verileri daha verimli bir şekilde erişir.</span><span class="sxs-lookup"><span data-stu-id="b5405-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="b5405-341">Aşağıdaki sorgu sonucu şekillendirme ile ilgili daha fazla bilgi için ziyaret [msdn makalesi](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="b5405-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="b5405-342">Görev 1 - değiştirme StoreController albümleri veritabanından alınamıyor</span><span class="sxs-lookup"><span data-stu-id="b5405-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="b5405-343">Bu görevde, değişiklik yapacağınız **StoreController** albümleri belirli bir türe almak için veritabanına erişmek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="b5405-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="b5405-344">Açık **başlamak** çözüm bulunan **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** ilk kod yaklaşımı kullanmak istiyorsanız bir klasör veya **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** Database-First yaklaşımı kullanmak istiyorsanız bir klasör.</span><span class="sxs-lookup"><span data-stu-id="b5405-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="b5405-345">Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="b5405-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="b5405-346">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="b5405-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b5405-347">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="b5405-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b5405-348">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="b5405-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b5405-349">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="b5405-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b5405-350">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="b5405-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b5405-351">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="b5405-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b5405-352">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="b5405-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="b5405-353">Açık **StoreController** değiştirmek için sınıf **Gözat** eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b5405-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="b5405-354">Bunu yapmak için **Çözüm Gezgini**, genişletme **denetleyicileri** klasörü ve çift **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="b5405-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="b5405-355">Değişiklik **Gözat** Albümler belirli bir türe'almak için eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b5405-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="b5405-356">Bunu yapmak için aşağıdaki kodu değiştirin:</span><span class="sxs-lookup"><span data-stu-id="b5405-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="b5405-357">(Kod parçacığını - *modeller ve veri erişimi - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="b5405-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="b5405-358">Varlık koleksiyonu doldurmak için kullanmanız gerekir **INCLUDE** albümleri çok almak istediğiniz belirtmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b5405-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="b5405-359">Kullanabilirsiniz. **Single()** LINQ uzantısı bu durumda yalnızca bir türe için albüm beklendiğinden.</span><span class="sxs-lookup"><span data-stu-id="b5405-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="b5405-360">**Single()** yöntem adı ile eşleşen tanımlanan değeri olacak şekilde, bu durumda tek bir türe nesne belirten bir parametre olarak bir Lambda ifadesi alır.</span><span class="sxs-lookup"><span data-stu-id="b5405-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="b5405-361">Tarz nesne alındığında de yüklenen istediğiniz diğer ilgili varlıkları belirtmenize olanak tanıyan bir özelliğin avantajlarından yararlanmak.</span><span class="sxs-lookup"><span data-stu-id="b5405-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="b5405-362">Bu özelliğin adı **sorgu sonucu şekillendirme**ve bilgi almak için veritabanına erişmek için gereken sayısını azaltmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="b5405-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="b5405-363">Bu senaryoda, Albümler almanızı Tarz için önceden getirme isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="b5405-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="b5405-364">Sorgu içeren **Genres.Include (&quot;albümleri&quot;)** ilgili Albümler istediğinizi belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="b5405-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="b5405-365">Tek veritabanı isteği Tarz hem albüm verileri alacak olduğundan bu daha verimli bir uygulamada neden olur.</span><span class="sxs-lookup"><span data-stu-id="b5405-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="b5405-366">Görev 2 - uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b5405-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="b5405-367">Bu görevde, uygulamayı çalıştırmak ve belirli bir türe albümleri veritabanından alınamıyor.</span><span class="sxs-lookup"><span data-stu-id="b5405-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="b5405-368">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b5405-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b5405-369">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="b5405-369">The project starts in the Home page.</span></span> <span data-ttu-id="b5405-370">URL'yi **/Store/göz atma? Tarz Pop =** sonuçları veritabanından alınan olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="b5405-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="b5405-371">![Türe göre gözatma](aspnet-mvc-4-models-and-data-access/_static/image24.png "türe göre göz atma")</span><span class="sxs-lookup"><span data-stu-id="b5405-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="b5405-372">*Tarama/Store/göz atma? Tarz Pop =*</span><span class="sxs-lookup"><span data-stu-id="b5405-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="b5405-373">Görev 3 - kimliğine göre albümleri erişme</span><span class="sxs-lookup"><span data-stu-id="b5405-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="b5405-374">Bu görevde, Albümler kimliklerine göre almak için önceki yordamı yinelenir.</span><span class="sxs-lookup"><span data-stu-id="b5405-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="b5405-375">Gerekirse Visual Studio'ya dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="b5405-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="b5405-376">Açık **StoreController** değiştirmek için sınıf **ayrıntıları** eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b5405-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="b5405-377">Bunu yapmak için **Çözüm Gezgini**, genişletme **denetleyicileri** klasörü ve çift **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="b5405-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="b5405-378">Değişiklik **ayrıntıları** albümleri ayrıntılarını almak için eylem yöntemine bağlı olarak kendi **kimliği**. Bunu yapmak için aşağıdaki kodu değiştirin:</span><span class="sxs-lookup"><span data-stu-id="b5405-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="b5405-379">(Kod parçacığını - *modeller ve veri erişimi - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="b5405-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="b5405-380">Görev 4 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b5405-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="b5405-381">Bu görevde, uygulama bir web tarayıcısında çalışacak ve kimliklerine göre albümü ayrıntılarını alma</span><span class="sxs-lookup"><span data-stu-id="b5405-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="b5405-382">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b5405-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b5405-383">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="b5405-383">The project starts in the Home page.</span></span> <span data-ttu-id="b5405-384">URL'yi **/Store/Details/51** türleri göz atın veya veritabanından sonuçları alınıyor olduğunu doğrulamak için bir albümü seçin.</span><span class="sxs-lookup"><span data-stu-id="b5405-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="b5405-385">![Ayrıntılar gözatma](aspnet-mvc-4-models-and-data-access/_static/image25.png "ayrıntıları gözatma")</span><span class="sxs-lookup"><span data-stu-id="b5405-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="b5405-386">*Gözatma /Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="b5405-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="b5405-387">Ayrıca, bu uygulama için Windows Azure Web siteleri aşağıdaki dağıtabilirsiniz [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="b5405-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b5405-388">Özet</span><span class="sxs-lookup"><span data-stu-id="b5405-388">Summary</span></span>

<span data-ttu-id="b5405-389">ASP.NET MVC modeller ve veri erişimi temelleri öğrenilen Bu uygulamalı laboratuvarı tamamlamak, kullanarak **veritabanı ilk** yaklaşım yanı sıra **Code First** yaklaşım:</span><span class="sxs-lookup"><span data-stu-id="b5405-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="b5405-390">Bir veritabanı verilerini kullanmak için çözüm ekleme</span><span class="sxs-lookup"><span data-stu-id="b5405-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="b5405-391">Görünüm şablonları yerine sabit kodlanmış bir veritabanından alınan veri sağlamak için denetleyicileri güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="b5405-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="b5405-392">Parametreleri kullanarak veritabanını sorgulama</span><span class="sxs-lookup"><span data-stu-id="b5405-392">How to query the database using parameters</span></span>
- <span data-ttu-id="b5405-393">Sorgu sonucu daha verimli bir şekilde veri alma şekillendirme, veritabanı erişimlerine sayısını azaltan bir özelliğini kullanma</span><span class="sxs-lookup"><span data-stu-id="b5405-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="b5405-394">Veritabanı ilk ve Code First yaklaşımları modeli ile veritabanına bağlanmak için Microsoft Entity Framework kullanma</span><span class="sxs-lookup"><span data-stu-id="b5405-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b5405-395">Ek A: Visual Studio'yu Web için Express 2012</span><span class="sxs-lookup"><span data-stu-id="b5405-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b5405-396">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="b5405-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b5405-397">Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="b5405-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b5405-398">Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="b5405-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b5405-399">Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="b5405-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="b5405-400">Tıklayarak **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="b5405-400">Click on **Install Now**.</span></span> <span data-ttu-id="b5405-401">Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b5405-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b5405-402">Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="b5405-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b5405-403">![Visual Studio Express yükleyin](aspnet-mvc-4-models-and-data-access/_static/image26.png "Visual Studio Express'i yükle")</span><span class="sxs-lookup"><span data-stu-id="b5405-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b5405-404">*Visual Studio Express yükleyin*</span><span class="sxs-lookup"><span data-stu-id="b5405-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b5405-405">Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="b5405-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşullarını kabul etme](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="b5405-407">*Lisans koşullarını kabul etme*</span><span class="sxs-lookup"><span data-stu-id="b5405-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b5405-408">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="b5405-408">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="b5405-410">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="b5405-410">*Installation progress*</span></span>
6. <span data-ttu-id="b5405-411">Yükleme tamamlandığında, tıklayın **son**.</span><span class="sxs-lookup"><span data-stu-id="b5405-411">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="b5405-413">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="b5405-413">*Installation completed*</span></span>
7. <span data-ttu-id="b5405-414">Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="b5405-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b5405-415">Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.</span><span class="sxs-lookup"><span data-stu-id="b5405-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Web kutucuğu için VS Express](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="b5405-417">*Web kutucuğu için VS Express*</span><span class="sxs-lookup"><span data-stu-id="b5405-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b5405-418">Ek B: bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="b5405-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="b5405-419">Bu ekte, Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturun ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlama gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b5405-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="b5405-420">Görev 1 Windows yeni bir Web sitesi oluşturma - Azure portalı</span><span class="sxs-lookup"><span data-stu-id="b5405-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="b5405-421">Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="b5405-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5405-422">Windows Azure'la 10 ASP.NET Web sitesini ücretsiz olarak barındırın ve ardından trafiğiniz büyüdükçe ölçeğinizi artırın.</span><span class="sxs-lookup"><span data-stu-id="b5405-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="b5405-423">Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="b5405-423">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="b5405-424">![Windows Azure Portal'da oturum açın](aspnet-mvc-4-models-and-data-access/_static/image31.png "Windows Azure Portal'da oturum açın")</span><span class="sxs-lookup"><span data-stu-id="b5405-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="b5405-425">*Windows Azure yönetim portalında oturum açın*</span><span class="sxs-lookup"><span data-stu-id="b5405-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="b5405-426">Tıklayın **yeni** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="b5405-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="b5405-427">![Yeni bir Web sitesi oluşturma](aspnet-mvc-4-models-and-data-access/_static/image32.png "yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="b5405-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="b5405-428">*Yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="b5405-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="b5405-429">Tıklayın **işlem** | **Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="b5405-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="b5405-430">Ardından **hızlı Oluştur** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="b5405-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="b5405-431">Yeni web sitesi için kullanılabilen bir URL girin ve tıklatın **Web sitesi oluştur**.</span><span class="sxs-lookup"><span data-stu-id="b5405-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5405-432">Bir Windows Azure Web sitesi kontrol edebildiğiniz ve yönetebildiğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="b5405-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="b5405-433">Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portalın dışında dağıtmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="b5405-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="b5405-434">Bir veritabanını ayarlamak için adımları içermez.</span><span class="sxs-lookup"><span data-stu-id="b5405-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="b5405-435">![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](aspnet-mvc-4-models-and-data-access/_static/image33.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="b5405-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="b5405-436">*Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="b5405-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="b5405-437">Yeni kadar bekleyin **Web sitesi** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b5405-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="b5405-438">Web sitesi oluşturulduktan sonra altındaki bağlantıya tıklayın **URL** sütun.</span><span class="sxs-lookup"><span data-stu-id="b5405-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="b5405-439">Yeni Web sitesi çalışıp çalışmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="b5405-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="b5405-440">![Yeni web sitesi için gözatma](aspnet-mvc-4-models-and-data-access/_static/image34.png "yeni web sitesine göz atma")</span><span class="sxs-lookup"><span data-stu-id="b5405-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="b5405-441">*Yeni web sitesine göz atma*</span><span class="sxs-lookup"><span data-stu-id="b5405-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="b5405-442">![Web sitesi çalışan](aspnet-mvc-4-models-and-data-access/_static/image35.png "çalışan Web sitesi")</span><span class="sxs-lookup"><span data-stu-id="b5405-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="b5405-443">*Çalışan Web sitesi*</span><span class="sxs-lookup"><span data-stu-id="b5405-443">*Web site running*</span></span>
6. <span data-ttu-id="b5405-444">Portala geri dönün ve web sitesi altında adına **adı** yönetim sayfaları görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="b5405-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="b5405-445">![Web sitesi Yönetim sayfalarının açma](aspnet-mvc-4-models-and-data-access/_static/image36.png "web sitesi Yönetim sayfalarının açma")</span><span class="sxs-lookup"><span data-stu-id="b5405-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="b5405-446">*Web Sitesi Yönetim sayfalarının açma*</span><span class="sxs-lookup"><span data-stu-id="b5405-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="b5405-447">İçinde **Pano** sayfasındaki **Hızlı Bakış** bölümünde **yayımlama profili indir** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="b5405-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5405-448">*Yayımlama profilini* tüm her etkin yayımlama yöntemi için bir Windows Azure Web sitesi için bir web uygulaması yayımlamak için gereken bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="b5405-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="b5405-449">Yayımlama profili URL'leri, kullanıcı kimlik bilgileri ve her bir yayımlama yönteminin etkinleştirildiği uç noktalarına karşı kimlik doğrulaması yapmak ve bağlanmak için gereken veritabanı dizelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="b5405-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="b5405-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma desteği yayımlama profillerini yapılandırma için bu programların otomatik hale getirmek için Windows Azure Web siteleri'ne yayımlama web uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="b5405-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="b5405-451">![Yayımlama profili web sitesi indiriliyor](aspnet-mvc-4-models-and-data-access/_static/image37.png "yayımlama profili web sitesi indiriliyor")</span><span class="sxs-lookup"><span data-stu-id="b5405-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="b5405-452">*Yayımlama profili Web sitesi indiriliyor*</span><span class="sxs-lookup"><span data-stu-id="b5405-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="b5405-453">Yayımlama profili dosyasını bilinen bir konuma indirin.</span><span class="sxs-lookup"><span data-stu-id="b5405-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="b5405-454">Daha fazla Bu alıştırmada, bu dosyayı Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlama için nasıl kullanılacağını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="b5405-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="b5405-455">![Yayımlama profili dosyasını kaydetme](aspnet-mvc-4-models-and-data-access/_static/image38.png "yayımlama profili kaydediliyor")</span><span class="sxs-lookup"><span data-stu-id="b5405-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="b5405-456">*Yayımlama profili dosyasını kaydetme*</span><span class="sxs-lookup"><span data-stu-id="b5405-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="b5405-457">Görev 2 - veritabanı sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b5405-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="b5405-458">Uygulamanızı kullanan SQL Server'ın yaparsa veritabanlarını bir SQL veritabanı sunucusu oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="b5405-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="b5405-459">SQL Server kullanmayan basit bir uygulama dağıtmak istiyorsanız bu görevi atla.</span><span class="sxs-lookup"><span data-stu-id="b5405-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="b5405-460">SQL veritabanı sunucusu, uygulama veritabanını depolamak için gerekir.</span><span class="sxs-lookup"><span data-stu-id="b5405-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="b5405-461">SQL veritabanı sunucularını, aboneliğinizde Windows Azure Yönetim Portalı'nda görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**.</span><span class="sxs-lookup"><span data-stu-id="b5405-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="b5405-462">Oluşturulan server yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğunda düğme.</span><span class="sxs-lookup"><span data-stu-id="b5405-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="b5405-463">Not **sunucu adı ve URL, yönetici oturum açma adı ve parola**, sonraki görevleri kullanacağınız.</span><span class="sxs-lookup"><span data-stu-id="b5405-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="b5405-464">Daha sonraki bir aşamasında oluşturulacak şekilde veritabanı henüz oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="b5405-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="b5405-465">![SQL veritabanı sunucu Panosu](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL veritabanı sunucu Panosu")</span><span class="sxs-lookup"><span data-stu-id="b5405-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="b5405-466">*SQL veritabanı sunucu Panosu*</span><span class="sxs-lookup"><span data-stu-id="b5405-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="b5405-467">İşlemin sonraki görev ihtiyacınız sunucunun listesinde yerel IP adresinizi eklemek, bu nedenle Visual Studio'dan veritabanı bağlantısını test eder **izin verilen IP adresleri**.</span><span class="sxs-lookup"><span data-stu-id="b5405-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="b5405-468">Bunu yapmanın tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** ve yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) düğmesi.</span><span class="sxs-lookup"><span data-stu-id="b5405-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![İstemci IP adresi ekleme](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="b5405-470">*İstemci IP adresi ekleme*</span><span class="sxs-lookup"><span data-stu-id="b5405-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="b5405-471">Bir kez **istemci IP adresi** için izin verilen IP adreslerini eklenir listesinde, tıklayın **Kaydet** değişiklikleri onaylamak için.</span><span class="sxs-lookup"><span data-stu-id="b5405-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Değişiklikleri onaylayın](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="b5405-473">*Değişiklikleri onaylayın*</span><span class="sxs-lookup"><span data-stu-id="b5405-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b5405-474">Görev 3 - bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="b5405-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="b5405-475">ASP.NET MVC 4 çözüme geri dönün.</span><span class="sxs-lookup"><span data-stu-id="b5405-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="b5405-476">İçinde **Çözüm Gezgini**, web sitesi projesini sağ tıklatın ve seçin **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="b5405-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="b5405-477">![Uygulama yayımlama](aspnet-mvc-4-models-and-data-access/_static/image43.png "uygulama yayımlama")</span><span class="sxs-lookup"><span data-stu-id="b5405-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="b5405-478">*Web sitesi yayımlama*</span><span class="sxs-lookup"><span data-stu-id="b5405-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="b5405-479">İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="b5405-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="b5405-480">![Yayımlama profilini içeri aktarma](aspnet-mvc-4-models-and-data-access/_static/image44.png "yayımlama profilini içeri aktarma")</span><span class="sxs-lookup"><span data-stu-id="b5405-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="b5405-481">*Yayımlama profilini içeri aktarma*</span><span class="sxs-lookup"><span data-stu-id="b5405-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="b5405-482">Tıklayın **bağlantısını doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="b5405-482">Click **Validate Connection**.</span></span> <span data-ttu-id="b5405-483">Doğrulama tamamlandıktan sonra tıklayın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="b5405-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5405-484">Bağlantıyı doğrula düğmesi yanında görünür yeşil bir onay işareti gördükten sonra doğrulama tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="b5405-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="b5405-485">![Bağlantı doğrulama](aspnet-mvc-4-models-and-data-access/_static/image45.png "bağlantısı doğrulanıyor")</span><span class="sxs-lookup"><span data-stu-id="b5405-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="b5405-486">*Bağlantı doğrulama*</span><span class="sxs-lookup"><span data-stu-id="b5405-486">*Validating connection*</span></span>
4. <span data-ttu-id="b5405-487">İçinde **ayarları** sayfasındaki **veritabanları** bölümünde, veritabanı bağlantının metin kutusunun yanındaki düğmeye tıklayın (yani **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="b5405-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="b5405-488">![Web dağıtımı yapılandırma](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web dağıtımı yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="b5405-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="b5405-489">*Web dağıtımı yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="b5405-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="b5405-490">Veritabanı bağlantısı aşağıdaki gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="b5405-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="b5405-491">İçinde **sunucu adı** , SQL veritabanı sunucu URL'sini kullanarak yazın *tcp:* önek.</span><span class="sxs-lookup"><span data-stu-id="b5405-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="b5405-492">İçinde **kullanıcı adı** , Sunucu Yöneticisi oturum açma adı yazın.</span><span class="sxs-lookup"><span data-stu-id="b5405-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="b5405-493">İçinde **parola** Sunucu Yöneticisi oturum açma parolanızı yazın.</span><span class="sxs-lookup"><span data-stu-id="b5405-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="b5405-494">Yeni bir veritabanı adı yazın.</span><span class="sxs-lookup"><span data-stu-id="b5405-494">Type a new database name.</span></span>

     <span data-ttu-id="b5405-495">![Hedef bağlantı dizesi yapılandırma](aspnet-mvc-4-models-and-data-access/_static/image47.png "hedef bağlantı dizesini yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="b5405-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="b5405-496">*Hedef bağlantı dizesini yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="b5405-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="b5405-497">Sonra **Tamam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b5405-497">Then click **OK**.</span></span> <span data-ttu-id="b5405-498">Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.</span><span class="sxs-lookup"><span data-stu-id="b5405-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="b5405-499">![Veritabanı oluşturma](aspnet-mvc-4-models-and-data-access/_static/image48.png "veritabanı dizesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="b5405-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="b5405-500">*Veritabanı oluşturma*</span><span class="sxs-lookup"><span data-stu-id="b5405-500">*Creating the database*</span></span>
7. <span data-ttu-id="b5405-501">Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini, varsayılan bağlantı metin kutusu içinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="b5405-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="b5405-502">Sonra **İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b5405-502">Then click **Next**.</span></span>

    <span data-ttu-id="b5405-503">![SQL veritabanı'na işaret eden bağlantı dizesi](aspnet-mvc-4-models-and-data-access/_static/image49.png "SQL veritabanına işaret eden bağlantı dizesi")</span><span class="sxs-lookup"><span data-stu-id="b5405-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="b5405-504">*SQL veritabanı'na işaret eden bağlantı dizesi*</span><span class="sxs-lookup"><span data-stu-id="b5405-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="b5405-505">İçinde **Önizleme** sayfasında **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="b5405-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="b5405-506">![Web uygulaması yayımlama](aspnet-mvc-4-models-and-data-access/_static/image50.png "web uygulaması yayımlama")</span><span class="sxs-lookup"><span data-stu-id="b5405-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="b5405-507">*Web uygulaması yayımlama*</span><span class="sxs-lookup"><span data-stu-id="b5405-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="b5405-508">Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesi açılır.</span><span class="sxs-lookup"><span data-stu-id="b5405-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="b5405-509">Ek C: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="b5405-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="b5405-510">Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır.</span><span class="sxs-lookup"><span data-stu-id="b5405-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b5405-511">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="b5405-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b5405-512">![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-models-and-data-access/_static/image51.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="b5405-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b5405-513">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="b5405-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="b5405-514">***Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="b5405-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="b5405-515">Kod eklemesini istediğiniz imleci yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="b5405-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b5405-516">(Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="b5405-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b5405-517">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="b5405-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b5405-518">Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="b5405-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b5405-519">İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="b5405-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="b5405-520">![Kod parçacığı adını yazmaya başlayın](aspnet-mvc-4-models-and-data-access/_static/image52.png "kod parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="b5405-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="b5405-521">*Kod parçacığı adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="b5405-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="b5405-522">![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](aspnet-mvc-4-models-and-data-access/_static/image53.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="b5405-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="b5405-523">*Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="b5405-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="b5405-524">![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](aspnet-mvc-4-models-and-data-access/_static/image54.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")</span><span class="sxs-lookup"><span data-stu-id="b5405-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="b5405-525">*Yeniden Tab tuşuna basın ve kod parçacığı genişletir*</span><span class="sxs-lookup"><span data-stu-id="b5405-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="b5405-526">***Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1.</span><span class="sxs-lookup"><span data-stu-id="b5405-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="b5405-527">Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b5405-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="b5405-528">Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.</span><span class="sxs-lookup"><span data-stu-id="b5405-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="b5405-529">Tıklayarak ilgili kod parçacığı listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="b5405-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="b5405-530">![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](aspnet-mvc-4-models-and-data-access/_static/image55.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")</span><span class="sxs-lookup"><span data-stu-id="b5405-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="b5405-531">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*</span><span class="sxs-lookup"><span data-stu-id="b5405-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="b5405-532">![Tıklayarak ilgili kod parçacığını listesinden çekme](aspnet-mvc-4-models-and-data-access/_static/image56.png "tıklayarak ilgili kod parçacığı listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="b5405-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="b5405-533">*Tıklayarak ilgili kod parçacığı listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="b5405-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
