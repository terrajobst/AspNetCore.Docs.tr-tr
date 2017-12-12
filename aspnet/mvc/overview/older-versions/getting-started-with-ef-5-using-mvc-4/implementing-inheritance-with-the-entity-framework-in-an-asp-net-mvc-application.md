---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Bir ASP.NET MVC uygulamasındaki (8, 10) Entity Framework kalıtım uygulama | Microsoft Docs"
author: tdykstra
description: "Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio kullanarak ASP.NET MVC 4 uygulamaları oluşturmak nasıl gösteren..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 54e46c6f996b6fe86a227c851562e61678b02780
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a><span data-ttu-id="0e580-103">Bir ASP.NET MVC uygulamasındaki (8, 10) Entity Framework kalıtım uygulama</span><span class="sxs-lookup"><span data-stu-id="0e580-103">Implementing Inheritance with the Entity Framework in an ASP.NET MVC Application (8 of 10)</span></span>
====================
<span data-ttu-id="0e580-104">tarafından [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0e580-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="0e580-105">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="0e580-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="0e580-106">Contoso University örnek web uygulaması Entity Framework 5 Code First ve Visual Studio 2012 kullanarak ASP.NET MVC 4 uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="0e580-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="0e580-107">Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="0e580-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="0e580-108">Eğitmen serisi baştan başlayabilirsiniz veya [Bu bölüm için bir başlangıç projesi indirme](building-the-ef5-mvc4-chapter-downloads.md) ve buradan başlayın.</span><span class="sxs-lookup"><span data-stu-id="0e580-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="0e580-109">Olamaz gidermek, bir sorunla çalıştırırsanız [tamamlanmış bölüm karşıdan](building-the-ef5-mvc4-chapter-downloads.md) ve sorunu yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="0e580-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="0e580-110">Tamamlanan kodu kodunuzu karşılaştırarak genellikle soruna çözüm bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0e580-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="0e580-111">Bazı yaygın hatalar ve bunları çözmek nasıl için bkz: [hatalarını ve geçici çözümler.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="0e580-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="0e580-112">Önceki öğreticide eşzamanlılık işlenir.</span><span class="sxs-lookup"><span data-stu-id="0e580-112">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="0e580-113">Bu öğreticide veri modelinde devralma uygulamak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="0e580-113">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="0e580-114">Nesne odaklı programlama içinde devralma yedekli kod ortadan kaldırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0e580-114">In object-oriented programming, you can use inheritance to eliminate redundant code.</span></span> <span data-ttu-id="0e580-115">Bu öğreticide, değiştireceğiz `Instructor` ve `Student` öğesinden türetilen sınıflara bir `Person` temel gibi özellikler içeren sınıfı `LastName` Eğitmen ve öğrenciler için ortak olan.</span><span class="sxs-lookup"><span data-stu-id="0e580-115">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="0e580-116">Ekleme veya tüm web sayfalarını değiştirme olmaz ancak bazı kodları değiştireceksiniz ve bu değişiklikleri otomatik olarak veritabanında yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="0e580-116">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a><span data-ttu-id="0e580-117">Tablo başına hiyerarşisi tablo başına türü devralma karşılaştırması</span><span class="sxs-lookup"><span data-stu-id="0e580-117">Table-per-Hierarchy versus Table-per-Type Inheritance</span></span>

<span data-ttu-id="0e580-118">Nesne odaklı programlama ile ilgili sınıflar iş kolaylaştırmak için devralma kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0e580-118">In object-oriented programming, you can use inheritance to make it easier to work with related classes.</span></span> <span data-ttu-id="0e580-119">Örneğin, `Instructor` ve `Student` sınıfları `School` veri modeli, hangi yedek kodda sonuçları birkaç özellikleri paylaşır:</span><span class="sxs-lookup"><span data-stu-id="0e580-119">For example, the `Instructor` and `Student` classes in the `School` data model share several properties, which results in redundant code:</span></span>

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

<span data-ttu-id="0e580-121">Tarafından paylaşılan özellikler için yedek kod ortadan kaldırmak istediğinizi varsayalım `Instructor` ve `Student` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="0e580-121">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="0e580-122">Oluşturabilirsiniz bir `Person` temel yalnızca bu özellikleri paylaşılan içeren sınıfı sonra olun `Instructor` ve `Student` varlıklar aşağıdaki çizimde gösterildiği gibi bu taban sınıfından devral:</span><span class="sxs-lookup"><span data-stu-id="0e580-122">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

<span data-ttu-id="0e580-124">Bu devralma yapı veritabanında gösterilebilir birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="0e580-124">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="0e580-125">Sahip olabilir bir `Person` Öğrenciler ve tek bir tablodaki Eğitmen hakkında bilgi içeren tablo.</span><span class="sxs-lookup"><span data-stu-id="0e580-125">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="0e580-126">Bazı sütunları yalnızca eğitmen için geçerli olabilir (`HireDate`), bazı yalnızca Öğrenciler (`EnrollmentDate`), bazı iki (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="0e580-126">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="0e580-127">Genellikle, olurdu bir *Ayrıştırıcıyı* her satır tür belirtmek için sütun temsil eder.</span><span class="sxs-lookup"><span data-stu-id="0e580-127">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="0e580-128">Örneğin, ayrıştırıcı sütunun "Eğitmen" Eğitmen ve "Öğrenci" Öğrenciler için olabilir.</span><span class="sxs-lookup"><span data-stu-id="0e580-128">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Tablo başına hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

<span data-ttu-id="0e580-130">Bir varlığın devralma yapısı tek veritabanı tablosundan oluşturmanın bu deseni adlı *tablo başına hiyerarşisi* (TPH) devralma.</span><span class="sxs-lookup"><span data-stu-id="0e580-130">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="0e580-131">Devralma yapısı gibi daha fazla ara veritabanını yapmak için kullanılan bir alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="0e580-131">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="0e580-132">Yalnızca ad alanları gibi olabilir `Person` tablo ve ayrı sahip `Instructor` ve `Student` tarih alanları tablolarla.</span><span class="sxs-lookup"><span data-stu-id="0e580-132">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Tablo başına type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

<span data-ttu-id="0e580-134">Her varlık sınıfı adlı için bir veritabanı tablosu yapma bu deseni *tablo türü başına* (birleştirilmiş TPT) devralma.</span><span class="sxs-lookup"><span data-stu-id="0e580-134">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="0e580-135">Birleştirilmiş TPT desenleri karmaşık JOIN sorguları sağladığından TPH devralma desenleri birleştirilmiş TPT devralma düzenleri daha Entity Framework içinde daha iyi performans genellikle sunar.</span><span class="sxs-lookup"><span data-stu-id="0e580-135">TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span> <span data-ttu-id="0e580-136">Bu öğretici nasıl TPH devralma uygulanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="0e580-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="0e580-137">Bu, aşağıdaki adımları gerçekleştirerek gerçekleştirirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0e580-137">You'll do that by performing the following steps:</span></span>

- <span data-ttu-id="0e580-138">Oluşturma bir `Person` sınıfı ve değişiklik `Instructor` ve `Student` öğesinden türetilen sınıflar `Person`.</span><span class="sxs-lookup"><span data-stu-id="0e580-138">Create a `Person` class and change the `Instructor` and `Student` classes to derive from `Person`.</span></span>
- <span data-ttu-id="0e580-139">Model veritabanı eşleme kodu veritabanı bağlamı sınıfına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0e580-139">Add model-to-database mapping code to the database context class.</span></span>
- <span data-ttu-id="0e580-140">Değişiklik `InstructorID` ve `StudentID` başvuruları projeye boyunca `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="0e580-140">Change `InstructorID` and `StudentID` references throughout the project to `PersonID`.</span></span>

## <a name="creating-the-person-class"></a><span data-ttu-id="0e580-141">Kişi sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="0e580-141">Creating the Person Class</span></span>

 <span data-ttu-id="0e580-142">Not: Bu sınıfları kullanan denetleyicileri güncelleştirilene kadar sınıfları oluşturduktan sonra projeyi derlemek mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="0e580-142">Note: You won't be able to compile the project after creating the classes below until you update the controllers that uses these classes.</span></span> 

<span data-ttu-id="0e580-143">İçinde *modelleri* klasörü oluşturmak *Person.cs* ve şablon kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="0e580-143">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="0e580-144">İçinde *Instructor.cs*, türetilen `Instructor` sınıfıyla `Person` sınıfı ve anahtar ve ad alanlarını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="0e580-144">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="0e580-145">Kod aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="0e580-145">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="0e580-146">Benzer değişiklik *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="0e580-146">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="0e580-147">`Student` Sınıfı, aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="0e580-147">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a><span data-ttu-id="0e580-148">Kişi varlık türü için Model ekleme</span><span class="sxs-lookup"><span data-stu-id="0e580-148">Adding the Person Entity Type to the Model</span></span>

<span data-ttu-id="0e580-149">İçinde *SchoolContext.cs*, ekleme bir `DbSet` özelliği için `Person` varlık türü:</span><span class="sxs-lookup"><span data-stu-id="0e580-149">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="0e580-150">Entity Framework tablo başına hiyerarşisi devralma yapılandırmak için gereken budur.</span><span class="sxs-lookup"><span data-stu-id="0e580-150">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="0e580-151">Veritabanı yeniden oluşturulan olduğunda, göreceğiniz gibi sahip bir `Person` yerine tablo `Student` ve `Instructor` tabloları.</span><span class="sxs-lookup"><span data-stu-id="0e580-151">As you'll see, when the database is re-created, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="changing-instructorid-and-studentid-to-personid"></a><span data-ttu-id="0e580-152">InstructorID ve StudentID PersonID için değiştirme</span><span class="sxs-lookup"><span data-stu-id="0e580-152">Changing InstructorID and StudentID to PersonID</span></span>

<span data-ttu-id="0e580-153">İçinde *SchoolContext.cs*, eğitmen indirmelere eşleme ifadesini değiştirin `MapRightKey("InstructorID")` için `MapRightKey("PersonID")`:</span><span class="sxs-lookup"><span data-stu-id="0e580-153">In *SchoolContext.cs*, in the Instructor-Course mapping statement, change `MapRightKey("InstructorID")` to `MapRightKey("PersonID")`:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

<span data-ttu-id="0e580-154">Bu değişikliği gerekli değildir; yalnızca, çoktan bire çok birleşme tablodaki InstructorID sütun adını değiştirir.</span><span class="sxs-lookup"><span data-stu-id="0e580-154">This change isn't required; it just changes the name of the InstructorID column in the many-to-many join table.</span></span> <span data-ttu-id="0e580-155">Uygulama adı InstructorID olarak bırakılırsa, hala düzgün çalışır.</span><span class="sxs-lookup"><span data-stu-id="0e580-155">If you left the name as InstructorID, the application would still work correctly.</span></span> <span data-ttu-id="0e580-156">İşte tamamlanan *SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="0e580-156">Here is the completed *SchoolContext.cs*:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

<span data-ttu-id="0e580-157">Sonraki değiştirmeye ihtiyaç `InstructorID` için `PersonID` ve `StudentID` için `PersonID` proje boyunca ***dışında*** zaman damgalı geçişler dosyalarında *geçişler* klasör.</span><span class="sxs-lookup"><span data-stu-id="0e580-157">Next you need to change `InstructorID` to `PersonID` and `StudentID` to `PersonID` throughout the project ***except*** in the time-stamped migrations files in the *Migrations* folder.</span></span> <span data-ttu-id="0e580-158">Bunu yapmak için bulmak ve açık yalnızca değiştirilmesi gereken dosyalar ardından açık dosyalarda genel değişiklik yapmak.</span><span class="sxs-lookup"><span data-stu-id="0e580-158">To do that you'll find and open only the files that need to be changed, then perform a global change on the opened files.</span></span> <span data-ttu-id="0e580-159">Yalnızca dosyasında *geçişler* değiştirmek klasördür *Migrations\Configuration.cs.*</span><span class="sxs-lookup"><span data-stu-id="0e580-159">The only file in the *Migrations* folder you should change is *Migrations\Configuration.cs.*</span></span>

1. > [!IMPORTANT]
 > <span data-ttu-id="0e580-160">Visual Studio'da açık olan tüm dosyaları kapatarak başlar.</span><span class="sxs-lookup"><span data-stu-id="0e580-160">Begin by closing all the open files in Visual Studio.</span></span>
2. <span data-ttu-id="0e580-161">Tıklatın **Bul ve Değiştir – tüm dosyaları bulmak** içinde **Düzenle** menüsüne ve ardından arama içeren projedeki tüm dosyalar için `InstructorID`.</span><span class="sxs-lookup"><span data-stu-id="0e580-161">Click **Find and Replace -- Find all Files** in the **Edit** menu, and then search for all files in the project that contain `InstructorID`.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. <span data-ttu-id="0e580-162">Her dosya içine açmak **Bul sonuçları** penceresi ***dışında*** &lt;zaman damgası&gt;*\_.cs* geçişdosyalarında*Geçişler* her dosya için bir satırı çift tıklatarak klasör.</span><span class="sxs-lookup"><span data-stu-id="0e580-162">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. <span data-ttu-id="0e580-163">Açık **dosyalarda Değiştir** iletişim ve değişiklik **konum** için **tüm açık belgeleri**.</span><span class="sxs-lookup"><span data-stu-id="0e580-163">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
5. <span data-ttu-id="0e580-164">Kullanım **dosyalarda Değiştir** tüm değiştirmek için iletişim `InstructorID` için`PersonID.`</span><span class="sxs-lookup"><span data-stu-id="0e580-164">Use the **Replace in Files** dialog to change all `InstructorID` to `PersonID.`</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. <span data-ttu-id="0e580-165">Tüm dosyaları içeren projeye bulmak `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="0e580-165">Find all the files in the project that contain `StudentID`.</span></span>
7. <span data-ttu-id="0e580-166">Her dosya içine açmak **Bul sonuçları** penceresi ***dışında*** &lt;zaman damgası&gt;*\_\*.cs* geçiş dosyaları içinde *geçişler* her dosya için bir satırı çift tıklatarak klasör.</span><span class="sxs-lookup"><span data-stu-id="0e580-166">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_\*.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. <span data-ttu-id="0e580-167">Açık **dosyalarda Değiştir** iletişim ve değişiklik **konum** için **tüm açık belgeleri**.</span><span class="sxs-lookup"><span data-stu-id="0e580-167">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
9. <span data-ttu-id="0e580-168">Kullanım **dosyalarda Değiştir** tüm değiştirmek için iletişim `StudentID` için `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="0e580-168">Use the **Replace in Files** dialog to change all `StudentID` to `PersonID`.</span></span>   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. <span data-ttu-id="0e580-169">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0e580-169">Build the project.</span></span>

<span data-ttu-id="0e580-170">(Bu gösterir Not bir *dezavantajı* , `classnameID` birincil anahtarlar adlandırma deseni.</span><span class="sxs-lookup"><span data-stu-id="0e580-170">(Note that this demonstrates a *disadvantage* of the `classnameID` pattern for naming primary keys.</span></span> <span data-ttu-id="0e580-171">Sınıf adı önek olmadan birincil anahtarlar kimliği adlı değilse *hiçbir* yeniden adlandırma olacaktır gerekli şimdi.)</span><span class="sxs-lookup"><span data-stu-id="0e580-171">If you had named primary keys ID without prefixing the class name, *no* renaming would be necessary now.)</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="0e580-172">Oluşturma ve geçişler dosyasını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="0e580-172">Create and Update a Migrations File</span></span>

<span data-ttu-id="0e580-173">Paket Yöneticisi Konsolu (PMC)'da, aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="0e580-173">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="0e580-174">Çalıştırma `Update-Database` PMC komutu.</span><span class="sxs-lookup"><span data-stu-id="0e580-174">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="0e580-175">Geçişleri nasıl ele alınacağını bilmeniz değil mevcut verileri sahibiz komutu bu noktada başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="0e580-175">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="0e580-176">Aşağıdaki hatayı alıyorsunuz:</span><span class="sxs-lookup"><span data-stu-id="0e580-176">You get the following error:</span></span>

<span data-ttu-id="0e580-177">*ALTER TABLE deyimi yabancı anahtar kısıtlaması ile çakıştı "FK\_dbo. Departman\_dbo. Kişi\_PersonID ". Veritabanı "ContosoUniversity" Tablo "dbo. çakışma oluştu Kişi", sütun 'PersonID'.*</span><span class="sxs-lookup"><span data-stu-id="0e580-177">*The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK\_dbo.Department\_dbo.Person\_PersonID". The conflict occurred in database "ContosoUniversity", table "dbo.Person", column 'PersonID'.*</span></span>

<span data-ttu-id="0e580-178">Açık *geçişler\&lt; zaman damgası&gt;\_Inheritance.cs* ve değiştirme `Up` aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="0e580-178">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

<span data-ttu-id="0e580-179">Çalıştırma `update-database` yeniden komutu.</span><span class="sxs-lookup"><span data-stu-id="0e580-179">Run the `update-database` command again.</span></span>

> [!NOTE]
> <span data-ttu-id="0e580-180">Veri ve yapma şema değişiklikleri geçirilirken diğer hatalarıyla mümkündür.</span><span class="sxs-lookup"><span data-stu-id="0e580-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="0e580-181">Geçiş hataları alırsanız, olamaz gidermek, devam edebilirsiniz eğitici bağlantı dizesinde değiştirerek *Web.config* dosya veya veritabanı siliniyor.</span><span class="sxs-lookup"><span data-stu-id="0e580-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or deleting the database.</span></span> <span data-ttu-id="0e580-182">Veritabanında yeniden adlandırmak için basit yaklaşımdır *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="0e580-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="0e580-183">Örneğin, CU için veritabanı adını değiştirin\_aşağıdaki örnekte gösterildiği gibi test edin:</span><span class="sxs-lookup"><span data-stu-id="0e580-183">For example, change the database name to CU\_test as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> <span data-ttu-id="0e580-184">Yeni bir veritabanı ile geçirmek için veri yok ve `update-database` komut hatasız tamamlamak çok daha büyük bir olasılıkla.</span><span class="sxs-lookup"><span data-stu-id="0e580-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="0e580-185">Veritabanını silmek yönergeler için bkz: [Visual Studio 2012'den bir veritabanını bırakmak nasıl](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="0e580-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="0e580-186">Bu öğreticiyi devam edebilmek için bu yaklaşımı benimsemeye varsa, geçişler otomatik çalıştırıldığında dağıtılan site aynı hata elde edebileceğiniz beri bu öğreticinin sonunda dağıtım adımı atlayın.</span><span class="sxs-lookup"><span data-stu-id="0e580-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial, since the deployed site would get the same error when it runs migrations automatically.</span></span> <span data-ttu-id="0e580-187">Geçişler hata için sorun giderme istiyorsanız, en iyi kaynak Entity Framework forumları veya StackOverflow.com biridir.</span><span class="sxs-lookup"><span data-stu-id="0e580-187">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="0e580-188">Sınama</span><span class="sxs-lookup"><span data-stu-id="0e580-188">Testing</span></span>

<span data-ttu-id="0e580-189">Siteyi çalıştırın ve çeşitli sayfalar deneyin.</span><span class="sxs-lookup"><span data-stu-id="0e580-189">Run the site and try various pages.</span></span> <span data-ttu-id="0e580-190">Önce yaptığınız gibi her şey aynı çalışır.</span><span class="sxs-lookup"><span data-stu-id="0e580-190">Everything works the same as it did before.</span></span>

<span data-ttu-id="0e580-191">İçinde **Sunucu Gezgini** genişletin **SchoolContext** ve ardından **tabloları**, ve, gördüğünüz **Öğrenci** ve **Eğitmen**  tabloları değiştirilir tarafından bir **kişi** tablo.</span><span class="sxs-lookup"><span data-stu-id="0e580-191">In **Server Explorer,** expand **SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="0e580-192">Genişletme **kişi** tablo göreceksiniz bulunması için kullanılan sütunların tümünün sahip **Öğrenci** ve **Eğitmen** tabloları.</span><span class="sxs-lookup"><span data-stu-id="0e580-192">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="0e580-194">Kişi tabloyu sağ tıklatın ve ardından **Show Table Data** ayrıştırıcı sütunun görmek için.</span><span class="sxs-lookup"><span data-stu-id="0e580-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="0e580-195">Aşağıdaki diyagram yeni Okul veritabanının yapısını gösterir:</span><span class="sxs-lookup"><span data-stu-id="0e580-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a><span data-ttu-id="0e580-197">Özet</span><span class="sxs-lookup"><span data-stu-id="0e580-197">Summary</span></span>

<span data-ttu-id="0e580-198">Tablo başına hiyerarşisi devralma şimdi uygulandıktan için `Person`, `Student`, ve `Instructor` sınıfları.</span><span class="sxs-lookup"><span data-stu-id="0e580-198">Table-per-hierarchy inheritance has now been implemented for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="0e580-199">Bu ve diğer devralma yapıları hakkında daha fazla bilgi için bkz: [devralma eşleme stratejileri](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) Morteza Manavi'nın blogunda.</span><span class="sxs-lookup"><span data-stu-id="0e580-199">For more information about this and other inheritance structures, see [Inheritance Mapping Strategies](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) on Morteza Manavi's blog.</span></span> <span data-ttu-id="0e580-200">Sonraki öğreticide depo ve iş desenleri biriminin uygulamak için bazı yollar görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="0e580-200">In the next tutorial you'll see some ways to implement the repository and unit of work patterns.</span></span>

<span data-ttu-id="0e580-201">Diğer Entity Framework kaynaklarına bağlantılar bulunabilir [ASP.NET Data Access içerik haritası](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="0e580-201">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0e580-202">[Önceki](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[sonraki](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="0e580-202">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span></span>
