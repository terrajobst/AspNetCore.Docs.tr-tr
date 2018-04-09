---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 modelleri ve veri erişimi | Microsoft Docs
author: rick-anderson
description: 'Not: Bu uygulamalı Laboratuvar ASP.NET MVC temel bilgiye sahip olduğunu varsayar. ASP.NET MVC önce kullanmadıysanız, ASP.NET MVC 4 gitmenizi öneririz...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 081a71ef67a6eee6c84058c30f9e15301afbed23
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="13aeb-104">ASP.NET MVC 4 modelleri ve veri erişimi</span><span class="sxs-lookup"><span data-stu-id="13aeb-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="13aeb-105">Tarafından [Web Camps ekibi](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="13aeb-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="13aeb-106">Kit eğitim Web Camps indirin</span><span class="sxs-lookup"><span data-stu-id="13aeb-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="13aeb-107">Bu uygulamalı Laboratuvar temel bilgiye sahip varsayar **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="13aeb-108">Değil kullandıysanız **ASP.NET MVC** gitmenizi öneririz önce **ASP.NET MVC 4 Temelleri** uygulamalı Laboratuvar.</span><span class="sxs-lookup"><span data-stu-id="13aeb-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="13aeb-109">Bu Laboratuvar, kaynak klasöre sağlanan örnek bir Web uygulamasına küçük değişiklikler uygulayarak daha önce açıklanan yeni özellikleri ve geliştirmeleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="13aeb-110">Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [Microsoft-Web/WebCampTrainingKit sürümleri](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="13aeb-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="13aeb-111">Bu Laboratuvar için belirli proje şu adresten edinilebilir [ASP.NET MVC 4 modelleri ve veri erişimi](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="13aeb-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="13aeb-112">İçinde **ASP.NET MVC Temelleri** uygulamalı Laboratuvar, sabit kodlanmış veri geçirerek denetleyicilerinden görünüm şablonları.</span><span class="sxs-lookup"><span data-stu-id="13aeb-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="13aeb-113">Ancak, gerçek bir Web uygulaması oluşturmak için gerçek bir veritabanını kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="13aeb-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="13aeb-114">Bu uygulamalı Laboratuvar depolamak ve müzik deposu uygulaması için gereken verileri almak için bir veritabanı motoru kullanmak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="13aeb-115">Bunu yapmaya yönelik var olan bir veritabanı başlatın ve varlık veri modeli oluşturmak.</span><span class="sxs-lookup"><span data-stu-id="13aeb-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="13aeb-116">Bu Laboratuvar, karşılayacağını **veritabanı ilk** yaklaşım yanı sıra **Code First** yaklaşım.</span><span class="sxs-lookup"><span data-stu-id="13aeb-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="13aeb-117">Ancak, aynı zamanda kullanabilirsiniz **Model First** yaklaşımını, araçları kullanarak aynı modelin oluşturun ve ondan veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13aeb-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="13aeb-118">![Veritabanı ilk vs. Model ilk](aspnet-mvc-4-models-and-data-access/_static/image1.png "veritabanı ilk vs. Model First")</span><span class="sxs-lookup"><span data-stu-id="13aeb-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="13aeb-119">*Veritabanı ilk vs. Model First*</span><span class="sxs-lookup"><span data-stu-id="13aeb-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="13aeb-120">Model oluşturma sonra StoreController sabit kodlanmış verileri kullanmak yerine veritabanından alınan veri deposu görünümlerini sağlamak için uygun düzeltmeleri yapar.</span><span class="sxs-lookup"><span data-stu-id="13aeb-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="13aeb-121">Görünüm şablonları için aynı ViewModels StoreController döndürme çünkü bu kez veritabanından veri gelir ancak herhangi bir değişiklik görünüm şablonlarda yapmak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="13aeb-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="13aeb-122">**İlk kod yaklaşımı**</span><span class="sxs-lookup"><span data-stu-id="13aeb-122">**The Code First Approach**</span></span>

<span data-ttu-id="13aeb-123">Code First yaklaşım bize genellikle framework ile birlikte sınıfları oluşturmadan kodu modelden tanımlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="13aeb-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="13aeb-124">Kodda, ilk olarak, model nesneleri POCOs ile tanımlanan &quot;düz eski CLR nesnelerini&quot;.</span><span class="sxs-lookup"><span data-stu-id="13aeb-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="13aeb-125">POCOs hiçbir devralma ve arabirimleri kullanılmaz basit düz sınıflarıdır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="13aeb-126">Biz otomatik olarak veritabanı onlardan oluşturabilir veya biz varolan bir veritabanını kullanın ve sınıf eşleme kodundan oluşturduğunuz.</span><span class="sxs-lookup"><span data-stu-id="13aeb-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="13aeb-127">Bu yaklaşımı kullanmanın avantajları olan POCOs sınıfları eşleme framework ile bağlı değil olarak Model (Bu durumda, Entity Framework), Kalıcılık çerçeveden bağımsız olarak kalır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="13aeb-128">Bu Laboratuvar ASP.NET MVC 4 temel alır ve müzik deposu örnek uygulama sürümü özelleştirilmiş ve yalnızca bu uygulamalı laboratuar ortamında gösterilen özellikleri uyacak şekilde simge durumuna küçültülmüş.</span><span class="sxs-lookup"><span data-stu-id="13aeb-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="13aeb-129">Bütün araştırmak istiyorsanız **müzik deposu** içinde bulabilirsiniz öğretici uygulama [MVC müzik deposu](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="13aeb-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="13aeb-130">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="13aeb-130">Prerequisites</span></span>

<span data-ttu-id="13aeb-131">Bu laboratuvarı tamamlamak için aşağıdaki öğeleri sahip olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="13aeb-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="13aeb-132">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="13aeb-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="13aeb-133">Kurulum</span><span class="sxs-lookup"><span data-stu-id="13aeb-133">Setup</span></span>

<span data-ttu-id="13aeb-134">**Kod parçacıkları yükleme**</span><span class="sxs-lookup"><span data-stu-id="13aeb-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="13aeb-135">Kolaylık olması için bu Laboratuvar yönetme kod çoğunu Visual Studio kod parçacıkları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="13aeb-136">Çalıştırma kod parçacıkları yüklemek için **.\Source\Setup\CodeSnippets.vsi** dosya.</span><span class="sxs-lookup"><span data-stu-id="13aeb-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="13aeb-137">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek C: kullanarak kod parçacıkları](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="13aeb-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="13aeb-138">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="13aeb-138">Exercises</span></span>

<span data-ttu-id="13aeb-139">Bu uygulamalı Laboratuvar tarafından aşağıdaki alıştırmaları oluşmaktadır:</span><span class="sxs-lookup"><span data-stu-id="13aeb-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="13aeb-140">Alıştırma 1: bir veritabanına ekleme</span><span class="sxs-lookup"><span data-stu-id="13aeb-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="13aeb-141">Alıştırma 2: Code First kullanarak veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="13aeb-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="13aeb-142">Alıştırma 3: parametrelerle veritabanını sorgulama</span><span class="sxs-lookup"><span data-stu-id="13aeb-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="13aeb-143">Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="13aeb-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="13aeb-144">Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="13aeb-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="13aeb-145">Bu laboratuvarı tamamlamak için süre tahmini: **35 dakika**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="13aeb-146">Alıştırma 1: bir veritabanına ekleme</span><span class="sxs-lookup"><span data-stu-id="13aeb-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="13aeb-147">Bu alıştırmada, kendi veri tüketmek için çözüm MusicStore uygulamaya tablolar ile bir veritabanı eklemek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="13aeb-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="13aeb-148">Veritabanı modeli ile oluşturulan ve çözüme eklendi sonra Görünüm şablonu sabit kodlanmış değerler yerine veritabanından alınan veri sağlamak için StoreController sınıfı değiştirecektir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="13aeb-149">Görev 1 - bir veritabanı ekleme</span><span class="sxs-lookup"><span data-stu-id="13aeb-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="13aeb-150">Bu görevde, çözüme MusicStore uygulamasının ana tablolarla önceden oluşturulmuş bir veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="13aeb-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="13aeb-151">Açık **başlamak** çözüm bulunan **kaynak/Ex1-AddingADatabaseDBFirst/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="13aeb-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="13aeb-152">Bazı eksik NuGet paketlerini indirmek gerekir devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="13aeb-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="13aeb-153">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="13aeb-154">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="13aeb-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="13aeb-155">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="13aeb-156">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="13aeb-157">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="13aeb-158">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="13aeb-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="13aeb-159">Ekleme **MvcMusicStore** veritabanı dosyası.</span><span class="sxs-lookup"><span data-stu-id="13aeb-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="13aeb-160">Bu uygulamalı laboratuar ortamında adlı bir veritabanı zaten oluşturuldu kullanacağı **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="13aeb-161">Bunu yapmak için sağ **uygulama\_veri** klasörünü **Ekle** ve ardından **varolan öğeyi**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="13aeb-162">Gözat **\Source\Assets** seçip **MvcMusicStore.mdf** dosya.</span><span class="sxs-lookup"><span data-stu-id="13aeb-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="13aeb-163">![Var olan bir öğe ekleme](aspnet-mvc-4-models-and-data-access/_static/image2.png "var olan bir öğe ekleme")</span><span class="sxs-lookup"><span data-stu-id="13aeb-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="13aeb-164">*Var olan bir öğe ekleme*</span><span class="sxs-lookup"><span data-stu-id="13aeb-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="13aeb-165">![MvcMusicStore.mdf veritabanı dosyası](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf veritabanı dosyası")</span><span class="sxs-lookup"><span data-stu-id="13aeb-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="13aeb-166">*MvcMusicStore.mdf veritabanı dosyası*</span><span class="sxs-lookup"><span data-stu-id="13aeb-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="13aeb-167">Veritabanı projeye eklendi.</span><span class="sxs-lookup"><span data-stu-id="13aeb-167">The database has been added to the project.</span></span> <span data-ttu-id="13aeb-168">Veritabanı içinde çözüm bile bulunur, sorgu ve onu farklı bir veritabanı sunucusu barındırılan gibi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="13aeb-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="13aeb-169">![Çözüm Gezgini'nde MvcMusicStore veritabanı](aspnet-mvc-4-models-and-data-access/_static/image4.png "Çözüm Gezgini'nde MvcMusicStore veritabanı")</span><span class="sxs-lookup"><span data-stu-id="13aeb-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="13aeb-170">*Çözüm Gezgini'nde MvcMusicStore veritabanı*</span><span class="sxs-lookup"><span data-stu-id="13aeb-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="13aeb-171">Veritabanı bağlantısı doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-171">Verify the connection to the database.</span></span> <span data-ttu-id="13aeb-172">Bunu yapmak için çift **MvcMusicStore.mdf** bağlantı kuramıyor.</span><span class="sxs-lookup"><span data-stu-id="13aeb-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="13aeb-173">![MvcMusicStore.mdf için bağlanma](aspnet-mvc-4-models-and-data-access/_static/image5.png "MvcMusicStore.mdf için bağlanma")</span><span class="sxs-lookup"><span data-stu-id="13aeb-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="13aeb-174">*MvcMusicStore.mdf için bağlanma*</span><span class="sxs-lookup"><span data-stu-id="13aeb-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="13aeb-175">Görev 2 - bir veri modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="13aeb-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="13aeb-176">Bu görevde, önceki görevde eklenen veritabanıyla etkileşim kurmak için bir veri modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="13aeb-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="13aeb-177">Veritabanı temsil edecek bir veri modeli oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13aeb-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="13aeb-178">Bu, Çözüm Gezgini içinde yapmak için sağ **modelleri** klasörünü **Ekle** ve ardından **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="13aeb-179">İçinde **Yeni Öğe Ekle** iletişim kutusunda **veri** şablonu ve ardından **ADO.NET varlık veri modeli** öğesi.</span><span class="sxs-lookup"><span data-stu-id="13aeb-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="13aeb-180">Veri modeli adı değiştirmek **StoreDB.edmx** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="13aeb-181">![StoreDB ADO.NET varlık veri modeli ekleme](aspnet-mvc-4-models-and-data-access/_static/image6.png "StoreDB ADO.NET varlık veri modeli ekleme")</span><span class="sxs-lookup"><span data-stu-id="13aeb-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="13aeb-182">*StoreDB ADO.NET varlık veri modeli ekleme*</span><span class="sxs-lookup"><span data-stu-id="13aeb-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="13aeb-183">**Varlık veri modeli Sihirbazı** görünür.</span><span class="sxs-lookup"><span data-stu-id="13aeb-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="13aeb-184">Bu sihirbaz, modeli katmanı oluşturulmasını yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="13aeb-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="13aeb-185">Model eklenen varolan veritabanı recentyl tabanlı oluşturulmalıdır beri seçin **veritabanından Oluştur** tıklatıp **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="13aeb-186">![Model içeriğinin seçme](aspnet-mvc-4-models-and-data-access/_static/image7.png "model içeriğinin seçme")</span><span class="sxs-lookup"><span data-stu-id="13aeb-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="13aeb-187">*Model içeriğinin seçme*</span><span class="sxs-lookup"><span data-stu-id="13aeb-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="13aeb-188">Model bir veritabanından oluşturma olduğundan, bağlantıyı kullanacak şekilde belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="13aeb-189">Tıklatın **yeni bağlantı**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="13aeb-190">Seçin **Microsoft SQL Server veritabanı dosyası** tıklatıp **devam**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="13aeb-191">![Veri Kaynağı Seç](aspnet-mvc-4-models-and-data-access/_static/image8.png "Seç veri kaynağı")</span><span class="sxs-lookup"><span data-stu-id="13aeb-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="13aeb-192">*Veri kaynağı iletişim seçin*</span><span class="sxs-lookup"><span data-stu-id="13aeb-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="13aeb-193">Tıklatın **Gözat** ve veritabanını seçin **MvcMusicStore.mdf** içinde bulunan **uygulama\_veri** klasörü ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="13aeb-194">![Bağlantı özellikleri](aspnet-mvc-4-models-and-data-access/_static/image9.png "bağlantı özellikleri")</span><span class="sxs-lookup"><span data-stu-id="13aeb-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="13aeb-195">*Bağlantı özellikleri*</span><span class="sxs-lookup"><span data-stu-id="13aeb-195">*Connection properties*</span></span>
6. <span data-ttu-id="13aeb-196">Oluşturulan sınıf varlık bağlantı dizesi ile aynı ada sahip, bu nedenle adının değiştirmek **MusicStoreEntities** tıklatıp **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="13aeb-197">![Veri bağlantısı seçme](aspnet-mvc-4-models-and-data-access/_static/image10.png "veri bağlantısı seçme")</span><span class="sxs-lookup"><span data-stu-id="13aeb-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="13aeb-198">*Veri bağlantısı seçme*</span><span class="sxs-lookup"><span data-stu-id="13aeb-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="13aeb-199">Kullanmak için veritabanı nesneleri seçin.</span><span class="sxs-lookup"><span data-stu-id="13aeb-199">Choose the database objects to use.</span></span> <span data-ttu-id="13aeb-200">Varlık modeli yalnızca veritabanı tabloları kullanacak şekilde seçin **tabloları** seçeneği ve olduğundan emin olun **yabancı anahtar sütunlarını modele dahil** ve **Pluralize veya tekil hale getirin oluşturulan Nesne adları** seçenekleri da seçilir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="13aeb-201">Model Namespace değiştirme **MvcMusicStore.Model** tıklatıp **son**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="13aeb-202">![Veritabanı nesneleri seçme](aspnet-mvc-4-models-and-data-access/_static/image11.png "veritabanı nesneleri seçme")</span><span class="sxs-lookup"><span data-stu-id="13aeb-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="13aeb-203">*Veritabanı nesneleri seçme*</span><span class="sxs-lookup"><span data-stu-id="13aeb-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="13aeb-204">Bir güvenlik uyarısı iletişim kutusu gösterilirse, tıklatın **Tamam** şablonu çalıştırın ve model varlıkların sınıfları oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="13aeb-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="13aeb-205">Veritabanı için bir varlığın diyagram, her tablo veritabanına eşlemeleri ayrı bir sınıf oluşturulacak sırada görünür.</span><span class="sxs-lookup"><span data-stu-id="13aeb-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="13aeb-206">Örneğin, **albümleri** tabloda belirtildiği bir **albüm** sınıfı, burada tablodaki her sütun eşlemek için bir sınıf özelliği.</span><span class="sxs-lookup"><span data-stu-id="13aeb-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="13aeb-207">Bu, sorgu ve veritabanında satır temsil eden nesneler üzerinde çalışmak olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="13aeb-208">![Varlık diyagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "varlık diyagramı")</span><span class="sxs-lookup"><span data-stu-id="13aeb-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="13aeb-209">*Varlık diyagramı*</span><span class="sxs-lookup"><span data-stu-id="13aeb-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="13aeb-210">T4 şablonları (.tt) varlıklar sınıfları oluşturmak için kodu çalıştırın ve aynı ada sahip mevcut sınıflarını üzerine yazar.</span><span class="sxs-lookup"><span data-stu-id="13aeb-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="13aeb-211">Bu örnekte, sınıfları &quot;albüm&quot;, &quot;Tarz&quot; ve &quot;sanatçı&quot; ile oluşturulan kodun verilerinin üzerine yazıldı.</span><span class="sxs-lookup"><span data-stu-id="13aeb-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="13aeb-212">Görev 3 - uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="13aeb-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="13aeb-213">Model oluşturma kaldırıldı. ancak bu görevde, kontrol eder **albüm**, **Tarz** ve **sanatçı** modeli sınıfları, projenin başarıyla kullanarak oluşturur Yeni veri modeli sınıfları.</span><span class="sxs-lookup"><span data-stu-id="13aeb-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="13aeb-214">Seçerek Projeyi derlemek **yapı** menü öğesini ve ardından **yapı MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="13aeb-215">![Proje derleme](aspnet-mvc-4-models-and-data-access/_static/image13.png "projesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="13aeb-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="13aeb-216">*Proje derleme*</span><span class="sxs-lookup"><span data-stu-id="13aeb-216">*Building the project*</span></span>
2. <span data-ttu-id="13aeb-217">Proje başarıyla oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="13aeb-217">The project builds successfully.</span></span> <span data-ttu-id="13aeb-218">Neden hala çalışır?</span><span class="sxs-lookup"><span data-stu-id="13aeb-218">Why does it still work?</span></span> <span data-ttu-id="13aeb-219">Veritabanı tablolarını kaldırılan sınıflarda kullanmakta olduğunuz özellikleri içeren alanlar olduğundan çalışır **albüm** ve **Tarz**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="13aeb-220">![Başarılı bir şekilde derlemeleri](aspnet-mvc-4-models-and-data-access/_static/image14.png "başarılı derlemeleri")</span><span class="sxs-lookup"><span data-stu-id="13aeb-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="13aeb-221">*Derlemeleri başarılı oldu*</span><span class="sxs-lookup"><span data-stu-id="13aeb-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="13aeb-222">Tasarımcı varlıklar diyagramı biçiminde görüntüler olsa da, bunlar gerçekten C# sınıflarıdır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="13aeb-223">Genişletme **StoreDB.edmx** Çözüm Gezgini'nde düğümünü ve ardından **StoreDB.tt**, yeni oluşturulan varlıkları görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="13aeb-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="13aeb-224">![Oluşturulan dosyaları](aspnet-mvc-4-models-and-data-access/_static/image15.png "oluşturulan dosyaları")</span><span class="sxs-lookup"><span data-stu-id="13aeb-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="13aeb-225">*Oluşturulan dosyalar*</span><span class="sxs-lookup"><span data-stu-id="13aeb-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="13aeb-226">Görev 4 - veritabanını sorgulama</span><span class="sxs-lookup"><span data-stu-id="13aeb-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="13aeb-227">Bu görevde, sabit kodlanmış verileri kullanmak yerine, bu bilgileri almak için veritabanı sorgulayacak böylece StoreController sınıfı güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="13aeb-228">Açık **Controllers\StoreController.cs** ve aşağıdaki alan örneği tutacak sınıfına ekleyin **MusicStoreEntities** adlı sınıf **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="13aeb-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="13aeb-229">(Kod parçacığını - *modelleri ve veri erişimi - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
~~~
2. <span data-ttu-id="13aeb-230">**MusicStoreEntities** sınıfı veritabanındaki her tablo için bir koleksiyon özelliği sunar.</span><span class="sxs-lookup"><span data-stu-id="13aeb-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="13aeb-231">Güncelleştirme **Gözat** bir tarzını tüm almak için eylem yöntemini **albümleri**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="13aeb-232">(Kod parçacığını - *modelleri ve veri erişimi - Ex1 deposu Gözat*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
~~~
3. <span data-ttu-id="13aeb-233">Güncelleştirme **dizin** tüm türler almak için eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="13aeb-233">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="13aeb-234">(Kod parçacığını - *modelleri ve veri erişimi - Ex1 deposu dizini*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-234">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
~~~
4. <span data-ttu-id="13aeb-235">Güncelleştirme **dizin** tüm türler almak ve bir liste koleksiyona dönüştürmek için eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="13aeb-235">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="13aeb-236">(Kod parçacığını - *modelleri ve veri erişimi - Ex1 deposu GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-236">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]
~~~

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="13aeb-237">Görev 5 - Uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="13aeb-237">Task 5 - Running the Application</span></span>

<span data-ttu-id="13aeb-238">Bu görevde deposu dizin sayfası kodlanmış olanları yerine veritabanında depolanan türler artık göstereceğini kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="13aeb-238">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="13aeb-239">Şablonu görüntüleme çünkü değiştirmenize gerek yoktur **StoreController** bu kez veritabanından veri gelir ancak aynı varlık önceki gibi döndüren.</span><span class="sxs-lookup"><span data-stu-id="13aeb-239">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="13aeb-240">Tuşuna basın ve çözüm yeniden **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-240">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="13aeb-241">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-241">The project starts in the Home page.</span></span> <span data-ttu-id="13aeb-242">Doğrulayın menüsünü **türler** artık sabit kodlanmış bir listesidir ve verileri doğrudan veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-242">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="13aeb-244">*Veritabanından gözatma türler*</span><span class="sxs-lookup"><span data-stu-id="13aeb-244">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="13aeb-245">Şimdi bir tarzını gözatın ve albümleri veritabanından doldurulur doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-245">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="13aeb-246">![Veritabanından albümleri gözatma](aspnet-mvc-4-models-and-data-access/_static/image17.png "gözatma albümleri veritabanından")</span><span class="sxs-lookup"><span data-stu-id="13aeb-246">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="13aeb-247">*Veritabanından albümleri gözatma*</span><span class="sxs-lookup"><span data-stu-id="13aeb-247">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="13aeb-248">Alıştırma 2: İlk kod kullanarak bir veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="13aeb-248">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="13aeb-249">Bu alıştırmada, kod ilk yaklaşım MusicStore uygulama tablolarla bir veritabanı oluşturmak için nasıl kullanılacağı ve onun verilere nasıl erişileceğini öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="13aeb-249">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="13aeb-250">Model oluşturulduktan sonra Görünüm şablonu sabit kodlanmış değerler yerine veritabanından alınan veri sağlamak için StoreController değiştirecektir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-250">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="13aeb-251">Alıştırma 1 tamamladıktan ve veritabanı ilk yaklaşımda zaten çalıştıysa farklı bir işlem ile aynı sonuçları almak nasıl şimdi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="13aeb-251">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="13aeb-252">Alıştırma 1 ortak olan görevleri okuma kolaylaştırmak için işaretlendi.</span><span class="sxs-lookup"><span data-stu-id="13aeb-252">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="13aeb-253">Alıştırma 1 tamamlamadıysanız, ancak ilk kod yaklaşımı öğrenmek ister misiniz Bu alıştırmada başlatın ve tam kapsamı konunun alın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-253">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="13aeb-254">Görev 1 - doldurma örnek veriler</span><span class="sxs-lookup"><span data-stu-id="13aeb-254">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="13aeb-255">Kod ilk kullanılarak başlangışta oluşturulduğunda bu görevde, veritabanı örnek verileri ile doldurur.</span><span class="sxs-lookup"><span data-stu-id="13aeb-255">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="13aeb-256">Açık **başlamak** çözüm bulunan **kaynak/Ex2-CreatingADatabaseCodeFirst/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="13aeb-256">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="13aeb-257">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="13aeb-257">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="13aeb-258">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="13aeb-258">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="13aeb-259">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-259">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="13aeb-260">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="13aeb-260">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="13aeb-261">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-261">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="13aeb-262">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-262">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="13aeb-263">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-263">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="13aeb-264">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="13aeb-264">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="13aeb-265">Ekleme **SampleData.cs** dosya **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="13aeb-265">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="13aeb-266">Bunu yapmak için sağ **modelleri** klasörünü **Ekle** ve ardından **varolan öğeyi**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-266">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="13aeb-267">Gözat **\Source\Assets** seçip **SampleData.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="13aeb-267">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="13aeb-268">![Örnek verileri doldurmak kod](aspnet-mvc-4-models-and-data-access/_static/image18.png "örnek verileri doldurmak kodu")</span><span class="sxs-lookup"><span data-stu-id="13aeb-268">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="13aeb-269">*Örnek verileri kod doldurma*</span><span class="sxs-lookup"><span data-stu-id="13aeb-269">*Sample data populate code*</span></span>
3. <span data-ttu-id="13aeb-270">Açık **Global.asax.cs** dosya ve aşağıdakileri ekleyin *kullanarak* deyimleri.</span><span class="sxs-lookup"><span data-stu-id="13aeb-270">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="13aeb-271">(Kod parçacığını - *modelleri ve veri erişimi - Ex2 genel Asax kullanımları*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-271">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
~~~
4. <span data-ttu-id="13aeb-272">İçinde **uygulama\_Start()** yöntemi veritabanı Başlatıcısı ayarlamak için aşağıdaki satırı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="13aeb-272">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="13aeb-273">(Kod parçacığını - *modelleri ve veri erişimi - Ex2 genel Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]
~~~

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="13aeb-274">Görev 2 - veritabanı bağlantısı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="13aeb-274">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="13aeb-275">Projemizin için bir veritabanı zaten eklenmiş, yazacağınız **Web.config** bağlantı dizesi dosya.</span><span class="sxs-lookup"><span data-stu-id="13aeb-275">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="13aeb-276">Konumunda bir bağlantı dizesi eklemek **Web.config**. Bunu yapmak için açık **Web.config** proje kök ve bağlantı dizesi bu satırda ile DefaultConnection adlı Değiştir **&lt;connectionStrings&gt;** bölümü:</span><span class="sxs-lookup"><span data-stu-id="13aeb-276">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="13aeb-277">![Web.config dosyası konumu](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config dosyası konumu")</span><span class="sxs-lookup"><span data-stu-id="13aeb-277">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="13aeb-278">*Web.config dosyası konumu*</span><span class="sxs-lookup"><span data-stu-id="13aeb-278">*Web.config file location*</span></span>


~~~
[!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="13aeb-279">Görev 3 - modeli ile çalışma</span><span class="sxs-lookup"><span data-stu-id="13aeb-279">Task 3 - Working with the Model</span></span>

<span data-ttu-id="13aeb-280">Veritabanı bağlantısı zaten yapılandırdığınıza göre veritabanı tablolarını modeliyle bağlayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="13aeb-280">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="13aeb-281">Bu görevde, Code First olan veritabanına bağlı bir sınıf oluşturur.</span><span class="sxs-lookup"><span data-stu-id="13aeb-281">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="13aeb-282">Değiştirilmesi gereken mevcut bir POCO model sınıfı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-282">Remember that there is an existent POCO model class that should be modified.</span></span>

   > [!NOTE]
> <span data-ttu-id="13aeb-283">Alıştırma 1 tamamladıysa, bu adım sihirbaz tarafından gerçekleştirildi Not.</span><span class="sxs-lookup"><span data-stu-id="13aeb-283">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="13aeb-284">Code First yaparak, veri varlıklarına bağlı sınıfları el ile oluşturur.</span><span class="sxs-lookup"><span data-stu-id="13aeb-284">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>


1. <span data-ttu-id="13aeb-285">POCO model sınıfı açmak **Tarz** gelen **modelleri** proje klasörünü ve bir kimliğini de ekleyin.</span><span class="sxs-lookup"><span data-stu-id="13aeb-285">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="13aeb-286">İnt özelliği addaki **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-286">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="13aeb-287">(Kod parçacığını - *modelleri ve veri erişimi - Ex2 kod ilk Tarz*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-287">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

> [!NOTE]
> To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.
> 
> You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
~~~
2. <span data-ttu-id="13aeb-288">Şimdi, POCO model sınıfı açmak **albüm** gelen **modelleri** proje klasörünü ve yabancı anahtarlar dahil, adlarıyla özellikleri oluşturma **GenreId** ve  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-288">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="13aeb-289">Bu sınıf zaten **GenreId** birincil anahtar.</span><span class="sxs-lookup"><span data-stu-id="13aeb-289">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="13aeb-290">(Kod parçacığını - *modelleri ve veri erişimi - Ex2 kod ilk albüm*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-290">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
~~~
3. <span data-ttu-id="13aeb-291">POCO model sınıfı açmak **sanatçı** ve dahil **ArtistId** özelliği.</span><span class="sxs-lookup"><span data-stu-id="13aeb-291">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="13aeb-292">(Kod parçacığını - *modelleri ve veri erişimi - Ex2 kod ilk sanatçı*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-292">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
~~~
4. <span data-ttu-id="13aeb-293">Sağ **modelleri** proje klasörünü ve select **Ekle | Sınıf**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-293">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="13aeb-294">Dosya adı **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-294">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="13aeb-295">Ardından **Ekle.**</span><span class="sxs-lookup"><span data-stu-id="13aeb-295">Then, click **Add.**</span></span>

    <span data-ttu-id="13aeb-296">![Sınıf ekleme](aspnet-mvc-4-models-and-data-access/_static/image20.png "sınıf ekleme")</span><span class="sxs-lookup"><span data-stu-id="13aeb-296">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="13aeb-297">*Yeni bir öğe ekleme*</span><span class="sxs-lookup"><span data-stu-id="13aeb-297">*Adding a new item*</span></span>

    <span data-ttu-id="13aeb-298">![Bir Ders2 ekleme](aspnet-mvc-4-models-and-data-access/_static/image21.png "bir Ders2 ekleme")</span><span class="sxs-lookup"><span data-stu-id="13aeb-298">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="13aeb-299">*Sınıf ekleme*</span><span class="sxs-lookup"><span data-stu-id="13aeb-299">*Adding a class*</span></span>
5. <span data-ttu-id="13aeb-300">Az önce oluşturduğunuz, sınıfın açık **MusicStoreEntities.cs**ve ad alanlarını dahil **System.Data.Entity** ve **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-300">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
~~~
6. <span data-ttu-id="13aeb-301">Genişletmek için sınıf bildirimi Değiştir **DbContext** sınıfı: Genel bildirme **DBSet** ve geçersiz kılma **OnModelCreating** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="13aeb-301">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="13aeb-302">Bu adımdan sonra modelinizi Entity Framework bağlayacak bir etki alanı sınıf alırsınız.</span><span class="sxs-lookup"><span data-stu-id="13aeb-302">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="13aeb-303">Bunu yapmak için sınıf kodu aşağıdakilerle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="13aeb-303">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="13aeb-304">(Kod parçacığını - *modelleri ve veri erişimi - Ex2 kod ilk MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-304">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre. By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table. You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)
~~~

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="13aeb-305">Görev 4 - veritabanını sorgulama</span><span class="sxs-lookup"><span data-stu-id="13aeb-305">Task 4 - Querying the Database</span></span>

<span data-ttu-id="13aeb-306">Bu görevde, sabit kodlanmış verileri kullanmak yerine, onu veritabanından alır, böylece StoreController sınıfı güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-306">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="13aeb-307">Bu görev ortak alıştırma 1 ' dir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-307">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="13aeb-308">Alıştırma 1 tamamladıysanız şu adımları her iki yaklaşımın aynı olduğunu unutmayın (ilk veritabanı veya ilk kod).</span><span class="sxs-lookup"><span data-stu-id="13aeb-308">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="13aeb-309">Verileri nasıl modelle bağlı olarak farklı, ancak veri varlıklara erişim henüz denetleyicisinden saydamdır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-309">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="13aeb-310">Açık **Controllers\StoreController.cs** ve aşağıdaki alan örneği tutacak sınıfına ekleyin **MusicStoreEntities** adlı sınıf **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="13aeb-310">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="13aeb-311">(Kod parçacığını - *modelleri ve veri erişimi - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-311">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
~~~
2. <span data-ttu-id="13aeb-312">**MusicStoreEntities** sınıfı veritabanındaki her tablo için bir koleksiyon özelliği sunar.</span><span class="sxs-lookup"><span data-stu-id="13aeb-312">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="13aeb-313">Güncelleştirme **Gözat** bir tarzını tüm almak için eylem yöntemini **albümleri**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-313">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="13aeb-314">(Kod parçacığını - *modelleri ve veri erişimi - Ex2 deposu Gözat*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-314">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
~~~
3. <span data-ttu-id="13aeb-315">Güncelleştirme **dizin** tüm türler almak için eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="13aeb-315">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="13aeb-316">(Kod parçacığını - *modelleri ve veri erişimi - Ex2 deposu dizini*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-316">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
~~~
4. <span data-ttu-id="13aeb-317">Güncelleştirme **dizin** tüm türler almak ve bir liste koleksiyona dönüştürmek için eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="13aeb-317">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="13aeb-318">(Kod parçacığını - *modelleri ve veri erişimi - Ex2 deposu GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-318">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]
~~~

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="13aeb-319">Görev 5 - Uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="13aeb-319">Task 5 - Running the Application</span></span>

<span data-ttu-id="13aeb-320">Bu görevde deposu dizin sayfası kodlanmış olanları yerine veritabanında depolanan türler artık göstereceğini kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="13aeb-320">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="13aeb-321">Şablonu görüntüleme çünkü değiştirmenize gerek yoktur **StoreController** aynı döndürmektir **StoreIndexViewModel** eskisi ancak bu kez verileri veritabanından gelen.</span><span class="sxs-lookup"><span data-stu-id="13aeb-321">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="13aeb-322">Tuşuna basın ve çözüm yeniden **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-322">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="13aeb-323">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-323">The project starts in the Home page.</span></span> <span data-ttu-id="13aeb-324">Doğrulayın menüsünü **türler** artık sabit kodlanmış bir listesidir ve verileri doğrudan veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-324">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="13aeb-326">*Veritabanından gözatma türler*</span><span class="sxs-lookup"><span data-stu-id="13aeb-326">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="13aeb-327">Şimdi bir tarzını gözatın ve albümleri veritabanından doldurulur doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-327">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="13aeb-328">![Veritabanından albümleri gözatma](aspnet-mvc-4-models-and-data-access/_static/image23.png "gözatma albümleri veritabanından")</span><span class="sxs-lookup"><span data-stu-id="13aeb-328">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="13aeb-329">*Veritabanından albümleri gözatma*</span><span class="sxs-lookup"><span data-stu-id="13aeb-329">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="13aeb-330">Alıştırma 3: parametrelerle veritabanını sorgulama</span><span class="sxs-lookup"><span data-stu-id="13aeb-330">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="13aeb-331">Bu alıştırmada parametreleri kullanarak veritabanını sorgulama ve sorgu sonucu şekillendirme kullanmayı öğreneceksiniz, numara veritabanı azaltan bir özellik alma verileri daha verimli bir şekilde erişir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-331">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="13aeb-332">Sorgu sonucu şekillendirme hakkında daha fazla bilgi için aşağıdaki ziyaret [msdn makalesine](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="13aeb-332">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="13aeb-333">Görev 1 - değiştirme StoreController albümleri veritabanından almak için</span><span class="sxs-lookup"><span data-stu-id="13aeb-333">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="13aeb-334">Bu görevde, değişiklik yapacağınız **StoreController** belirli bir tarzını albümleri almak için veritabanına erişmek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="13aeb-334">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="13aeb-335">Açık **başlamak** çözüm bulunan **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** ilk kod yaklaşımı kullanmak istiyorsanız, klasör veya **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** veritabanı ilk yaklaşımı kullanmak istiyorsanız, klasör.</span><span class="sxs-lookup"><span data-stu-id="13aeb-335">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="13aeb-336">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="13aeb-336">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="13aeb-337">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="13aeb-337">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="13aeb-338">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-338">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="13aeb-339">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="13aeb-339">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="13aeb-340">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-340">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="13aeb-341">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-341">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="13aeb-342">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-342">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="13aeb-343">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="13aeb-343">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="13aeb-344">Açık **StoreController** değiştirmek için sınıf **Gözat** eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="13aeb-344">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="13aeb-345">Bunu yapmak için **Çözüm Gezgini**, genişletin **denetleyicileri** klasörü ve çift **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-345">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="13aeb-346">Değişiklik **Gözat** belirli bir tarzını albümleri almak için eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="13aeb-346">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="13aeb-347">Bunu yapmak için aşağıdaki kodu değiştirin:</span><span class="sxs-lookup"><span data-stu-id="13aeb-347">To do this, replace the following code:</span></span>

    <span data-ttu-id="13aeb-348">(Kod parçacığını - *modelleri ve veri erişimi - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-348">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too. You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album. The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.
> 
> You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved. This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information. In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.
> 
> The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well. This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="13aeb-349">Görev 2 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="13aeb-349">Task 2 - Running the Application</span></span>

<span data-ttu-id="13aeb-350">Bu görevde, uygulamayı çalıştırın ve belirli bir tarzını albümleri veritabanından.</span><span class="sxs-lookup"><span data-stu-id="13aeb-350">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="13aeb-351">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-351">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="13aeb-352">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-352">The project starts in the Home page.</span></span> <span data-ttu-id="13aeb-353">URL'ye değiştirin **/deposu/Gözat? Tarz Pop =** sonuçları veritabanından alınmakta olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-353">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="13aeb-354">![Türe göre gözatma](aspnet-mvc-4-models-and-data-access/_static/image24.png "türe göre gözatma")</span><span class="sxs-lookup"><span data-stu-id="13aeb-354">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="13aeb-355">*Tarama/deposu/Gözat? Tarz Pop =*</span><span class="sxs-lookup"><span data-stu-id="13aeb-355">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="13aeb-356">Görev 3 - kimliği albümlerini erişme</span><span class="sxs-lookup"><span data-stu-id="13aeb-356">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="13aeb-357">Bu görevde, Albümler kimliklerine göre almak için önceki yordamı tekrar edilir</span><span class="sxs-lookup"><span data-stu-id="13aeb-357">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="13aeb-358">Gerekirse Visual Studio'ya dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-358">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="13aeb-359">Açık **StoreController** değiştirmek için sınıf **ayrıntıları** eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="13aeb-359">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="13aeb-360">Bunu yapmak için **Çözüm Gezgini**, genişletin **denetleyicileri** klasörü ve çift **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-360">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="13aeb-361">Değişiklik **ayrıntıları** albümleri ayrıntıları almak için eylem yöntemine bağlı olarak kendi **kimliği**. Bunu yapmak için aşağıdaki kodu değiştirin:</span><span class="sxs-lookup"><span data-stu-id="13aeb-361">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="13aeb-362">(Kod parçacığını - *modelleri ve veri erişimi - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="13aeb-362">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="13aeb-363">Görev 4 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="13aeb-363">Task 4 - Running the Application</span></span>

<span data-ttu-id="13aeb-364">Bu görevde, uygulama bir web tarayıcısında çalışacak ve kimliklerine göre Albüm ayrıntılarını alma</span><span class="sxs-lookup"><span data-stu-id="13aeb-364">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="13aeb-365">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-365">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="13aeb-366">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-366">The project starts in the Home page.</span></span> <span data-ttu-id="13aeb-367">URL'ye değiştirin **/Store/Details/51** veya türler göz atın ve sonuçları veritabanından alınmakta olduğunu doğrulamak için bir albümü seçin.</span><span class="sxs-lookup"><span data-stu-id="13aeb-367">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="13aeb-368">![Ayrıntılar gözatma](aspnet-mvc-4-models-and-data-access/_static/image25.png "ayrıntıları gözatma")</span><span class="sxs-lookup"><span data-stu-id="13aeb-368">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="13aeb-369">*/Store/details/51 gözatma*</span><span class="sxs-lookup"><span data-stu-id="13aeb-369">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="13aeb-370">Ayrıca, Windows Azure Web siteleri aşağıdaki bu uygulamayı dağıtabilmek için [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="13aeb-370">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="13aeb-371">Özet</span><span class="sxs-lookup"><span data-stu-id="13aeb-371">Summary</span></span>

<span data-ttu-id="13aeb-372">ASP.NET MVC modelleri ve veri erişimi temelleri öğrenilen Bu uygulamalı Laboratuvar Tamamlanıyor, kullanarak **veritabanı ilk** yaklaşım yanı sıra **Code First** yaklaşım:</span><span class="sxs-lookup"><span data-stu-id="13aeb-372">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="13aeb-373">Bir veritabanı çözümü kendi veri tüketmek için ekleme</span><span class="sxs-lookup"><span data-stu-id="13aeb-373">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="13aeb-374">Görünüm şablonları sabit kodlanmış bir yerine veritabanından alınan veri sağlamak için denetleyicileri güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="13aeb-374">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="13aeb-375">Parametreleri kullanarak veritabanını sorgulama</span><span class="sxs-lookup"><span data-stu-id="13aeb-375">How to query the database using parameters</span></span>
- <span data-ttu-id="13aeb-376">Sorgu sonuç verileri daha verimli bir şekilde alma şekillendirme, veritabanı erişimlerine sayısını azaltan bir özellik kullanma</span><span class="sxs-lookup"><span data-stu-id="13aeb-376">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="13aeb-377">Veritabanı ilk ve Code First yaklaşımlar Microsoft Entity Framework modelini veritabanıyla bağlantı için nasıl kullanılacağını</span><span class="sxs-lookup"><span data-stu-id="13aeb-377">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="13aeb-378">Ek A: Yükleme Web Visual Studio Express 2012 için</span><span class="sxs-lookup"><span data-stu-id="13aeb-378">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="13aeb-379">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-379">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="13aeb-380">Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="13aeb-380">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="13aeb-381">Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="13aeb-381">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="13aeb-382">Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK'sı Web</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="13aeb-382">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="13aeb-383">Tıklayın **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-383">Click on **Install Now**.</span></span> <span data-ttu-id="13aeb-384">Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-384">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="13aeb-385">Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="13aeb-385">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="13aeb-386">![Visual Studio Express yükleme](aspnet-mvc-4-models-and-data-access/_static/image26.png "yükleme Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="13aeb-386">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="13aeb-387">*Visual Studio Express yükleme*</span><span class="sxs-lookup"><span data-stu-id="13aeb-387">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="13aeb-388">Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="13aeb-388">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşulları kabul ediliyor](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="13aeb-390">*Lisans koşulları kabul ediliyor*</span><span class="sxs-lookup"><span data-stu-id="13aeb-390">*Accepting the license terms*</span></span>
5. <span data-ttu-id="13aeb-391">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="13aeb-391">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="13aeb-393">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="13aeb-393">*Installation progress*</span></span>
6. <span data-ttu-id="13aeb-394">Yükleme tamamlandığında tıklatın **son**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-394">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="13aeb-396">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="13aeb-396">*Installation completed*</span></span>
7. <span data-ttu-id="13aeb-397">Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-397">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="13aeb-398">Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.</span><span class="sxs-lookup"><span data-stu-id="13aeb-398">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web döşemeye](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="13aeb-400">*VS Express Web döşemeye*</span><span class="sxs-lookup"><span data-stu-id="13aeb-400">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="13aeb-401">Ek B: Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="13aeb-401">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="13aeb-402">Bu ekte Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturmak ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlamak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-402">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="13aeb-403">Görev 1 - yeni bir Web sitesi oluşturma Windows Azure portalı</span><span class="sxs-lookup"><span data-stu-id="13aeb-403">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="13aeb-404">Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) ve aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-404">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="13aeb-405">Windows Azure ile 10 ASP.NET Web siteleri ücretsiz barındırma ve ardından trafiğiniz büyüdükçe ölçeğinizi.</span><span class="sxs-lookup"><span data-stu-id="13aeb-405">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="13aeb-406">Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="13aeb-406">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="13aeb-407">![Windows Azure portalında oturum açtığı](aspnet-mvc-4-models-and-data-access/_static/image31.png "Windows Azure Portal'da oturum açın")</span><span class="sxs-lookup"><span data-stu-id="13aeb-407">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="13aeb-408">*Windows Azure yönetim portalında oturum açtığı*</span><span class="sxs-lookup"><span data-stu-id="13aeb-408">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="13aeb-409">Tıklatın **yeni** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="13aeb-409">Click **New** on the command bar.</span></span>

    <span data-ttu-id="13aeb-410">![Yeni bir Web sitesi oluşturma](aspnet-mvc-4-models-and-data-access/_static/image32.png "yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="13aeb-410">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="13aeb-411">*Yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="13aeb-411">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="13aeb-412">Tıklatın **işlem** | **Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-412">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="13aeb-413">Ardından **hızlı Oluştur** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="13aeb-413">Then select **Quick Create** option.</span></span> <span data-ttu-id="13aeb-414">Yeni web sitesi için kullanılabilir bir URL girin ve tıklayın **Web sitesi oluştur**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-414">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="13aeb-415">Windows Azure Web sitesi, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-415">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="13aeb-416">Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portal dışındaki dağıtmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="13aeb-416">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="13aeb-417">Bir veritabanını ayarlamak için adımları içermez.</span><span class="sxs-lookup"><span data-stu-id="13aeb-417">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="13aeb-418">![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](aspnet-mvc-4-models-and-data-access/_static/image33.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="13aeb-418">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="13aeb-419">*Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="13aeb-419">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="13aeb-420">Yeni kadar bekleyin **Web sitesi** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="13aeb-420">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="13aeb-421">Web sitesi oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun.</span><span class="sxs-lookup"><span data-stu-id="13aeb-421">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="13aeb-422">Yeni Web sitesi çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="13aeb-422">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="13aeb-423">![Yeni web sitesi için gözatma](aspnet-mvc-4-models-and-data-access/_static/image34.png "yeni web sitesi için gözatma")</span><span class="sxs-lookup"><span data-stu-id="13aeb-423">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="13aeb-424">*Yeni web sitesi için gözatma*</span><span class="sxs-lookup"><span data-stu-id="13aeb-424">*Browsing to the new web site*</span></span>

    <span data-ttu-id="13aeb-425">![Çalışan Web sitesi](aspnet-mvc-4-models-and-data-access/_static/image35.png "çalışan Web sitesi")</span><span class="sxs-lookup"><span data-stu-id="13aeb-425">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="13aeb-426">*Çalışan Web sitesi*</span><span class="sxs-lookup"><span data-stu-id="13aeb-426">*Web site running*</span></span>
6. <span data-ttu-id="13aeb-427">Portalına geri dönün ve web sitesi altında adına tıklayın **adı** yönetim sayfaları görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="13aeb-427">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="13aeb-428">![Web sitesi Yönetim sayfalarının açma](aspnet-mvc-4-models-and-data-access/_static/image36.png "web sitesi Yönetim sayfalarının açma")</span><span class="sxs-lookup"><span data-stu-id="13aeb-428">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="13aeb-429">*Web Sitesi Yönetim sayfalarının açma*</span><span class="sxs-lookup"><span data-stu-id="13aeb-429">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="13aeb-430">İçinde **Pano** sayfasında **Hızlı Bakış** 'yi tıklatın **yayım profili indirin** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="13aeb-430">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="13aeb-431">*Yayımlama profili* her etkin yayımlama yöntemi için bir Windows Azure Web sitesi bir web uygulamasına yayımlamak için gereken bilgilerin tümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-431">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="13aeb-432">Yayımlama profili URL'leri, kullanıcı kimlik bilgilerini ve bağlanmak ve her bir yayımlama yönteminin etkinleştirildiği uç noktaları karşı kimlik doğrulaması için gerekli veritabanı dizelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-432">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="13aeb-433">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma destek yayımlamak için bu programlar yapılandırılmasını otomatikleştirmek için profilleri Windows Azure Web siteleri için Web uygulamaları yayımlama.</span><span class="sxs-lookup"><span data-stu-id="13aeb-433">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="13aeb-434">![Yayımlama profili web sitesi Yükleniyor](aspnet-mvc-4-models-and-data-access/_static/image37.png "yayımlama profili web sitesi yükleniyor")</span><span class="sxs-lookup"><span data-stu-id="13aeb-434">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="13aeb-435">*Yayımlama profili Web sitesi yükleniyor*</span><span class="sxs-lookup"><span data-stu-id="13aeb-435">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="13aeb-436">Yayımlama profili dosyasını bilinen bir konuma indirin.</span><span class="sxs-lookup"><span data-stu-id="13aeb-436">Download the publish profile file to a known location.</span></span> <span data-ttu-id="13aeb-437">Daha fazla Bu alıştırmada Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlamak için bu dosyayı kullanmak nasıl göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="13aeb-437">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="13aeb-438">![Yayımlama profili dosyasını kaydetme](aspnet-mvc-4-models-and-data-access/_static/image38.png "yayımlama profilini kaydetme")</span><span class="sxs-lookup"><span data-stu-id="13aeb-438">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="13aeb-439">*Yayımlama profili dosyasını kaydetme*</span><span class="sxs-lookup"><span data-stu-id="13aeb-439">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="13aeb-440">Görev 2 - veritabanı sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="13aeb-440">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="13aeb-441">Uygulamanızı SQL Server'ın kullanmak yaparsa veritabanlarının bir SQL veritabanı sunucusu oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-441">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="13aeb-442">SQL Server kullanmayan basit bir uygulamayı dağıtmak istiyorsanız, bu görevi atlamak.</span><span class="sxs-lookup"><span data-stu-id="13aeb-442">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="13aeb-443">Uygulama veritabanını depolamak için bir SQL veritabanı sunucusu gerekir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-443">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="13aeb-444">Aboneliğiniz Windows Azure Yönetim Portalı'nda SQL veritabanı sunucularının görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-444">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="13aeb-445">Oluşturulan bir sunucu yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğundan düğme.</span><span class="sxs-lookup"><span data-stu-id="13aeb-445">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="13aeb-446">Not edin **sunucu adı ve URL, yönetici oturum açma adı ve parola**sonraki görevleri kullanacağı gibi.</span><span class="sxs-lookup"><span data-stu-id="13aeb-446">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="13aeb-447">Daha sonraki bir aşamada oluşturulacak şekilde veritabanı henüz oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-447">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="13aeb-448">![SQL veritabanı sunucusu Pano](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL veritabanı sunucu Panosu")</span><span class="sxs-lookup"><span data-stu-id="13aeb-448">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="13aeb-449">*SQL veritabanı sunucu Panosu*</span><span class="sxs-lookup"><span data-stu-id="13aeb-449">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="13aeb-450">İşlemin sonraki görev, sunucunun listesinde, yerel IP adresi içermesi gereken bu nedenle veritabanı bağlantısı Visual Studio'dan test edecek **izin verilen IP adreslerini**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-450">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="13aeb-451">Bunu yapmak için tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** üzerinde yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) düğmesi.</span><span class="sxs-lookup"><span data-stu-id="13aeb-451">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![İstemci IP adresi ekleme](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="13aeb-453">*İstemci IP adresi ekleme*</span><span class="sxs-lookup"><span data-stu-id="13aeb-453">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="13aeb-454">Bir kez **istemci IP adresi** izin verilen IP adreslerine eklenen listesinde, tıklayın **kaydetmek** değişiklikleri onaylamak için.</span><span class="sxs-lookup"><span data-stu-id="13aeb-454">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Değişiklikleri onaylamak](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="13aeb-456">*Değişiklikleri onaylamak*</span><span class="sxs-lookup"><span data-stu-id="13aeb-456">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="13aeb-457">Görev 3 - Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="13aeb-457">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="13aeb-458">ASP.NET MVC 4 çözüme geri dönün.</span><span class="sxs-lookup"><span data-stu-id="13aeb-458">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="13aeb-459">İçinde **Çözüm Gezgini**, web sitesi projesine sağ tıklatın ve **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-459">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="13aeb-460">![Uygulama yayımlama](aspnet-mvc-4-models-and-data-access/_static/image43.png "uygulama yayımlama")</span><span class="sxs-lookup"><span data-stu-id="13aeb-460">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="13aeb-461">*Web sitesi yayımlama*</span><span class="sxs-lookup"><span data-stu-id="13aeb-461">*Publishing the web site*</span></span>
2. <span data-ttu-id="13aeb-462">İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-462">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="13aeb-463">![Yayımlama profilini içeri aktarma](aspnet-mvc-4-models-and-data-access/_static/image44.png "yayımlama profilini içeri aktarma")</span><span class="sxs-lookup"><span data-stu-id="13aeb-463">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="13aeb-464">*Yayımlama profilini içeri aktarma*</span><span class="sxs-lookup"><span data-stu-id="13aeb-464">*Importing publish profile*</span></span>
3. <span data-ttu-id="13aeb-465">Tıklatın **bağlantısı doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-465">Click **Validate Connection**.</span></span> <span data-ttu-id="13aeb-466">Doğrulama tamamlandıktan sonra tıklayın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-466">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="13aeb-467">Yanındaki bağlantıyı doğrula düğmesi görünür yeşil bir onay işareti gördüğünüzde doğrulama tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="13aeb-467">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="13aeb-468">![Bağlantı doğrulama](aspnet-mvc-4-models-and-data-access/_static/image45.png "bağlantısı doğrulanıyor")</span><span class="sxs-lookup"><span data-stu-id="13aeb-468">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="13aeb-469">*Doğrulama bağlantısı*</span><span class="sxs-lookup"><span data-stu-id="13aeb-469">*Validating connection*</span></span>
4. <span data-ttu-id="13aeb-470">İçinde **ayarları** sayfasında **veritabanları** bölümünde, veritabanı bağlantının textbox yanındaki düğmesini tıklatın (yani **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="13aeb-470">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="13aeb-471">![Web dağıtımı yapılandırma](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web dağıtımı yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="13aeb-471">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="13aeb-472">*Web dağıtımı yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="13aeb-472">*Web deploy configuration*</span></span>
5. <span data-ttu-id="13aeb-473">Veritabanı bağlantısı aşağıdaki gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="13aeb-473">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="13aeb-474">İçinde **sunucu adı** , SQL veritabanı sunucusu URL'yi kullanarak yazın *tcp:* öneki.</span><span class="sxs-lookup"><span data-stu-id="13aeb-474">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="13aeb-475">İçinde **kullanıcı adı** sunucunuzun yönetici oturum açma adını yazın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-475">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="13aeb-476">İçinde **parola** sunucu yönetici oturum açma parolasını yazın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-476">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="13aeb-477">Yeni bir veritabanı adı yazın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-477">Type a new database name.</span></span>

     <span data-ttu-id="13aeb-478">![Hedef bağlantı dizesi yapılandırma](aspnet-mvc-4-models-and-data-access/_static/image47.png "hedef bağlantı dizesi yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="13aeb-478">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="13aeb-479">*Hedef bağlantı dizesi yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="13aeb-479">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="13aeb-480">Sonra **Tamam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-480">Then click **OK**.</span></span> <span data-ttu-id="13aeb-481">Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-481">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="13aeb-482">![Veritabanı oluşturma](aspnet-mvc-4-models-and-data-access/_static/image48.png "veritabanı dizesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="13aeb-482">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="13aeb-483">*Veritabanı oluşturuluyor*</span><span class="sxs-lookup"><span data-stu-id="13aeb-483">*Creating the database*</span></span>
7. <span data-ttu-id="13aeb-484">Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini varsayılan bağlantı textbox içinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-484">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="13aeb-485">Sonra **İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-485">Then click **Next**.</span></span>

    <span data-ttu-id="13aeb-486">![SQL veritabanına işaret eden bağlantı dizesi](aspnet-mvc-4-models-and-data-access/_static/image49.png "SQL veritabanına işaret eden bağlantı dizesi")</span><span class="sxs-lookup"><span data-stu-id="13aeb-486">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="13aeb-487">*SQL veritabanına işaret eden bağlantı dizesi*</span><span class="sxs-lookup"><span data-stu-id="13aeb-487">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="13aeb-488">İçinde **Önizleme** sayfasında, **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-488">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="13aeb-489">![Web uygulaması yayımlama](aspnet-mvc-4-models-and-data-access/_static/image50.png "web uygulaması yayımlama")</span><span class="sxs-lookup"><span data-stu-id="13aeb-489">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="13aeb-490">*Web uygulaması yayımlama*</span><span class="sxs-lookup"><span data-stu-id="13aeb-490">*Publishing the web application*</span></span>
9. <span data-ttu-id="13aeb-491">Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesini açın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-491">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="13aeb-492">Ek C: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="13aeb-492">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="13aeb-493">Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip.</span><span class="sxs-lookup"><span data-stu-id="13aeb-493">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="13aeb-494">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="13aeb-494">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="13aeb-495">![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-models-and-data-access/_static/image51.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="13aeb-495">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="13aeb-496">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="13aeb-496">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="13aeb-497">***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="13aeb-497">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="13aeb-498">İmleci, burada kod eklemek istediğiniz yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="13aeb-498">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="13aeb-499">(Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-499">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="13aeb-500">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="13aeb-500">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="13aeb-501">Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="13aeb-501">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="13aeb-502">İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-502">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="13aeb-503">![Kod parçacığında adını yazmaya başlayın](aspnet-mvc-4-models-and-data-access/_static/image52.png "parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="13aeb-503">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="13aeb-504">*Kod parçacığında adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="13aeb-504">*Start typing the snippet name*</span></span>

<span data-ttu-id="13aeb-505">![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](aspnet-mvc-4-models-and-data-access/_static/image53.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="13aeb-505">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="13aeb-506">*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="13aeb-506">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="13aeb-507">![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](aspnet-mvc-4-models-and-data-access/_static/image54.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")</span><span class="sxs-lookup"><span data-stu-id="13aeb-507">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="13aeb-508">*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*</span><span class="sxs-lookup"><span data-stu-id="13aeb-508">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="13aeb-509">***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1.</span><span class="sxs-lookup"><span data-stu-id="13aeb-509">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="13aeb-510">Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="13aeb-510">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="13aeb-511">Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.</span><span class="sxs-lookup"><span data-stu-id="13aeb-511">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="13aeb-512">Tıklayarak ilgili kod parçacığında listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="13aeb-512">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="13aeb-513">![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](aspnet-mvc-4-models-and-data-access/_static/image55.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")</span><span class="sxs-lookup"><span data-stu-id="13aeb-513">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="13aeb-514">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*</span><span class="sxs-lookup"><span data-stu-id="13aeb-514">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="13aeb-515">![Tıklayarak ilgili kod parçacığında listeden çekme](aspnet-mvc-4-models-and-data-access/_static/image56.png "tıklayarak ilgili kod parçacığında listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="13aeb-515">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="13aeb-516">*Tıklayarak ilgili kod parçacığında listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="13aeb-516">*Pick the relevant snippet from the list, by clicking on it*</span></span>
