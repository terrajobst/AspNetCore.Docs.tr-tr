---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Şablonu: Bir ASP.NET MVC 5 uygulamalarında EF kalıtım uygulama'
description: Bu öğreticide, veri modelinde devralma uygulanması gösterilmektedir.
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: df8715e4416ce3ccdf1d9e380addcded553d85f8
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444291"
---
# <a name="template-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a><span data-ttu-id="2137c-103">Şablonu: Bir ASP.NET MVC 5 uygulamasında EF kalıtım uygulama</span><span class="sxs-lookup"><span data-stu-id="2137c-103">Template: Implement Inheritance with EF in an ASP.NET MVC 5 app</span></span>

<span data-ttu-id="2137c-104">Önceki öğreticide eşzamanlılık özel durumları işlenir.</span><span class="sxs-lookup"><span data-stu-id="2137c-104">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="2137c-105">Bu öğreticide, veri modelinde devralma uygulanması gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2137c-105">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="2137c-106">Nesne yönelimli programlama, kullandığınız [devralma](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) kolaylaştırmak için [yeniden kod](http://en.wikipedia.org/wiki/Code_reuse).</span><span class="sxs-lookup"><span data-stu-id="2137c-106">In object-oriented programming, you can use [inheritance](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to facilitate [code reuse](http://en.wikipedia.org/wiki/Code_reuse).</span></span> <span data-ttu-id="2137c-107">Bu öğreticide, değiştireceksiniz `Instructor` ve `Student` oldukları türetilmesi sınıflara bir `Person` temel gibi özellikler içeren sınıf `LastName` eğitmenler ve öğrenciler için ortak olan.</span><span class="sxs-lookup"><span data-stu-id="2137c-107">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="2137c-108">Ekleyebilir veya herhangi bir web sayfalarını değiştirmesine olmaz ancak bazı kodları değiştireceksiniz ve bu değişiklikleri veritabanında otomatik olarak yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="2137c-108">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

<span data-ttu-id="2137c-109">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="2137c-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2137c-110">Devralma için veritabanı eşleme hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="2137c-110">Learn to map inheritance to database</span></span>
> * <span data-ttu-id="2137c-111">Kişi sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="2137c-111">Create the Person class</span></span>
> * <span data-ttu-id="2137c-112">Güncelleştirme Eğitmen ve Öğrenci</span><span class="sxs-lookup"><span data-stu-id="2137c-112">Update Instructor and Student</span></span>
> * <span data-ttu-id="2137c-113">Modele Kişi Ekle</span><span class="sxs-lookup"><span data-stu-id="2137c-113">Add Person to the Model</span></span>
> * <span data-ttu-id="2137c-114">Oluşturma ve geçişler güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="2137c-114">Create and Update Migrations</span></span>
> * <span data-ttu-id="2137c-115">Uygulama testi</span><span class="sxs-lookup"><span data-stu-id="2137c-115">Test the implementation</span></span>
> * <span data-ttu-id="2137c-116">Azure’a dağıtma</span><span class="sxs-lookup"><span data-stu-id="2137c-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2137c-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="2137c-117">Prerequisites</span></span>

* [<span data-ttu-id="2137c-118">Devralma Uygulama</span><span class="sxs-lookup"><span data-stu-id="2137c-118">Implementing Inheritance</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a><span data-ttu-id="2137c-119">Devralma için veritabanı eşleme</span><span class="sxs-lookup"><span data-stu-id="2137c-119">Map inheritance to database</span></span>

<span data-ttu-id="2137c-120">`Instructor` Ve `Student` sınıfları `School` veri modeline sahip aynı olan çeşitli özellikler:</span><span class="sxs-lookup"><span data-stu-id="2137c-120">The `Instructor` and `Student` classes in the `School` data model have several properties that are identical:</span></span>

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="2137c-122">Gereksiz kod tarafından paylaşılan özellikleri için kaldırmak istediğiniz varsayalım `Instructor` ve `Student` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="2137c-122">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="2137c-123">Veya adın bir eğitmen ya da bir öğrenci gelmediğini caring olmadan adları biçimlendirebilen hizmet yazmak istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="2137c-123">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="2137c-124">Oluşturabilirsiniz bir `Person` temel sınıfı, yalnızca bu paylaşılan özelliklerini içeren ve ardından olun `Instructor` ve `Student` varlıklar aşağıdaki çizimde gösterildiği gibi o temel sınıftan devralınan:</span><span class="sxs-lookup"><span data-stu-id="2137c-124">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="2137c-126">Bu devralma yapı veritabanında gösterilebilir birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="2137c-126">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="2137c-127">Sahip olabilir bir `Person` Öğrenciler hem tek bir tabloyu eğitmenlerini hakkında bilgi içeren tablo.</span><span class="sxs-lookup"><span data-stu-id="2137c-127">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="2137c-128">Bazı sütunların yalnızca eğitmen için geçerli olabilir (`HireDate`), bazı yalnızca Öğrenciler (`EnrollmentDate`), bazı her ikisine de (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="2137c-128">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="2137c-129">Genellikle, olurdu bir *ayrıştırıcı* her satır türü belirtmek için sütunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="2137c-129">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="2137c-130">Örneğin, ayrıştırıcı sütununu "Eğitmen" eğitmenler ve "Öğrenci" Öğrenciler için olabilir.</span><span class="sxs-lookup"><span data-stu-id="2137c-130">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Tablo başına hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="2137c-132">Bu düzen, tek veritabanı tablosundan bir varlık devralma yapısı oluşturma adlı *tablo başına hiyerarşi* (TPH) devralma.</span><span class="sxs-lookup"><span data-stu-id="2137c-132">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="2137c-133">Devralma yapısı gibi daha veritabanını yapmak için kullanılan bir alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="2137c-133">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="2137c-134">Yalnızca ad alanları gibi olabilir `Person` tablo ve ayrı sahip `Instructor` ve `Student` tablolarla birlikte tarih.</span><span class="sxs-lookup"><span data-stu-id="2137c-134">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Tablo başına type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="2137c-136">Her varlık sınıfı adı için bir veritabanı tablosu yaparak bu deseni *tablo başına tür* (TPT) devralma.</span><span class="sxs-lookup"><span data-stu-id="2137c-136">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="2137c-137">Henüz başka bir seçenek tüm soyut olmayan türler için tek tek tablolar eşlemektir.</span><span class="sxs-lookup"><span data-stu-id="2137c-137">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="2137c-138">Devralınan özellikler dahil olmak üzere, bir sınıfın tüm özellikler için karşılık gelen bir tablonun sütunlarını eşleyin.</span><span class="sxs-lookup"><span data-stu-id="2137c-138">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="2137c-139">Bu düzen (TPC) tablo başına somut sınıf devralma çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2137c-139">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="2137c-140">TPC devralma için uygulanırsa `Person`, `Student`, ve `Instructor` sınıfları daha önce gösterildiği gibi `Student` ve `Instructor` tabloları benzeyecektir farklı Öncekine göre devralma uyguladıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="2137c-140">If you implemented TPC inheritance for the `Person`, `Student`, and `Instructor` classes as shown earlier, the `Student` and `Instructor` tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="2137c-141">Karmaşık birleştirme sorgularda TPT desenleri sağladığından TPC ve TPH devralma desenler genellikle daha iyi performans varlık çerçevesi TPT devralma desenleri daha sunar.</span><span class="sxs-lookup"><span data-stu-id="2137c-141">TPC and TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="2137c-142">Bu öğreticide, TPH devralma uygulanması gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2137c-142">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="2137c-143">TPH olan Entity Framework varsayılan devralma desende yapmanız gereken tek şey, bu nedenle oluşturma bir `Person` sınıfı, değişiklik `Instructor` ve `Student` sınıfların türetilmesi için `Person`, yeni sınıfa eklemek `DbContext`, oluşturup bir geçiş.</span><span class="sxs-lookup"><span data-stu-id="2137c-143">TPH is the default inheritance pattern in the Entity Framework, so all you have to do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span> <span data-ttu-id="2137c-144">(Bir devralma desenlerinin nasıl uygulandığını hakkında daha fazla bilgi için bkz. [tablo başına tür (TPT) devralma eşleme](https://msdn.microsoft.com/data/jj591617#2.5) ve [tablo başına somut sınıf (TPC) devralma eşleme](https://msdn.microsoft.com/data/jj591617#2.6) MSDN'de Entity Framework belgeleri.)</span><span class="sxs-lookup"><span data-stu-id="2137c-144">(For information about how to implement the other inheritance patterns, see [Mapping the Table-Per-Type (TPT) Inheritance](https://msdn.microsoft.com/data/jj591617#2.5) and [Mapping the Table-Per-Concrete Class (TPC) Inheritance](https://msdn.microsoft.com/data/jj591617#2.6) in the MSDN Entity Framework documentation.)</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="2137c-145">Kişi sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="2137c-145">Create the Person class</span></span>

<span data-ttu-id="2137c-146">İçinde *modelleri* klasör oluşturma *Person.cs* ve şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="2137c-146">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a><span data-ttu-id="2137c-147">Güncelleştirme Eğitmen ve Öğrenci</span><span class="sxs-lookup"><span data-stu-id="2137c-147">Update Instructor and Student</span></span>

<span data-ttu-id="2137c-148">Şimdi Güncelleştir *Instructor.cs* ve *Sudent.cs* değerlerinden devralmak için *Person.sc*.</span><span class="sxs-lookup"><span data-stu-id="2137c-148">Now update the *Instructor.cs* and *Sudent.cs* to inherit values from the *Person.sc*.</span></span>

<span data-ttu-id="2137c-149">İçinde *Instructor.cs*, türetilen `Instructor` gelen sınıfı `Person` sınıfı ve anahtar ve ad alanlarını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="2137c-149">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="2137c-150">Kod, aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="2137c-150">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="2137c-151">Benzer değişiklik *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="2137c-151">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="2137c-152">`Student` Sınıfı aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="2137c-152">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a><span data-ttu-id="2137c-153">Modele Kişi Ekle</span><span class="sxs-lookup"><span data-stu-id="2137c-153">Add Person to the Model</span></span>

<span data-ttu-id="2137c-154">İçinde *SchoolContext.cs*, ekleme bir `DbSet` özelliği `Person` varlık türü:</span><span class="sxs-lookup"><span data-stu-id="2137c-154">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="2137c-155">Entity Framework tablo başına hiyerarşi devralmayı yapılandırmak için gereken budur.</span><span class="sxs-lookup"><span data-stu-id="2137c-155">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="2137c-156">Veritabanı güncelleştirildiğinde, anlatıldığı gibi sahip bir `Person` yerine tablo `Student` ve `Instructor` tablolar.</span><span class="sxs-lookup"><span data-stu-id="2137c-156">As you'll see, when the database is updated, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="create-and-update-migrations"></a><span data-ttu-id="2137c-157">Oluşturma ve geçişler güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="2137c-157">Create and Update Migrations</span></span>

<span data-ttu-id="2137c-158">Paket Yöneticisi Konsolu (PMC'de), aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="2137c-158">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="2137c-159">Çalıştırma `Update-Database` PMC'yi komutunu.</span><span class="sxs-lookup"><span data-stu-id="2137c-159">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="2137c-160">Geçişleri ne yapılacağını bildiğiniz olmayan mevcut verileri aldık komutu bu noktada başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="2137c-160">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="2137c-161">Bir hata iletisi aşağıdakine benzer olursunuz:</span><span class="sxs-lookup"><span data-stu-id="2137c-161">You get an error message like the following one:</span></span>

> <span data-ttu-id="2137c-162">*Nesne bırakılamadı ' dbo. Eğitmen ' yabancı anahtar kısıtlaması tarafından başvurulduğundan.*</span><span class="sxs-lookup"><span data-stu-id="2137c-162">*Could not drop object 'dbo.Instructor' because it is referenced by a FOREIGN KEY constraint.*</span></span>


<span data-ttu-id="2137c-163">Açık *geçişler\&lt; zaman damgası&gt;\_Inheritance.cs* değiştirin `Up` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="2137c-163">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="2137c-164">Bu kod aşağıdaki veritabanı güncelleştirme görevleri üstlenir:</span><span class="sxs-lookup"><span data-stu-id="2137c-164">This code takes care of the following database update tasks:</span></span>

- <span data-ttu-id="2137c-165">Yabancı anahtar kısıtlamaları ve Öğrenci tabloya noktası dizinleri kaldırır.</span><span class="sxs-lookup"><span data-stu-id="2137c-165">Removes foreign key constraints and indexes that point to the Student table.</span></span>
- <span data-ttu-id="2137c-166">Eğitmen tablo kişi olarak yeniden adlandırır ve Öğrenci verileri depolamak gerekli değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="2137c-166">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

    - <span data-ttu-id="2137c-167">Öğrenciler için boş değer atanabilir EnrollmentDate ekler.</span><span class="sxs-lookup"><span data-stu-id="2137c-167">Adds nullable EnrollmentDate for students.</span></span>
    - <span data-ttu-id="2137c-168">Bir satır için bir öğrenci ya da bir eğitmen olup olmadığını belirtmek için ayrıştırıcı sütununu ekler.</span><span class="sxs-lookup"><span data-stu-id="2137c-168">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>
    - <span data-ttu-id="2137c-169">Öğrenci satırları seferde olmaz beri İşeAlmaTarihi boş değer atanabilir bir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="2137c-169">Makes HireDate nullable since student rows won't have hire dates.</span></span>
    - <span data-ttu-id="2137c-170">Öğrenciler için işaret yabancı anahtarlar güncelleştirmek için kullanılan geçici bir alan ekler.</span><span class="sxs-lookup"><span data-stu-id="2137c-170">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="2137c-171">Kişi tabloya Öğrenciler kopyaladığınızda yeni birincil anahtar değerlerini elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="2137c-171">When you copy students into the Person table they'll get new primary key values.</span></span>
- <span data-ttu-id="2137c-172">Kişi tabloya Öğrenci tablodan veri kopyalar.</span><span class="sxs-lookup"><span data-stu-id="2137c-172">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="2137c-173">Bu, Öğrenciler, yeni birincil anahtar değerlerini atanan neden olur.</span><span class="sxs-lookup"><span data-stu-id="2137c-173">This causes students to get assigned new primary key values.</span></span>
- <span data-ttu-id="2137c-174">Öğrenciler için işaret yabancı anahtar değerlerine düzeltir.</span><span class="sxs-lookup"><span data-stu-id="2137c-174">Fixes foreign key values that point to students.</span></span>
- <span data-ttu-id="2137c-175">Yabancı anahtar kısıtlamaları ve artık kişi tabloya işaret eden, dizinleri yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2137c-175">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="2137c-176">(GUID yerine tamsayı birincil anahtar türü kullansaydınız, Öğrenci birincil anahtar değerlerini değiştirmek zorunda mıydı ve bu adımların birçok atlandı.)</span><span class="sxs-lookup"><span data-stu-id="2137c-176">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="2137c-177">Çalıştırma `update-database` yeniden komutu.</span><span class="sxs-lookup"><span data-stu-id="2137c-177">Run the `update-database` command again.</span></span>

<span data-ttu-id="2137c-178">(Önceki veritabanı sürümüne geri dönmek için kullanan gerekiyordu durumunda bir üretim sisteminde karşılık gelen aşağı yöntemi değişiklik.</span><span class="sxs-lookup"><span data-stu-id="2137c-178">(In a production system you would make corresponding changes to the Down method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="2137c-179">Bu öğretici için aşağıya yöntemi kullanıyor olmanız gerekmez.)</span><span class="sxs-lookup"><span data-stu-id="2137c-179">For this tutorial you won't be using the Down method.)</span></span>

> [!NOTE]
> <span data-ttu-id="2137c-180">Veri ve yapma şema değişiklikleri geçiş sırasında diğer hatalarıyla mümkündür.</span><span class="sxs-lookup"><span data-stu-id="2137c-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="2137c-181">Geçiş hatalarla karşılaşırsanız, çözümleyemiyor, devam ederek öğreticiyle bağlantı dizesini değiştirerek *Web.config* dosya veya veritabanı silerek.</span><span class="sxs-lookup"><span data-stu-id="2137c-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or by deleting the database.</span></span> <span data-ttu-id="2137c-182">Veritabanında yeniden adlandırmak için en basit yaklaşımdır *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="2137c-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="2137c-183">Örneğin, veritabanı adını ContosoUniversity2 için aşağıdaki örnekte gösterildiği gibi değiştirin:</span><span class="sxs-lookup"><span data-stu-id="2137c-183">For example, change the database name to ContosoUniversity2 as shown in the following example:</span></span>
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> <span data-ttu-id="2137c-184">Yeni bir veritabanı ile geçirmek için veri yoktur ve `update-database` hatasız tamamlanması çok daha büyük olasılıkla komutu.</span><span class="sxs-lookup"><span data-stu-id="2137c-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="2137c-185">Veritabanı silme hakkında yönergeler için bkz: [Visual Studio 2012'den bir veritabanını bırakmak nasıl](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="2137c-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="2137c-186">Öğretici ile devam etmek için bu yaklaşımı benimsemeniz durumunda, bu öğreticinin sonunda dağıtım adımı atlayın veya yeni site ve veritabanı'na dağıtın.</span><span class="sxs-lookup"><span data-stu-id="2137c-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial or deploy to a new site and database.</span></span> <span data-ttu-id="2137c-187">Bir güncelleştirme zaten dağıtım siteye dağıtırsanız, EF geçişleri otomatik olarak çalıştığında aynı hatayı alırsınız.</span><span class="sxs-lookup"><span data-stu-id="2137c-187">If you deploy an update to the same site you've been deploying to already, EF will get the same error there when it runs migrations automatically.</span></span> <span data-ttu-id="2137c-188">Geçişleri hatayı gidermek, en iyi bir Entity Framework forumları veya StackOverflow.com kaynaktır.</span><span class="sxs-lookup"><span data-stu-id="2137c-188">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>

## <a name="test-the-implementation"></a><span data-ttu-id="2137c-189">Uygulama testi</span><span class="sxs-lookup"><span data-stu-id="2137c-189">Test the implementation</span></span>

<span data-ttu-id="2137c-190">Siteyi çalıştırın ve çeşitli sayfalar deneyin.</span><span class="sxs-lookup"><span data-stu-id="2137c-190">Run the site and try various pages.</span></span> <span data-ttu-id="2137c-191">Önce yaptığınız gibi her şey aynı çalışır.</span><span class="sxs-lookup"><span data-stu-id="2137c-191">Everything works the same as it did before.</span></span>

<span data-ttu-id="2137c-192">İçinde **Sunucu Gezgini** genişletin **veri Connections\SchoolContext** ardından **tabloları**, ve gördüğünüz **Öğrenci** ve **Eğitmen** tablolar tarafından değiştirildi bir **kişi** tablo.</span><span class="sxs-lookup"><span data-stu-id="2137c-192">In **Server Explorer,** expand **Data Connections\SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="2137c-193">Genişletin **kişi** tablo ve bkz tüm bulunması için kullanılan sütunları olan **Öğrenci** ve **Eğitmen** tablolar.</span><span class="sxs-lookup"><span data-stu-id="2137c-193">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

<span data-ttu-id="2137c-194">Kişi tabloya sağ tıklayıp ardından **tablo verilerini Göster** ayrıştırıcı sütununu görmek için.</span><span class="sxs-lookup"><span data-stu-id="2137c-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

<span data-ttu-id="2137c-195">Aşağıdaki diyagram, yeni School veritabanını yapısını gösterir:</span><span class="sxs-lookup"><span data-stu-id="2137c-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="2137c-197">Azure’a dağıtma</span><span class="sxs-lookup"><span data-stu-id="2137c-197">Deploy to Azure</span></span>

<span data-ttu-id="2137c-198">Bu bölümde isteğe bağlı tamamlamış olmanız gerekir **uygulamasını Azure'a dağıtma** konusundaki [bölüm 3, sıralama, filtreleme ve sayfalama](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) Bu öğretici serisinin.</span><span class="sxs-lookup"><span data-stu-id="2137c-198">This section requires you to have completed the optional **Deploying the app to Azure** section in [Part 3, Sorting, Filtering, and Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) of this tutorial series.</span></span> <span data-ttu-id="2137c-199">Veritabanı yerel projenizdeki silerek çözümlenen geçişleri hatasız tamamlanırsa, bu adımı atlayın; veya yeni site ve veritabanı oluşturun ve yeni ortama dağıtın.</span><span class="sxs-lookup"><span data-stu-id="2137c-199">If you had migrations errors that you resolved by deleting the database in your local project, skip this step; or create a new site and database, and deploy to the new environment.</span></span>

1. <span data-ttu-id="2137c-200">Visual Studio'da projeye sağ **Çözüm Gezgini** seçip **Yayımla** bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="2137c-200">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>

2. <span data-ttu-id="2137c-201">Tıklayın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="2137c-201">Click **Publish**.</span></span>

    <span data-ttu-id="2137c-202">Web uygulamasını varsayılan tarayıcınızda açılır.</span><span class="sxs-lookup"><span data-stu-id="2137c-202">The Web app opens in your default browser.</span></span>

3. <span data-ttu-id="2137c-203">Bunu doğrulamak için uygulamayı test etme çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2137c-203">Test the application to verify it's working.</span></span>

    <span data-ttu-id="2137c-204">Bir sayfa, ilk kez çalıştırdığınızda, veritabanına erişir, Entity Framework tüm geçişlerde çalıştırır `Up` veritabanı geçerli bir veri modeli ile güncel duruma getirmek için gerekli yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="2137c-204">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="2137c-205">Kodu alma</span><span class="sxs-lookup"><span data-stu-id="2137c-205">Get the code</span></span>

[<span data-ttu-id="2137c-206">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="2137c-206">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a><span data-ttu-id="2137c-207">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2137c-207">Additional resources</span></span>

<span data-ttu-id="2137c-208">Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET veri erişimi - önerilen kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="2137c-208">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="2137c-209">Bu ve diğer devralma yapıları hakkında daha fazla bilgi için bkz. [TPT devralma deseni](https://msdn.microsoft.com/data/jj618293) ve [TPH devralma deseni](https://msdn.microsoft.com/data/jj618292) MSDN'de.</span><span class="sxs-lookup"><span data-stu-id="2137c-209">For more information about this and other inheritance structures, see [TPT Inheritance Pattern](https://msdn.microsoft.com/data/jj618293) and [TPH Inheritance Pattern](https://msdn.microsoft.com/data/jj618292) on MSDN.</span></span> <span data-ttu-id="2137c-210">Sonraki öğreticide, bir göreceli olarak Gelişmiş Entity Framework senaryoları işlemek nasıl görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="2137c-210">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2137c-211">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="2137c-211">Next steps</span></span>

<span data-ttu-id="2137c-212">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="2137c-212">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2137c-213">Devralma veritabanına eşlemek öğrendiniz</span><span class="sxs-lookup"><span data-stu-id="2137c-213">Learned to map inheritance to database</span></span>
> * <span data-ttu-id="2137c-214">Kişi Sınıf oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="2137c-214">Created the Person class</span></span>
> * <span data-ttu-id="2137c-215">Güncelleştirilmiş bir eğitmen ve Öğrenci</span><span class="sxs-lookup"><span data-stu-id="2137c-215">Updated Instructor and Student</span></span>
> * <span data-ttu-id="2137c-216">Eklenen kişi modeli</span><span class="sxs-lookup"><span data-stu-id="2137c-216">Added Person to the Model</span></span>
> * <span data-ttu-id="2137c-217">Oluşturulan ve güncelleştirme geçişleri</span><span class="sxs-lookup"><span data-stu-id="2137c-217">Created and Update Migrations</span></span>
> * <span data-ttu-id="2137c-218">Test uygulaması</span><span class="sxs-lookup"><span data-stu-id="2137c-218">Tested the implementation</span></span>
> * <span data-ttu-id="2137c-219">Azure'a dağıtılan</span><span class="sxs-lookup"><span data-stu-id="2137c-219">Deployed to Azure</span></span>

<span data-ttu-id="2137c-220">Entity Framework Code First kullanan ASP.NET web uygulamaları geliştirmenin temellerini gidin ne zaman uyumlu olması için kullanışlı olan konular hakkında bilgi edinmek için sonraki makaleye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="2137c-220">Advance to the next article to learn about topics that are useful to be aware of when you go beyond the basics of developing ASP.NET web applications that use Entity Framework Code First.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="2137c-221">Gelişmiş Entity Framework Senaryoları</span><span class="sxs-lookup"><span data-stu-id="2137c-221">Advanced Entity Framework Scenarios</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)