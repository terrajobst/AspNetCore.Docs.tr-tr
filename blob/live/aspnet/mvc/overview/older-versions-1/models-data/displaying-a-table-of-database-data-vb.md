---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: "Bir tablo veritabanı verilerinin (VB) görüntüleyen | Microsoft Docs"
author: microsoft
description: "Bu öğreticide, ı veritabanı kayıt kümesini görüntülemek için iki yöntem gösterilmektedir. I HTML veritabanı kayıt kümesinin eri biçimlendirme için iki yöntem göster..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 6dc0aa91cfb68d308ed098f429d3251d424ab778
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="displaying-a-table-of-database-data-vb"></a><span data-ttu-id="b9a36-104">Bir tablo veritabanı verilerinin (VB) görüntüleme</span><span class="sxs-lookup"><span data-stu-id="b9a36-104">Displaying a Table of Database Data (VB)</span></span>
====================
<span data-ttu-id="b9a36-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b9a36-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="b9a36-106">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="b9a36-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> <span data-ttu-id="b9a36-107">Bu öğreticide, ı veritabanı kayıt kümesini görüntülemek için iki yöntem gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="b9a36-108">I veritabanı kayıtlarını HTML tablosunda bir dizi biçimlendirme için iki yöntem gösterir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="b9a36-109">İlk olarak, doğrudan bir görünüm içindeki veritabanı kayıtlarını nasıl biçimlendirebilirsiniz ı gösterir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="b9a36-110">Ardından, t nasıl, kısmi veritabanı kayıtlarını biçimlendirme sırasında yararlanabilir göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="b9a36-111">Bu öğretici, bir ASP.NET MVC uygulamasındaki HTML tablosu veritabanı verilerinin nasıl görüntüleyebilirsiniz açıklamak için hedefidir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="b9a36-112">İlk olarak, Visual Studio'daki yapı iskelesi araçları otomatik olarak bir kayıt kümesi görüntüleyen bir görünüm oluşturmak için nasıl kullanılacağını öğrenin.</span><span class="sxs-lookup"><span data-stu-id="b9a36-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="b9a36-113">Ardından, veritabanı kayıtlarını biçimlendirilirken bir kısmi bir şablon olarak kullanmak üzere nasıl öğrenin.</span><span class="sxs-lookup"><span data-stu-id="b9a36-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="b9a36-114">Model sınıfları oluşturma</span><span class="sxs-lookup"><span data-stu-id="b9a36-114">Create the Model Classes</span></span>

<span data-ttu-id="b9a36-115">Film veritabanı tablosundan kayıt kümesini görüntülemek olacak.</span><span class="sxs-lookup"><span data-stu-id="b9a36-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="b9a36-116">Film veritabanı tablo şu sütunları içerir:</span><span class="sxs-lookup"><span data-stu-id="b9a36-116">The Movies database table contains the following columns:</span></span>

<a id="0.4_table01"></a>


| <span data-ttu-id="b9a36-117">**Sütun adı**</span><span class="sxs-lookup"><span data-stu-id="b9a36-117">**Column Name**</span></span> | <span data-ttu-id="b9a36-118">**Veri türü**</span><span class="sxs-lookup"><span data-stu-id="b9a36-118">**Data Type**</span></span> | <span data-ttu-id="b9a36-119">**Null değerlere izin ver**</span><span class="sxs-lookup"><span data-stu-id="b9a36-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b9a36-120">Kimliği</span><span class="sxs-lookup"><span data-stu-id="b9a36-120">Id</span></span> | <span data-ttu-id="b9a36-121">int</span><span class="sxs-lookup"><span data-stu-id="b9a36-121">Int</span></span> | <span data-ttu-id="b9a36-122">False</span><span class="sxs-lookup"><span data-stu-id="b9a36-122">False</span></span> |
| <span data-ttu-id="b9a36-123">Başlık</span><span class="sxs-lookup"><span data-stu-id="b9a36-123">Title</span></span> | <span data-ttu-id="b9a36-124">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="b9a36-124">Nvarchar(200)</span></span> | <span data-ttu-id="b9a36-125">False</span><span class="sxs-lookup"><span data-stu-id="b9a36-125">False</span></span> |
| <span data-ttu-id="b9a36-126">Director</span><span class="sxs-lookup"><span data-stu-id="b9a36-126">Director</span></span> | <span data-ttu-id="b9a36-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="b9a36-127">NVarchar(50)</span></span> | <span data-ttu-id="b9a36-128">False</span><span class="sxs-lookup"><span data-stu-id="b9a36-128">False</span></span> |
| <span data-ttu-id="b9a36-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="b9a36-129">DateReleased</span></span> | <span data-ttu-id="b9a36-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="b9a36-130">DateTime</span></span> | <span data-ttu-id="b9a36-131">False</span><span class="sxs-lookup"><span data-stu-id="b9a36-131">False</span></span> |


<span data-ttu-id="b9a36-132">ASP.NET MVC uygulamamız filmler tabloda temsil etmek için size bir model sınıfı oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="b9a36-133">Bu öğreticide, bizim modeli sınıfları oluşturmak için Microsoft Entity Framework kullanın.</span><span class="sxs-lookup"><span data-stu-id="b9a36-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b9a36-134">Bu öğreticide, Microsoft Entity Framework kullanırız.</span><span class="sxs-lookup"><span data-stu-id="b9a36-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="b9a36-135">Ancak, bir veritabanından LINQ-SQL, NHibernate ya da ADO.NET dahil olmak üzere bir ASP.NET MVC uygulaması ile etkileşim kurmak için çeşitli farklı teknolojiler kullanabileceğinizi anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="b9a36-136">Varlık veri modeli Sihirbazı'nı başlatmak için şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="b9a36-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="b9a36-137">Çözüm Gezgini penceresinin ve menü seçeneğini seçin modelleri klasörünü sağ tıklatın **Ekle, yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="b9a36-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="b9a36-138">Seçin **veri** kategori ve select **ADO.NET varlık veri modeli** şablonu.</span><span class="sxs-lookup"><span data-stu-id="b9a36-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="b9a36-139">Veri modelinizi adını verin *MoviesDBModel.edmx* tıklatıp **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="b9a36-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="b9a36-140">Ekle düğmesine tıkladıktan sonra (bkz: Şekil 1) varlık veri modeli Sihirbazı görünür.</span><span class="sxs-lookup"><span data-stu-id="b9a36-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="b9a36-141">Sihirbazı tamamlamak için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="b9a36-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="b9a36-142">İçinde **Model içeriği seçin** adım, select **veritabanından Oluştur** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="b9a36-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="b9a36-143">İçinde **veri bağlantınızı** adım, kullanın *MoviesDB.mdf* veri bağlantısı ve ad *MoviesDBEntities* bağlantı ayarları.</span><span class="sxs-lookup"><span data-stu-id="b9a36-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="b9a36-144">Tıklatın **sonraki** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="b9a36-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="b9a36-145">İçinde **veritabanı nesnelerinizi** adım, tablolar düğümünü genişletin, filmler tabloyu seçin.</span><span class="sxs-lookup"><span data-stu-id="b9a36-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="b9a36-146">Ad alanı girin *modelleri* tıklatıp **son** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="b9a36-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="b9a36-147">[![LINQ SQL sınıfları için oluşturma](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b9a36-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span></span>

<span data-ttu-id="b9a36-148">**Şekil 01**: SQL sınıfları için oluşturma LINQ ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b9a36-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image2.png))</span></span>


<span data-ttu-id="b9a36-149">Entity Data Model Designer, varlık veri modeli Sihirbazı tamamladıktan sonra açılır.</span><span class="sxs-lookup"><span data-stu-id="b9a36-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="b9a36-150">Tasarımcı filmler varlık görüntülenmelidir (bkz: Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="b9a36-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="b9a36-151">[![Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b9a36-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span></span>

<span data-ttu-id="b9a36-152">**Şekil 02**: Entity Data Model Designer ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="b9a36-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image4.png))</span></span>


<span data-ttu-id="b9a36-153">Biz devam etmeden önce bir değişiklik yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-153">We need to make one change before we continue.</span></span> <span data-ttu-id="b9a36-154">Varlık veri Sihirbazı adlı bir model sınıfı oluşturur *filmler* , filmler veritabanı tablosu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="b9a36-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="b9a36-155">Film sınıfı belirli bir filmi temsil etmek için kullanacağız için olması için sınıf adını değiştirmek gerekiyor *film* yerine *filmler* (tekil yerine çoğul).</span><span class="sxs-lookup"><span data-stu-id="b9a36-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="b9a36-156">Sınıf Tasarımcısı yüzeyinde adına çift tıklayın ve film film sınıfın adını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b9a36-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="b9a36-157">Bu değişikliği yaptıktan sonra tıklatın **kaydetmek** film sınıfı oluşturmak için (disket simgesi) düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b9a36-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="b9a36-158">Film denetleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="b9a36-158">Create the Movies Controller</span></span>

<span data-ttu-id="b9a36-159">Veritabanı Kayıtlarımıza temsil etmek için bir yol sahibiz, filmler koleksiyonunu döndürür bir denetleyici oluşturabiliriz.</span><span class="sxs-lookup"><span data-stu-id="b9a36-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="b9a36-160">Visual Studio Çözüm Gezgini penceresi içinde denetleyicileri klasörünü sağ tıklatın ve menü seçeneğini **Ekle, denetleyici** (bkz: Şekil 3).</span><span class="sxs-lookup"><span data-stu-id="b9a36-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="b9a36-161">[![Denetleyici menü ekleme](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="b9a36-161">[![The Add Controller Menu](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span></span>

<span data-ttu-id="b9a36-162">**Şekil 03**: denetleyicisi menü Ekle ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b9a36-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image6.png))</span></span>


<span data-ttu-id="b9a36-163">Zaman **denetleyici Ekle** iletişim kutusu görüntülenirse, denetleyici adı MovieController girin (Şekil 4'e bakın).</span><span class="sxs-lookup"><span data-stu-id="b9a36-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="b9a36-164">Tıklatın **Ekle** düğmesi yeni denetleyici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b9a36-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="b9a36-165">[![Denetleyici Ekle iletişim kutusu](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="b9a36-165">[![The Add Controller dialog](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span></span>

<span data-ttu-id="b9a36-166">**Şekil 04**: Denetleyici Ekle iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="b9a36-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image8.png))</span></span>


<span data-ttu-id="b9a36-167">Biz, böylece veritabanı kayıtlarını kümesini döndürür film denetleyici tarafından kullanıma sunulan İNDİS() eylem değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="b9a36-168">Denetleyici denetleyicisi listeleme 1 gibi görünüyor şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b9a36-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="b9a36-169">**1 – Controllers\MovieController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="b9a36-169">**Listing 1 – Controllers\MovieController.vb**</span></span>

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

<span data-ttu-id="b9a36-170">Listeleme 1'de MoviesDBEntities sınıfı MoviesDB veritabanı göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b9a36-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="b9a36-171">İfade *varlıklar. MovieSet.ToList()* filmler veritabanı tablosundan tüm filmler kümesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="b9a36-171">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="b9a36-172">Görünüm Oluştur</span><span class="sxs-lookup"><span data-stu-id="b9a36-172">Create the View</span></span>

<span data-ttu-id="b9a36-173">Visual Studio tarafından sağlanan askılama özelliğinin avantajlarından yararlanmak için bir HTML tablosunda bir veritabanı kayıt kümesini görüntülemek için en kolay yolu olan.</span><span class="sxs-lookup"><span data-stu-id="b9a36-173">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="b9a36-174">Menü seçeneğini seçerek uygulamanızı **yapı, yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="b9a36-174">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="b9a36-175">Uygulamanızı açmadan önce oluşturmanız gerekir **Görünüm Ekle** iletişim ya da veri sınıfları iletişim kutusunda görünmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-175">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="b9a36-176">İNDİS() eyleme sağ tıklayın ve menü seçeneğini **Görünüm Ekle** (bkz. Şekil 5).</span><span class="sxs-lookup"><span data-stu-id="b9a36-176">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="b9a36-177">[![Bir görünümü ekleme](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="b9a36-177">[![Adding a view](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span></span>

<span data-ttu-id="b9a36-178">**Şekil 05**: bir görünüm ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="b9a36-178">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image10.png))</span></span>


<span data-ttu-id="b9a36-179">İçinde **Görünüm Ekle** iletişim kutusunda, etiketli onay kutusunu işaretleyin **kesin türü belirtilmiş görünüm oluşturmak**.</span><span class="sxs-lookup"><span data-stu-id="b9a36-179">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="b9a36-180">Film sınıf olarak seçin **görüntülemek veri sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="b9a36-180">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="b9a36-181">Seçin *listesi* olarak **görüntülemek içerik** (bkz. Şekil 6).</span><span class="sxs-lookup"><span data-stu-id="b9a36-181">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="b9a36-182">Bu seçenekleri filmler listesini görüntüleyen kesin türü belirtilmiş bir görünüm oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b9a36-182">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="b9a36-183">[![Görünüm Ekle iletişim kutusu](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="b9a36-183">[![The Add View dialog](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span></span>

<span data-ttu-id="b9a36-184">**Şekil 06**: Görünüm Ekle iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="b9a36-184">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image12.png))</span></span>


<span data-ttu-id="b9a36-185">Tıklattıktan sonra **Ekle** düğmesi, listeleme 2 görünümünde otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b9a36-185">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="b9a36-186">Bu görünüm filmler toplulukta tekrarlama ve her bir filmi özelliklerini görüntülemek için gerekli kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-186">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="b9a36-187">**2 – Views\Movie\Index.aspx listeleme**</span><span class="sxs-lookup"><span data-stu-id="b9a36-187">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

<span data-ttu-id="b9a36-188">Menü seçeneğini belirleyerek uygulamayı çalıştırabilirsiniz **hata ayıklama, hata ayıklamayı Başlat** (veya F5 tuşuna basmak).</span><span class="sxs-lookup"><span data-stu-id="b9a36-188">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="b9a36-189">Uygulamayı çalıştıran Internet Explorer başlatır.</span><span class="sxs-lookup"><span data-stu-id="b9a36-189">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="b9a36-190">Ardından /Movie URL'ye gidin, Şekil 7'deki sayfası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="b9a36-190">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="b9a36-191">[![Film tablosu](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="b9a36-191">[![A table of movies](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span></span>

<span data-ttu-id="b9a36-192">**Şekil 07**: filmler tablosu ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="b9a36-192">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image14.png))</span></span>


<span data-ttu-id="b9a36-193">Şekil 7'de veritabanı kayıtlarını kılavuzunun görünümünü hakkında hiçbir şey hoşlanmıyorsanız dizin görünümünün yalnızca değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b9a36-193">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="b9a36-194">Örneğin, değiştirebileceğiniz *DateReleased* başlığına *yayımlanma* dizin görünümünün değiştirerek.</span><span class="sxs-lookup"><span data-stu-id="b9a36-194">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="b9a36-195">Kısmi ile bir şablon oluşturma</span><span class="sxs-lookup"><span data-stu-id="b9a36-195">Create a Template with a Partial</span></span>

<span data-ttu-id="b9a36-196">Bir görünümü çok karmaşık aldığında kısmi görünümü çiğnemekten başlatmak için iyi bir fikirdir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-196">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="b9a36-197">Kısmi kullanarak kendi görünümlerinizi anlamak ve sürdürmek daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-197">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="b9a36-198">Kısmi, oluşturacağız biz her film veritabanı kayıtlarını biçimlendirmek için bir şablon olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b9a36-198">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="b9a36-199">Kısmi oluşturmak için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="b9a36-199">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="b9a36-200">Views\Movie klasörünü sağ tıklatın ve menü seçeneğini **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="b9a36-200">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="b9a36-201">Etiketli onay kutusunu işaretleyin *(.ascx) kısmi görünüm oluşturma*.</span><span class="sxs-lookup"><span data-stu-id="b9a36-201">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="b9a36-202">Kısmi adını *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="b9a36-202">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="b9a36-203">Etiketli onay kutusunu işaretleyin **kesin türü belirtilmiş görünüm oluşturmak**.</span><span class="sxs-lookup"><span data-stu-id="b9a36-203">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="b9a36-204">Film olarak seçin *görüntülemek veri sınıfı*.</span><span class="sxs-lookup"><span data-stu-id="b9a36-204">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="b9a36-205">Boş olarak işaretleyin *görüntülemek içerik*.</span><span class="sxs-lookup"><span data-stu-id="b9a36-205">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="b9a36-206">Tıklatın **Ekle** düğmesi kısmi projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b9a36-206">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="b9a36-207">Bu adımları tamamladıktan sonra listeleme 3 gibi aramak için kısmi MovieTemplate değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b9a36-207">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="b9a36-208">**3 – Views\Movie\MovieTemplate.ascx listeleme**</span><span class="sxs-lookup"><span data-stu-id="b9a36-208">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

<span data-ttu-id="b9a36-209">Kısmi listeleme 3'te kayıt tek bir satır için bir şablonu içerir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-209">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="b9a36-210">Listeleme 4 değiştirilmiş dizin görünümünde kısmi MovieTemplate kullanır.</span><span class="sxs-lookup"><span data-stu-id="b9a36-210">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="b9a36-211">**4 – Views\Movie\Index.aspx listeleme**</span><span class="sxs-lookup"><span data-stu-id="b9a36-211">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

<span data-ttu-id="b9a36-212">For listeleme 4 görünümünde içeren tüm filmler tekrarlanan her bir döngü.</span><span class="sxs-lookup"><span data-stu-id="b9a36-212">The view in Listing 4 contains a For Each loop that iterates through all of the movies.</span></span> <span data-ttu-id="b9a36-213">Her film için kısmi MovieTemplate film biçimlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b9a36-213">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="b9a36-214">MovieTemplate RenderPartial() yardımcı yöntemini çağırarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-214">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="b9a36-215">Veritabanı kayıtlarını çok aynı HTML tablosu değiştirilmiş Index görünümünü işler.</span><span class="sxs-lookup"><span data-stu-id="b9a36-215">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="b9a36-216">Ancak, görünümü önemli ölçüde basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b9a36-216">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="b9a36-217">Bir dize döndürmediğinden diğer yardımcı yöntemlerinin bir çoğunu RenderPartial() yöntemi farklıdır.</span><span class="sxs-lookup"><span data-stu-id="b9a36-217">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="b9a36-218">Bu nedenle, RenderPartial() yöntemini kullanarak çağırmalısınız &lt;Html.RenderPartial() %&gt; yerine &lt;% = Html.RenderPartial() %&gt;.</span><span class="sxs-lookup"><span data-stu-id="b9a36-218">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial() %&gt; instead of &lt;%= Html.RenderPartial() %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="b9a36-219">Özet</span><span class="sxs-lookup"><span data-stu-id="b9a36-219">Summary</span></span>

<span data-ttu-id="b9a36-220">Bu öğreticinin amacı, bir HTML tablosunda veritabanı kayıt kümesinin nasıl görüntüleyebilirsiniz açıklamak için oluştu.</span><span class="sxs-lookup"><span data-stu-id="b9a36-220">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="b9a36-221">İlk olarak, Microsoft Entity Framework yararlanarak bir denetleyici eylemi bir veritabanı kayıt kümesi döndürmek nasıl öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="b9a36-221">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="b9a36-222">Ardından, Visual Studio yapı iskelesi öğeleri koleksiyonu otomatik olarak görüntüleyen bir görünüm oluşturmak için nasıl kullanılacağı hakkında bilgi edindiniz.</span><span class="sxs-lookup"><span data-stu-id="b9a36-222">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="b9a36-223">Son olarak, kısmi yararlanarak görünümü basitleştirmek nasıl öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="b9a36-223">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="b9a36-224">Kısmi bir şablon olarak kullanabilir, böylece her veritabanı kaydı biçimlendirebilirsiniz öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="b9a36-224">You learned how to use a partial as a template so that you can format each database record.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b9a36-225">[Önceki](creating-model-classes-with-linq-to-sql-vb.md)
[sonraki](performing-simple-validation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b9a36-225">[Previous](creating-model-classes-with-linq-to-sql-vb.md)
[Next](performing-simple-validation-vb.md)</span></span>
