---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Bir tablo veritabanı verilerinin (C#) görüntüleyen | Microsoft Docs
author: microsoft
description: Bu öğreticide, ı veritabanı kayıt kümesini görüntülemek için iki yöntem gösterilmektedir. I HTML veritabanı kayıt kümesinin eri biçimlendirme için iki yöntem göster...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 1d5dc9dd4a82e4577c6c1a3b124d45fef0b0f67c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872890"
---
<a name="displaying-a-table-of-database-data-c"></a><span data-ttu-id="d9246-104">Bir tablo veritabanı verilerinin (C#) görüntüleme</span><span class="sxs-lookup"><span data-stu-id="d9246-104">Displaying a Table of Database Data (C#)</span></span>
====================
<span data-ttu-id="d9246-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d9246-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="d9246-106">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="d9246-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> <span data-ttu-id="d9246-107">Bu öğreticide, ı veritabanı kayıt kümesini görüntülemek için iki yöntem gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d9246-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="d9246-108">I veritabanı kayıtlarını HTML tablosunda bir dizi biçimlendirme için iki yöntem gösterir.</span><span class="sxs-lookup"><span data-stu-id="d9246-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="d9246-109">İlk olarak, doğrudan bir görünüm içindeki veritabanı kayıtlarını nasıl biçimlendirebilirsiniz ı gösterir.</span><span class="sxs-lookup"><span data-stu-id="d9246-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="d9246-110">Ardından, t nasıl, kısmi veritabanı kayıtlarını biçimlendirme sırasında yararlanabilir göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="d9246-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="d9246-111">Bu öğretici, bir ASP.NET MVC uygulamasındaki HTML tablosu veritabanı verilerinin nasıl görüntüleyebilirsiniz açıklamak için hedefidir.</span><span class="sxs-lookup"><span data-stu-id="d9246-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="d9246-112">İlk olarak, Visual Studio'daki yapı iskelesi araçları otomatik olarak bir kayıt kümesi görüntüleyen bir görünüm oluşturmak için nasıl kullanılacağını öğrenin.</span><span class="sxs-lookup"><span data-stu-id="d9246-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="d9246-113">Ardından, veritabanı kayıtlarını biçimlendirilirken bir kısmi bir şablon olarak kullanmak üzere nasıl öğrenin.</span><span class="sxs-lookup"><span data-stu-id="d9246-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="d9246-114">Model sınıfları oluşturma</span><span class="sxs-lookup"><span data-stu-id="d9246-114">Create the Model Classes</span></span>

<span data-ttu-id="d9246-115">Film veritabanı tablosundan kayıt kümesini görüntülemek olacak.</span><span class="sxs-lookup"><span data-stu-id="d9246-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="d9246-116">Film veritabanı tablo şu sütunları içerir:</span><span class="sxs-lookup"><span data-stu-id="d9246-116">The Movies database table contains the following columns:</span></span>

<a id="0.3_table01"></a>


| <span data-ttu-id="d9246-117">**Sütun adı**</span><span class="sxs-lookup"><span data-stu-id="d9246-117">**Column Name**</span></span> | <span data-ttu-id="d9246-118">**Veri türü**</span><span class="sxs-lookup"><span data-stu-id="d9246-118">**Data Type**</span></span> | <span data-ttu-id="d9246-119">**Null değerlere izin ver**</span><span class="sxs-lookup"><span data-stu-id="d9246-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d9246-120">Kimliği</span><span class="sxs-lookup"><span data-stu-id="d9246-120">Id</span></span> | <span data-ttu-id="d9246-121">int</span><span class="sxs-lookup"><span data-stu-id="d9246-121">Int</span></span> | <span data-ttu-id="d9246-122">False</span><span class="sxs-lookup"><span data-stu-id="d9246-122">False</span></span> |
| <span data-ttu-id="d9246-123">Başlık</span><span class="sxs-lookup"><span data-stu-id="d9246-123">Title</span></span> | <span data-ttu-id="d9246-124">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="d9246-124">Nvarchar(200)</span></span> | <span data-ttu-id="d9246-125">False</span><span class="sxs-lookup"><span data-stu-id="d9246-125">False</span></span> |
| <span data-ttu-id="d9246-126">Director</span><span class="sxs-lookup"><span data-stu-id="d9246-126">Director</span></span> | <span data-ttu-id="d9246-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="d9246-127">NVarchar(50)</span></span> | <span data-ttu-id="d9246-128">False</span><span class="sxs-lookup"><span data-stu-id="d9246-128">False</span></span> |
| <span data-ttu-id="d9246-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="d9246-129">DateReleased</span></span> | <span data-ttu-id="d9246-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="d9246-130">DateTime</span></span> | <span data-ttu-id="d9246-131">False</span><span class="sxs-lookup"><span data-stu-id="d9246-131">False</span></span> |


<span data-ttu-id="d9246-132">ASP.NET MVC uygulamamız filmler tabloda temsil etmek için size bir model sınıfı oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d9246-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="d9246-133">Bu öğreticide, bizim modeli sınıfları oluşturmak için Microsoft Entity Framework kullanın.</span><span class="sxs-lookup"><span data-stu-id="d9246-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d9246-134">Bu öğreticide, Microsoft Entity Framework kullanırız.</span><span class="sxs-lookup"><span data-stu-id="d9246-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="d9246-135">Ancak, bir veritabanından LINQ-SQL, NHibernate ya da ADO.NET dahil olmak üzere bir ASP.NET MVC uygulaması ile etkileşim kurmak için çeşitli farklı teknolojiler kullanabileceğinizi anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="d9246-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="d9246-136">Varlık veri modeli Sihirbazı'nı başlatmak için şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="d9246-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="d9246-137">Çözüm Gezgini penceresinin ve menü seçeneğini seçin modelleri klasörünü sağ tıklatın **Ekle, yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="d9246-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="d9246-138">Seçin **veri** kategori ve select **ADO.NET varlık veri modeli** şablonu.</span><span class="sxs-lookup"><span data-stu-id="d9246-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="d9246-139">Veri modelinizi adını verin *MoviesDBModel.edmx* tıklatıp **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d9246-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="d9246-140">Ekle düğmesine tıkladıktan sonra (bkz: Şekil 1) varlık veri modeli Sihirbazı görünür.</span><span class="sxs-lookup"><span data-stu-id="d9246-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="d9246-141">Sihirbazı tamamlamak için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="d9246-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="d9246-142">İçinde **Model içeriği seçin** adım, select **veritabanından Oluştur** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="d9246-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="d9246-143">İçinde **veri bağlantınızı** adım, kullanın *MoviesDB.mdf* veri bağlantısı ve ad *MoviesDBEntities* bağlantı ayarları.</span><span class="sxs-lookup"><span data-stu-id="d9246-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="d9246-144">Tıklatın **sonraki** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d9246-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="d9246-145">İçinde **veritabanı nesnelerinizi** adım, tablolar düğümünü genişletin, filmler tabloyu seçin.</span><span class="sxs-lookup"><span data-stu-id="d9246-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="d9246-146">Ad alanı girin *modelleri* tıklatıp **son** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d9246-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="d9246-147">[![LINQ SQL sınıfları için oluşturma](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d9246-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="d9246-148">**Şekil 01**: SQL sınıfları için oluşturma LINQ ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="d9246-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image2.png))</span></span>


<span data-ttu-id="d9246-149">Entity Data Model Designer, varlık veri modeli Sihirbazı tamamladıktan sonra açılır.</span><span class="sxs-lookup"><span data-stu-id="d9246-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="d9246-150">Tasarımcı filmler varlık görüntülenmelidir (bkz: Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="d9246-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="d9246-151">[![Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d9246-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span></span>

<span data-ttu-id="d9246-152">**Şekil 02**: Entity Data Model Designer ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="d9246-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image4.png))</span></span>


<span data-ttu-id="d9246-153">Biz devam etmeden önce bir değişiklik yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d9246-153">We need to make one change before we continue.</span></span> <span data-ttu-id="d9246-154">Varlık veri Sihirbazı adlı bir model sınıfı oluşturur *filmler* , filmler veritabanı tablosu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d9246-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="d9246-155">Film sınıfı belirli bir filmi temsil etmek için kullanacağız için olması için sınıf adını değiştirmek gerekiyor *film* yerine *filmler* (tekil yerine çoğul).</span><span class="sxs-lookup"><span data-stu-id="d9246-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="d9246-156">Sınıf Tasarımcısı yüzeyinde adına çift tıklayın ve film film sınıfın adını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d9246-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="d9246-157">Bu değişikliği yaptıktan sonra tıklatın **kaydetmek** film sınıfı oluşturmak için (disket simgesi) düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d9246-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="d9246-158">Film denetleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="d9246-158">Create the Movies Controller</span></span>

<span data-ttu-id="d9246-159">Veritabanı Kayıtlarımıza temsil etmek için bir yol sahibiz, filmler koleksiyonunu döndürür bir denetleyici oluşturabiliriz.</span><span class="sxs-lookup"><span data-stu-id="d9246-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="d9246-160">Visual Studio Çözüm Gezgini penceresi içinde denetleyicileri klasörünü sağ tıklatın ve menü seçeneğini **Ekle, denetleyici** (bkz: Şekil 3).</span><span class="sxs-lookup"><span data-stu-id="d9246-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="d9246-161">[![Denetleyici menü ekleme](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d9246-161">[![The Add Controller Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span></span>

<span data-ttu-id="d9246-162">**Şekil 03**: denetleyicisi menü Ekle ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d9246-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image6.png))</span></span>


<span data-ttu-id="d9246-163">Zaman **denetleyici Ekle** iletişim kutusu görüntülenirse, denetleyici adı MovieController girin (Şekil 4'e bakın).</span><span class="sxs-lookup"><span data-stu-id="d9246-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="d9246-164">Tıklatın **Ekle** düğmesi yeni denetleyici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d9246-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="d9246-165">[![Denetleyici Ekle iletişim kutusu](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="d9246-165">[![The Add Controller dialog](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span></span>

<span data-ttu-id="d9246-166">**Şekil 04**: Denetleyici Ekle iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="d9246-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image8.png))</span></span>


<span data-ttu-id="d9246-167">Biz, böylece veritabanı kayıtlarını kümesini döndürür film denetleyici tarafından kullanıma sunulan İNDİS() eylem değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d9246-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="d9246-168">Denetleyici denetleyicisi listeleme 1 gibi görünüyor şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d9246-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="d9246-169">**1 – Controllers\MovieController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="d9246-169">**Listing 1 – Controllers\MovieController.cs**</span></span>

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

<span data-ttu-id="d9246-170">Listeleme 1'de MoviesDBEntities sınıfı MoviesDB veritabanı göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d9246-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="d9246-171">Bu sınıf kullanmak için bu gibi MvcApplication1.Models ad alanı içe aktarmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="d9246-171">To use this class, you need to import the MvcApplication1.Models namespace like this:</span></span>

<span data-ttu-id="d9246-172">MvcApplication1.Models kullanarak;</span><span class="sxs-lookup"><span data-stu-id="d9246-172">using MvcApplication1.Models;</span></span>

<span data-ttu-id="d9246-173">İfade *varlıklar. MovieSet.ToList()* filmler veritabanı tablosundan tüm filmler kümesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="d9246-173">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="d9246-174">Görünüm Oluştur</span><span class="sxs-lookup"><span data-stu-id="d9246-174">Create the View</span></span>

<span data-ttu-id="d9246-175">Visual Studio tarafından sağlanan askılama özelliğinin avantajlarından yararlanmak için bir HTML tablosunda bir veritabanı kayıt kümesini görüntülemek için en kolay yolu olan.</span><span class="sxs-lookup"><span data-stu-id="d9246-175">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="d9246-176">Menü seçeneğini seçerek uygulamanızı **yapı, yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="d9246-176">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="d9246-177">Uygulamanızı açmadan önce oluşturmanız gerekir **Görünüm Ekle** iletişim ya da veri sınıfları iletişim kutusunda görünmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="d9246-177">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="d9246-178">İNDİS() eyleme sağ tıklayın ve menü seçeneğini **Görünüm Ekle** (bkz. Şekil 5).</span><span class="sxs-lookup"><span data-stu-id="d9246-178">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="d9246-179">[![Bir görünümü ekleme](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="d9246-179">[![Adding a view](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span></span>

<span data-ttu-id="d9246-180">**Şekil 05**: bir görünüm ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="d9246-180">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="d9246-181">İçinde **Görünüm Ekle** iletişim kutusunda, etiketli onay kutusunu işaretleyin **kesin türü belirtilmiş görünüm oluşturmak**.</span><span class="sxs-lookup"><span data-stu-id="d9246-181">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="d9246-182">Film sınıf olarak seçin **görüntülemek veri sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="d9246-182">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="d9246-183">Seçin *listesi* olarak **görüntülemek içerik** (bkz. Şekil 6).</span><span class="sxs-lookup"><span data-stu-id="d9246-183">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="d9246-184">Bu seçenekleri filmler listesini görüntüleyen kesin türü belirtilmiş bir görünüm oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d9246-184">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="d9246-185">[![Görünüm Ekle iletişim kutusu](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="d9246-185">[![The Add View dialog](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="d9246-186">**Şekil 06**: Görünüm Ekle iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="d9246-186">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image12.png))</span></span>


<span data-ttu-id="d9246-187">Tıklattıktan sonra **Ekle** düğmesi, listeleme 2 görünümünde otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d9246-187">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="d9246-188">Bu görünüm filmler toplulukta tekrarlama ve her bir filmi özelliklerini görüntülemek için gerekli kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="d9246-188">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="d9246-189">**Listing 2 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="d9246-189">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

<span data-ttu-id="d9246-190">Menü seçeneğini belirleyerek uygulamayı çalıştırabilirsiniz **hata ayıklama, hata ayıklamayı Başlat** (veya F5 tuşuna basmak).</span><span class="sxs-lookup"><span data-stu-id="d9246-190">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="d9246-191">Uygulamayı çalıştıran Internet Explorer başlatır.</span><span class="sxs-lookup"><span data-stu-id="d9246-191">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="d9246-192">Ardından /Movie URL'ye gidin, Şekil 7'deki sayfası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="d9246-192">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="d9246-193">[![Film tablosu](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="d9246-193">[![A table of movies](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span></span>

<span data-ttu-id="d9246-194">**Şekil 07**: filmler tablosu ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="d9246-194">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image14.png))</span></span>


<span data-ttu-id="d9246-195">Şekil 7'de veritabanı kayıtlarını kılavuzunun görünümünü hakkında hiçbir şey hoşlanmıyorsanız dizin görünümünün yalnızca değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9246-195">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="d9246-196">Örneğin, değiştirebileceğiniz *DateReleased* başlığına *yayımlanma* dizin görünümünün değiştirerek.</span><span class="sxs-lookup"><span data-stu-id="d9246-196">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="d9246-197">Kısmi ile bir şablon oluşturma</span><span class="sxs-lookup"><span data-stu-id="d9246-197">Create a Template with a Partial</span></span>

<span data-ttu-id="d9246-198">Bir görünümü çok karmaşık aldığında kısmi görünümü çiğnemekten başlatmak için iyi bir fikirdir.</span><span class="sxs-lookup"><span data-stu-id="d9246-198">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="d9246-199">Kısmi kullanarak kendi görünümlerinizi anlamak ve sürdürmek daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="d9246-199">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="d9246-200">Kısmi, oluşturacağız biz her film veritabanı kayıtlarını biçimlendirmek için bir şablon olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d9246-200">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="d9246-201">Kısmi oluşturmak için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="d9246-201">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="d9246-202">Views\Movie klasörünü sağ tıklatın ve menü seçeneğini **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="d9246-202">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="d9246-203">Etiketli onay kutusunu işaretleyin *(.ascx) kısmi görünüm oluşturma*.</span><span class="sxs-lookup"><span data-stu-id="d9246-203">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="d9246-204">Kısmi adını *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="d9246-204">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="d9246-205">Etiketli onay kutusunu işaretleyin **kesin türü belirtilmiş görünüm oluşturmak**.</span><span class="sxs-lookup"><span data-stu-id="d9246-205">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="d9246-206">Film olarak seçin *görüntülemek veri sınıfı*.</span><span class="sxs-lookup"><span data-stu-id="d9246-206">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="d9246-207">Boş olarak işaretleyin *görüntülemek içerik*.</span><span class="sxs-lookup"><span data-stu-id="d9246-207">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="d9246-208">Tıklatın **Ekle** düğmesi kısmi projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d9246-208">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="d9246-209">Bu adımları tamamladıktan sonra listeleme 3 gibi aramak için kısmi MovieTemplate değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d9246-209">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="d9246-210">**3 – Views\Movie\MovieTemplate.ascx listeleme**</span><span class="sxs-lookup"><span data-stu-id="d9246-210">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

<span data-ttu-id="d9246-211">Kısmi listeleme 3'te kayıt tek bir satır için bir şablonu içerir.</span><span class="sxs-lookup"><span data-stu-id="d9246-211">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="d9246-212">Listeleme 4 değiştirilmiş dizin görünümünde kısmi MovieTemplate kullanır.</span><span class="sxs-lookup"><span data-stu-id="d9246-212">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="d9246-213">**Listing 4 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="d9246-213">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

<span data-ttu-id="d9246-214">Listeleme 4 görünümünde tüm filmler tekrarlanan bir foreach döngüsü içerir.</span><span class="sxs-lookup"><span data-stu-id="d9246-214">The view in Listing 4 contains a foreach loop that iterates through all of the movies.</span></span> <span data-ttu-id="d9246-215">Her film için kısmi MovieTemplate film biçimlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d9246-215">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="d9246-216">MovieTemplate RenderPartial() yardımcı yöntemini çağırarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="d9246-216">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="d9246-217">Veritabanı kayıtlarını çok aynı HTML tablosu değiştirilmiş Index görünümünü işler.</span><span class="sxs-lookup"><span data-stu-id="d9246-217">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="d9246-218">Ancak, görünümü önemli ölçüde basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d9246-218">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="d9246-219">Bir dize döndürmediğinden diğer yardımcı yöntemlerinin bir çoğunu RenderPartial() yöntemi farklıdır.</span><span class="sxs-lookup"><span data-stu-id="d9246-219">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="d9246-220">Bu nedenle, RenderPartial() yöntemini kullanarak çağırmalısınız &lt;% Html.RenderPartial(); %&gt; yerine &lt;% Html.RenderPartial(); = %&gt;.</span><span class="sxs-lookup"><span data-stu-id="d9246-220">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial(); %&gt; instead of &lt;%= Html.RenderPartial(); %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="d9246-221">Özet</span><span class="sxs-lookup"><span data-stu-id="d9246-221">Summary</span></span>

<span data-ttu-id="d9246-222">Bu öğreticinin amacı, bir HTML tablosunda veritabanı kayıt kümesinin nasıl görüntüleyebilirsiniz açıklamak için oluştu.</span><span class="sxs-lookup"><span data-stu-id="d9246-222">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="d9246-223">İlk olarak, Microsoft Entity Framework yararlanarak bir denetleyici eylemi bir veritabanı kayıt kümesi döndürmek nasıl öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="d9246-223">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="d9246-224">Ardından, Visual Studio yapı iskelesi öğeleri koleksiyonu otomatik olarak görüntüleyen bir görünüm oluşturmak için nasıl kullanılacağı hakkında bilgi edindiniz.</span><span class="sxs-lookup"><span data-stu-id="d9246-224">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="d9246-225">Son olarak, kısmi yararlanarak görünümü basitleştirmek nasıl öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="d9246-225">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="d9246-226">Kısmi bir şablon olarak kullanabilir, böylece her veritabanı kaydı biçimlendirebilirsiniz öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="d9246-226">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d9246-227">[Önceki](creating-model-classes-with-linq-to-sql-cs.md)
> [sonraki](performing-simple-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="d9246-227">[Previous](creating-model-classes-with-linq-to-sql-cs.md)
[Next](performing-simple-validation-cs.md)</span></span>
