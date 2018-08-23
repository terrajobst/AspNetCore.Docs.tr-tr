---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: (C#) veritabanı verilerinin tablosunu görüntüleme | Microsoft Docs
author: microsoft
description: Bu öğreticide, ı veritabanı kayıt kümesini görüntülemek için iki yöntem göstermektedir. Ben bir HTML veritabanı kayıt kümesini veri biçimlendirme için iki yöntem göster...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: d0d3f6a574a4b923d5da73ccb2ab3bfbd6f305ef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754431"
---
<a name="displaying-a-table-of-database-data-c"></a><span data-ttu-id="b5390-104">(C#) veritabanı verilerinin tablosunu görüntüleme</span><span class="sxs-lookup"><span data-stu-id="b5390-104">Displaying a Table of Database Data (C#)</span></span>
====================
<span data-ttu-id="b5390-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b5390-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="b5390-106">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="b5390-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> <span data-ttu-id="b5390-107">Bu öğreticide, ı veritabanı kayıt kümesini görüntülemek için iki yöntem göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="b5390-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="b5390-108">Ben bir HTML tablosu veritabanı kayıtları bir dizi biçimlendirme için iki yöntem gösterir.</span><span class="sxs-lookup"><span data-stu-id="b5390-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="b5390-109">İlk olarak, doğrudan bir görünüm içindeki veritabanı kayıtlarını nasıl biçimlendirebilirsiniz ı gösterir.</span><span class="sxs-lookup"><span data-stu-id="b5390-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="b5390-110">Ardından, ben nasıl veritabanı kayıtlarını biçimlendirilirken kısmi avantajlarından yararlanabilirsiniz gösterir.</span><span class="sxs-lookup"><span data-stu-id="b5390-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="b5390-111">Bu öğreticide bir ASP.NET MVC uygulamasındaki HTML tablosu veritabanı verilerinin nasıl görüntüleyebilir açıklamak için hedefidir.</span><span class="sxs-lookup"><span data-stu-id="b5390-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="b5390-112">İlk olarak, bir kayıt kümesi otomatik olarak görüntüleyen bir görünüm oluşturmak için Visual Studio'ya dahil olan yapı iskelesi araçları kullanmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="b5390-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="b5390-113">Ardından, kısmi bir şablon olarak veritabanı kayıtlarını biçimlendirilirken kullanmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="b5390-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="b5390-114">Model sınıfları oluşturma</span><span class="sxs-lookup"><span data-stu-id="b5390-114">Create the Model Classes</span></span>

<span data-ttu-id="b5390-115">Film veritabanı tablosundan kayıt kümesini görüntülemek için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="b5390-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="b5390-116">Film veritabanı tablo şu sütunları içerir:</span><span class="sxs-lookup"><span data-stu-id="b5390-116">The Movies database table contains the following columns:</span></span>

<a id="0.3_table01"></a>


| <span data-ttu-id="b5390-117">**Sütun adı**</span><span class="sxs-lookup"><span data-stu-id="b5390-117">**Column Name**</span></span> | <span data-ttu-id="b5390-118">**Veri türü**</span><span class="sxs-lookup"><span data-stu-id="b5390-118">**Data Type**</span></span> | <span data-ttu-id="b5390-119">**Null değerlere izin ver**</span><span class="sxs-lookup"><span data-stu-id="b5390-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b5390-120">Kimliği</span><span class="sxs-lookup"><span data-stu-id="b5390-120">Id</span></span> | <span data-ttu-id="b5390-121">int</span><span class="sxs-lookup"><span data-stu-id="b5390-121">Int</span></span> | <span data-ttu-id="b5390-122">False</span><span class="sxs-lookup"><span data-stu-id="b5390-122">False</span></span> |
| <span data-ttu-id="b5390-123">Başlık</span><span class="sxs-lookup"><span data-stu-id="b5390-123">Title</span></span> | <span data-ttu-id="b5390-124">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="b5390-124">Nvarchar(200)</span></span> | <span data-ttu-id="b5390-125">False</span><span class="sxs-lookup"><span data-stu-id="b5390-125">False</span></span> |
| <span data-ttu-id="b5390-126">Direktörü</span><span class="sxs-lookup"><span data-stu-id="b5390-126">Director</span></span> | <span data-ttu-id="b5390-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="b5390-127">NVarchar(50)</span></span> | <span data-ttu-id="b5390-128">False</span><span class="sxs-lookup"><span data-stu-id="b5390-128">False</span></span> |
| <span data-ttu-id="b5390-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="b5390-129">DateReleased</span></span> | <span data-ttu-id="b5390-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="b5390-130">DateTime</span></span> | <span data-ttu-id="b5390-131">False</span><span class="sxs-lookup"><span data-stu-id="b5390-131">False</span></span> |


<span data-ttu-id="b5390-132">ASP.NET MVC uygulamamıza filmler tablosunda göstermek için bir model sınıfı oluşturmak ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="b5390-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="b5390-133">Bu öğreticide, bizim model sınıfları oluşturmak için Microsoft Entity Framework kullanın.</span><span class="sxs-lookup"><span data-stu-id="b5390-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b5390-134">Bu öğreticide, Microsoft Entity Framework kullanırız.</span><span class="sxs-lookup"><span data-stu-id="b5390-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="b5390-135">Ancak, bir veritabanından LINQ to SQL veya NHibernate ADO.NET dahil olmak üzere bir ASP.NET MVC uygulaması ile etkileşim kurmak için çeşitli farklı teknolojiler kullanabileceğinizi anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="b5390-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="b5390-136">Varlık veri modeli Sihirbazı başlatmak için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="b5390-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="b5390-137">Çözüm Gezgini penceresinde ve menü seçeneğini seçin modelleri klasörünü sağ tıklatın **Ekle, yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="b5390-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="b5390-138">Seçin **veri** kategori seçip alt **ADO.NET varlık veri modeli** şablonu.</span><span class="sxs-lookup"><span data-stu-id="b5390-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="b5390-139">Veri modelinizi adını verin *MoviesDBModel.edmx* tıklatıp **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="b5390-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="b5390-140">Ekle düğmesine tıkladıktan sonra (bkz. Şekil 1) varlık veri modeli Sihirbazı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b5390-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="b5390-141">Sihirbazı tamamlamak için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="b5390-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="b5390-142">İçinde **Choose Model Contents** adım, select **veritabanından Oluştur** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="b5390-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="b5390-143">İçinde **veri bağlantınızı seçin** adım, kullanın *MoviesDB.mdf* veri bağlantısı ve ad *MoviesDBEntities* bağlantı ayarları için.</span><span class="sxs-lookup"><span data-stu-id="b5390-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="b5390-144">Tıklayın **sonraki** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="b5390-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="b5390-145">İçinde **veritabanı nesnelerinizi seçin** adım, tablolar düğümünü genişletin, filmler tabloyu seçin.</span><span class="sxs-lookup"><span data-stu-id="b5390-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="b5390-146">Ad alanı girin *modelleri* tıklatıp **son** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="b5390-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="b5390-147">[![SQL sınıflarına LINQ oluşturma](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b5390-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="b5390-148">**Şekil 01**: LINQ to SQL sınıfları oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](displaying-a-table-of-database-data-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b5390-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image2.png))</span></span>


<span data-ttu-id="b5390-149">Varlık veri modeli Sihirbazı tamamladıktan sonra varlık veri modeli Tasarımcısı açılır.</span><span class="sxs-lookup"><span data-stu-id="b5390-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="b5390-150">Tasarımcı filmler varlık görüntülenmesi gerekir (bkz: Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="b5390-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="b5390-151">[![Varlık veri modeli Tasarımcısı](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b5390-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span></span>

<span data-ttu-id="b5390-152">**Şekil 02**: Varlık veri modeli Tasarımcısı ([tam boyutlu görüntüyü görmek için tıklatın](displaying-a-table-of-database-data-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="b5390-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image4.png))</span></span>


<span data-ttu-id="b5390-153">Biz devam etmeden önce bir değişiklik yapmak ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="b5390-153">We need to make one change before we continue.</span></span> <span data-ttu-id="b5390-154">Varlık veri Sihirbazı adlı bir model sınıfı oluşturur *filmler* film veritabanı tablosunu temsil eden.</span><span class="sxs-lookup"><span data-stu-id="b5390-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="b5390-155">Filmler sınıfı belirli bir filmi temsil etmek için kullanacağız çünkü olması için sınıfın adını değiştirmek ihtiyacımız *film* yerine *filmler* (tekil yerine çoğul).</span><span class="sxs-lookup"><span data-stu-id="b5390-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="b5390-156">Tasarımcı yüzeyinde sınıf adına çift tıklayın ve sınıfın adını film film değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b5390-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="b5390-157">Bu değişikliği yaptıktan sonra tıklayın **Kaydet** (disket simgesi) düğmesine film sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b5390-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="b5390-158">Denetleyici filmler oluşturun</span><span class="sxs-lookup"><span data-stu-id="b5390-158">Create the Movies Controller</span></span>

<span data-ttu-id="b5390-159">Veritabanı Kayıtlarımız temsil etmek için bir yolunu sunuyoruz, bir denetleyici filmler koleksiyonunu döndürür oluşturabiliriz.</span><span class="sxs-lookup"><span data-stu-id="b5390-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="b5390-160">Visual Studio Çözüm Gezgini penceresinde denetleyicileri klasörü sağ tıklatın ve menü seçeneğini **Ekle, denetleyici** (bkz: Şekil 3).</span><span class="sxs-lookup"><span data-stu-id="b5390-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="b5390-161">[![Menü denetleyicisi ekleme](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="b5390-161">[![The Add Controller Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span></span>

<span data-ttu-id="b5390-162">**Şekil 03**: denetleyicisi menü Ekle ([tam boyutlu görüntüyü görmek için tıklatın](displaying-a-table-of-database-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b5390-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image6.png))</span></span>


<span data-ttu-id="b5390-163">Zaman **denetleyici Ekle** iletişim kutusu açılır, denetleyici adı MovieController girin (bkz: Şekil 4).</span><span class="sxs-lookup"><span data-stu-id="b5390-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="b5390-164">Tıklayın **Ekle** düğmesini yeni denetleyici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b5390-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="b5390-165">[![Denetleyici Ekle iletişim kutusu](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="b5390-165">[![The Add Controller dialog](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span></span>

<span data-ttu-id="b5390-166">**Şekil 04**: Denetleyici Ekle iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](displaying-a-table-of-database-data-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="b5390-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image8.png))</span></span>


<span data-ttu-id="b5390-167">Biz, böylece veritabanı kayıt kümesini döndürür film denetleyici tarafından kullanıma sunulan İNDİS() eylem değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="b5390-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="b5390-168">Denetleyici Denetleyici 1 listeleme benzer şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b5390-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="b5390-169">**1 – Controllers\MovieController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="b5390-169">**Listing 1 – Controllers\MovieController.cs**</span></span>

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

<span data-ttu-id="b5390-170">Listeleme 1'de MoviesDBEntities sınıfı MoviesDB veritabanı temsil etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b5390-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="b5390-171">Bu sınıf kullanmak için bu gibi MvcApplication1.Models ad alanı içeri aktarmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="b5390-171">To use this class, you need to import the MvcApplication1.Models namespace like this:</span></span>

<span data-ttu-id="b5390-172">MvcApplication1.Models kullanma;</span><span class="sxs-lookup"><span data-stu-id="b5390-172">using MvcApplication1.Models;</span></span>

<span data-ttu-id="b5390-173">İfade *varlıklar. MovieSet.ToList()* film veritabanı tablosundan tüm filmlere kümesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="b5390-173">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="b5390-174">Görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="b5390-174">Create the View</span></span>

<span data-ttu-id="b5390-175">HTML tablosu halinde veritabanı kayıt kümesini görüntülemek için en kolay yolu, Visual Studio tarafından sağlanan yapı iskelesi yararlanmak sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="b5390-175">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="b5390-176">Menü seçeneği seçerek uygulamanızı **yapı, yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="b5390-176">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="b5390-177">Uygulamanızı açmadan önce derlemelidir **Görünüm Ekle** iletişim kutusu veya veri sınıflarınızı iletişim kutusunda görünmez.</span><span class="sxs-lookup"><span data-stu-id="b5390-177">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="b5390-178">Menü seçeneği İNDİS() eylem sağ tıklayıp **Görünüm Ekle** (bkz: Şekil 5).</span><span class="sxs-lookup"><span data-stu-id="b5390-178">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="b5390-179">[![Görünüm ekleme](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="b5390-179">[![Adding a view](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span></span>

<span data-ttu-id="b5390-180">**Şekil 05**: Görünüm ekleme ([tam boyutlu görüntüyü görmek için tıklatın](displaying-a-table-of-database-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="b5390-180">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="b5390-181">İçinde **Görünüm Ekle** iletişim kutusunda etiketli onay **kesin türü belirtilmiş görünüm oluşturmak**.</span><span class="sxs-lookup"><span data-stu-id="b5390-181">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="b5390-182">Film sınıfı olarak seçin **görüntülemek veri sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="b5390-182">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="b5390-183">Seçin *listesi* olarak **içeriği görüntüle** (bkz. Şekil 6).</span><span class="sxs-lookup"><span data-stu-id="b5390-183">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="b5390-184">Bu seçenekleri filmler listesini görüntüleyen bir kesin türü belirtilmiş görünüm oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b5390-184">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="b5390-185">[![Görünüm Ekle iletişim kutusu](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="b5390-185">[![The Add View dialog](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="b5390-186">**Şekil 06**: Görünüm Ekle iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](displaying-a-table-of-database-data-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="b5390-186">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image12.png))</span></span>


<span data-ttu-id="b5390-187">Tıkladıktan sonra **Ekle** 2 liste görünümünde düğme otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b5390-187">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="b5390-188">Bu görünüm, her bir filmi özelliklerini görüntüler ve filmler toplulukta tekrarlama için gerekli kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="b5390-188">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="b5390-189">**2 – Views\Movie\Index.aspx listeleme**</span><span class="sxs-lookup"><span data-stu-id="b5390-189">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

<span data-ttu-id="b5390-190">Menü seçeneğini belirleyerek uygulamayı çalıştırabilirsiniz **hata ayıklama, hata ayıklamayı Başlat** (veya F5 tuşuna basarak).</span><span class="sxs-lookup"><span data-stu-id="b5390-190">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="b5390-191">Uygulamayı çalıştıran, Internet Explorer başlatır.</span><span class="sxs-lookup"><span data-stu-id="b5390-191">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="b5390-192">Ardından /Movie URL'sine gidin, Şekil 7'de bir sayfa görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="b5390-192">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="b5390-193">[![Filmler tablosu](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="b5390-193">[![A table of movies](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span></span>

<span data-ttu-id="b5390-194">**Şekil 07**: filmler tablosu ([tam boyutlu görüntüyü görmek için tıklatın](displaying-a-table-of-database-data-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="b5390-194">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image14.png))</span></span>


<span data-ttu-id="b5390-195">Şekil 7'de veritabanı kayıtlarını kılavuz görünümünü ilgili hiçbir şeyi beğenmezseniz dizin görünümünün yalnızca değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b5390-195">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="b5390-196">Örneğin, değiştirebileceğiniz *DateReleased* başlığına *yayımlanma tarihi* dizin görünümünün değiştirerek.</span><span class="sxs-lookup"><span data-stu-id="b5390-196">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="b5390-197">İle kısmi bir şablon oluşturma</span><span class="sxs-lookup"><span data-stu-id="b5390-197">Create a Template with a Partial</span></span>

<span data-ttu-id="b5390-198">Çok karmaşık bir görünümünü alır, kısmi görünüm bozucu başlatmak için iyi bir fikirdir.</span><span class="sxs-lookup"><span data-stu-id="b5390-198">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="b5390-199">Kısmi bölümleri kullanarak kendi görünümlerinizi anlamak ve sürdürmek daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="b5390-199">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="b5390-200">Kısmi, oluşturacağız film veritabanı kayıtların her birinde biçimlendirmek için bir şablon olarak kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="b5390-200">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="b5390-201">Kısmi oluşturmak için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="b5390-201">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="b5390-202">Views\Movie klasörü sağ tıklatın ve menü seçeneğini **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="b5390-202">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="b5390-203">Etiketli onay *(.ascx) kısmi görünüm oluşturma*.</span><span class="sxs-lookup"><span data-stu-id="b5390-203">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="b5390-204">Kısmi ad *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="b5390-204">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="b5390-205">Etiketli onay **kesin türü belirtilmiş görünüm oluşturmak**.</span><span class="sxs-lookup"><span data-stu-id="b5390-205">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="b5390-206">Film olarak seçin *görüntülemek veri sınıfı*.</span><span class="sxs-lookup"><span data-stu-id="b5390-206">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="b5390-207">Boş olarak işaretleyin *içeriği görüntüle*.</span><span class="sxs-lookup"><span data-stu-id="b5390-207">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="b5390-208">Tıklayın **Ekle** düğmesini kısmi projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b5390-208">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="b5390-209">Bu adımları tamamladıktan sonra 3 listeleme gibi aramak için kısmi MovieTemplate değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b5390-209">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="b5390-210">**3 – Views\Movie\MovieTemplate.ascx listeleme**</span><span class="sxs-lookup"><span data-stu-id="b5390-210">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

<span data-ttu-id="b5390-211">Kısmi listeleme 3'te kayıt tek bir satır için bir şablonu içerir.</span><span class="sxs-lookup"><span data-stu-id="b5390-211">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="b5390-212">Listeleme 4'te değiştirilmiş dizini görünümü, kısmi MovieTemplate kullanır.</span><span class="sxs-lookup"><span data-stu-id="b5390-212">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="b5390-213">**4 – Views\Movie\Index.aspx listeleme**</span><span class="sxs-lookup"><span data-stu-id="b5390-213">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

<span data-ttu-id="b5390-214">Görünüm Listesi 4'te filmler tümünün yinelenen bir foreach döngüsü içeriyor.</span><span class="sxs-lookup"><span data-stu-id="b5390-214">The view in Listing 4 contains a foreach loop that iterates through all of the movies.</span></span> <span data-ttu-id="b5390-215">Her bir filmi kısmi MovieTemplate film biçimlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b5390-215">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="b5390-216">MovieTemplate RenderPartial() yardımcı yöntemini çağırarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="b5390-216">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="b5390-217">Veritabanı kayıtlarını çok aynı HTML tablosu değiştirilmiş Index görünümünü işler.</span><span class="sxs-lookup"><span data-stu-id="b5390-217">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="b5390-218">Ancak, görünümü önemli ölçüde basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b5390-218">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="b5390-219">Bir dize döndürmediğinden RenderPartial() yöntemi diğer yardımcı yöntemlerinin bir çoğu uygulamadan farklıdır.</span><span class="sxs-lookup"><span data-stu-id="b5390-219">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="b5390-220">Bu nedenle, RenderPartial() yöntemi kullanarak çağırmalıdır &lt;% Html.RenderPartial(); %&gt; yerine &lt;% Html.RenderPartial(); = %&gt;.</span><span class="sxs-lookup"><span data-stu-id="b5390-220">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial(); %&gt; instead of &lt;%= Html.RenderPartial(); %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="b5390-221">Özet</span><span class="sxs-lookup"><span data-stu-id="b5390-221">Summary</span></span>

<span data-ttu-id="b5390-222">Bu öğreticinin amacı, bir veritabanı kayıt kümesi ile HTML tablosu halinde nasıl görüntüleyebileceğiniz açıklamak için oluştu.</span><span class="sxs-lookup"><span data-stu-id="b5390-222">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="b5390-223">İlk olarak, Microsoft Entity Framework avantajlarından yararlanarak veritabanı kayıt kümesini bir denetleyici eylemi dönüş hakkında bilgi edindiniz.</span><span class="sxs-lookup"><span data-stu-id="b5390-223">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="b5390-224">Ardından, otomatik olarak öğelerin bir koleksiyonunu görüntüleyen bir görünüm oluşturmak için Visual Studio yapı iskelesi kullanmayı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="b5390-224">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="b5390-225">Son olarak, kısmi avantajlarından yararlanarak görünümü basitleştirmek hakkında bilgi edindiniz.</span><span class="sxs-lookup"><span data-stu-id="b5390-225">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="b5390-226">Kısmi bir şablon olarak kullanabilir, böylece her veritabanı kaydı biçimlendirebilirsiniz öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="b5390-226">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b5390-227">[Önceki](creating-model-classes-with-linq-to-sql-cs.md)
> [İleri](performing-simple-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b5390-227">[Previous](creating-model-classes-with-linq-to-sql-cs.md)
[Next](performing-simple-validation-cs.md)</span></span>
