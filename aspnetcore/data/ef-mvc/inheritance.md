---
title: "EF çekirdek - devralma - 9, 10 ile ASP.NET Core MVC"
author: tdykstra
description: "Bu öğretici ASP.NET Core uygulamada Entity Framework Çekirdek kullanarak veri modelindeki devralma uygulamak nasıl yapacağınızı gösterir."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 55221846422def25452bc148b68573a02299bbfe
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a><span data-ttu-id="a7138-103">Devralma - EF çekirdek ASP.NET Core MVC Öğreticisi (9, 10)</span><span class="sxs-lookup"><span data-stu-id="a7138-103">Inheritance - EF Core with ASP.NET Core MVC tutorial (9 of 10)</span></span>

<span data-ttu-id="a7138-104">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a7138-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a7138-105">Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a7138-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="a7138-106">Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).</span><span class="sxs-lookup"><span data-stu-id="a7138-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="a7138-107">Önceki öğreticide eşzamanlılık işlenir.</span><span class="sxs-lookup"><span data-stu-id="a7138-107">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="a7138-108">Bu öğreticide veri modelinde devralma uygulamak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="a7138-108">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="a7138-109">Nesne odaklı programlama kodu yeniden kullanma kolaylaştırmak için devralma kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7138-109">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="a7138-110">Bu öğreticide, değiştireceğiz `Instructor` ve `Student` öğesinden türetilen sınıflara bir `Person` temel gibi özellikler içeren sınıfı `LastName` Eğitmen ve öğrenciler için ortak olan.</span><span class="sxs-lookup"><span data-stu-id="a7138-110">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="a7138-111">Ekleme veya tüm web sayfalarını değiştirme olmaz ancak bazı kodları değiştireceksiniz ve bu değişiklikleri otomatik olarak veritabanında yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="a7138-111">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="a7138-112">Veritabanı tablolarında devralma eşleme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="a7138-112">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="a7138-113">`Instructor` Ve `Student` sınıfları Okul veri modelindeki özdeş çeşitli özellikleri vardır:</span><span class="sxs-lookup"><span data-stu-id="a7138-113">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Öğrenci ve Eğitmen sınıfları](inheritance/_static/no-inheritance.png)

<span data-ttu-id="a7138-115">Tarafından paylaşılan özellikler için yedek kod ortadan kaldırmak istediğinizi varsayalım `Instructor` ve `Student` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="a7138-115">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="a7138-116">Veya adları adı bir eğitmen veya öğrencinin gelmediğini caring olmadan biçimlendirebilirsiniz bir hizmet yazmak istiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="a7138-116">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="a7138-117">Oluşturabilirsiniz bir `Person` temel yalnızca bu özellikleri paylaşılan içeren sınıfı sonra olun `Instructor` ve `Student` sınıfları aşağıdaki çizimde gösterildiği gibi taban sınıfından devralan:</span><span class="sxs-lookup"><span data-stu-id="a7138-117">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Kişi sınıfından türetilen Öğrenci ve Eğitmen sınıfları](inheritance/_static/inheritance.png)

<span data-ttu-id="a7138-119">Bu devralma yapı veritabanında gösterilebilir birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="a7138-119">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="a7138-120">Öğrenciler ve tek bir tablodaki Eğitmen hakkında bilgi içeren bir kişinin Tablo olabilir.</span><span class="sxs-lookup"><span data-stu-id="a7138-120">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="a7138-121">Bazı sütunları yalnızca Eğitmen (İşeAlmaTarihi), bazı yalnızca Öğrenciler (EnrollmentDate), bazı hem (Soyadı, adı) için geçerli olabilir.</span><span class="sxs-lookup"><span data-stu-id="a7138-121">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="a7138-122">Genellikle, her satır temsil eden tür belirtmek için ayrıştırıcı sütunun gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7138-122">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="a7138-123">Örneğin, ayrıştırıcı sütunun "Eğitmen" Eğitmen ve "Öğrenci" Öğrenciler için olabilir.</span><span class="sxs-lookup"><span data-stu-id="a7138-123">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Tablo başına hiyerarşisi örneği](inheritance/_static/tph.png)

<span data-ttu-id="a7138-125">Bir varlığın devralma yapısı tek veritabanı tablosundan oluşturmanın bu deseni tablo başına hiyerarşisi (TPH) devralma adı verilir.</span><span class="sxs-lookup"><span data-stu-id="a7138-125">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="a7138-126">Devralma yapısı gibi daha fazla ara veritabanını yapmak için kullanılan bir alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="a7138-126">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="a7138-127">Örneğin, kişi tablosu yalnızca ad alanlarına sahip ve tarih alanları ayrı Eğitmen ve Öğrenci tablolarla sahip.</span><span class="sxs-lookup"><span data-stu-id="a7138-127">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Tür başına Tablo Devralma](inheritance/_static/tpt.png)

<span data-ttu-id="a7138-129">Bu desen her varlık sınıfı için bir veritabanı tablosu hale getirme türü (birleştirilmiş TPT) devralma her tablo adı verilir.</span><span class="sxs-lookup"><span data-stu-id="a7138-129">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="a7138-130">Henüz başka bir tek tek tablolar için tüm Özet olmayan türlerine eşlemek için bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="a7138-130">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="a7138-131">Devralınan özellikler de dahil olmak üzere, bir sınıfın tüm özellikleri ilgili tablo sütunlarına eşleyin.</span><span class="sxs-lookup"><span data-stu-id="a7138-131">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="a7138-132">Bu desen tablo başına somut sınıfı (TPC) devralma adı verilir.</span><span class="sxs-lookup"><span data-stu-id="a7138-132">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="a7138-133">Daha önce gösterildiği gibi kişi, Öğrenci ve Eğitmen sınıfları için TPC devralma uygulanırsa Öğrenci ve Eğitmen tablolarını farklı Öncekine göre devralma kullanıldıktan sonra görünür.</span><span class="sxs-lookup"><span data-stu-id="a7138-133">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="a7138-134">Birleştirilmiş TPT desenleri karmaşık JOIN sorguları sağladığından TPC ve TPH devralma desenleri genellikle birleştirilmiş TPT devralma desenleri, daha iyi performans sunar.</span><span class="sxs-lookup"><span data-stu-id="a7138-134">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="a7138-135">Bu öğretici nasıl TPH devralma uygulanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a7138-135">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="a7138-136">TPH Entity Framework Çekirdek destekleyen tek devralma deseni ortaya çıkar.</span><span class="sxs-lookup"><span data-stu-id="a7138-136">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="a7138-137">Ne siz gerçekleştirirsiniz oluşturmaktır bir `Person` sınıfı, değişiklik `Instructor` ve `Student` öğesinden türetilen sınıflar `Person`, yeni sınıf ekleyin `DbContext`ve bir geçiş oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a7138-137">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP] 
> <span data-ttu-id="a7138-138">Aşağıdaki değişiklik yapmadan önce bir kopyasını proje kaydetme göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="a7138-138">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="a7138-139">Ardından, sorunları ve baştan başlamak gerek alıyorsanız, Bu öğretici için yapılan adımları ters çevirme veya devam eden yerine kaydedilmiş projeden geri tüm dizileri başlangıcına başlatmak daha kolay olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a7138-139">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="a7138-140">Kişi sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="a7138-140">Create the Person class</span></span>

<span data-ttu-id="a7138-141">Modeller klasörü Person.cs oluşturun ve şablon kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a7138-141">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="a7138-142">Kişiden devral Öğrenci ve Eğitmen sınıflarının</span><span class="sxs-lookup"><span data-stu-id="a7138-142">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="a7138-143">İçinde *Instructor.cs*, kişi sınıfından Eğitmen sınıf türetin ve anahtar ve ad alanlarını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="a7138-143">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="a7138-144">Kod aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="a7138-144">The code will look like the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="a7138-145">İçinde aynı değişiklik *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="a7138-145">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="a7138-146">Kişi varlık türü veri modeline Ekle</span><span class="sxs-lookup"><span data-stu-id="a7138-146">Add the Person entity type to the data model</span></span>

<span data-ttu-id="a7138-147">Kişi varlık türüne ekleme *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="a7138-147">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="a7138-148">Yeni satırlar vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="a7138-148">The new lines are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="a7138-149">Entity Framework tablo başına hiyerarşisi devralma yapılandırmak için gereken budur.</span><span class="sxs-lookup"><span data-stu-id="a7138-149">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="a7138-150">Veritabanı güncelleştirildiğinde, göreceğiniz gibi bir kişinin Tablo Öğrenci ve Eğitmen tabloları yerine sahip olur.</span><span class="sxs-lookup"><span data-stu-id="a7138-150">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="a7138-151">Oluşturma ve geçiş kodu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="a7138-151">Create and customize migration code</span></span>

<span data-ttu-id="a7138-152">Yaptığınız değişiklikleri kaydedin ve projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a7138-152">Save your changes and build the project.</span></span> <span data-ttu-id="a7138-153">Sonra proje klasöründe komut penceresi açın ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="a7138-153">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="a7138-154">Çalıştırma `database update` henüz komutu.</span><span class="sxs-lookup"><span data-stu-id="a7138-154">Don't run the `database update` command yet.</span></span> <span data-ttu-id="a7138-155">Bu komut, eğitmen tablo bırakma ve Öğrenci tablosunu kişiye yeniden adlandırmak için veri kaybına neden olur.</span><span class="sxs-lookup"><span data-stu-id="a7138-155">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="a7138-156">Mevcut verilerinizi korumak için özel kod sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7138-156">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="a7138-157">Açık *geçişleri /\<zaman damgası > _Inheritance.cs* ve değiştirme `Up` aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a7138-157">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="a7138-158">Bu kod, aşağıdaki veritabanı güncelleştirme görevleri mvc'deki:</span><span class="sxs-lookup"><span data-stu-id="a7138-158">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="a7138-159">Yabancı anahtar kısıtlamaları ve Öğrenci tabloya noktası dizinleri kaldırır.</span><span class="sxs-lookup"><span data-stu-id="a7138-159">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="a7138-160">Eğitmen tablo kişi olarak yeniden adlandırır ve Öğrenci verileri depolamak gerektiğinde değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="a7138-160">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="a7138-161">Öğrenciler için boş değer atanabilir EnrollmentDate ekler.</span><span class="sxs-lookup"><span data-stu-id="a7138-161">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="a7138-162">Bir satır öğrencinin veya bir eğitmen olup olmadığını belirtmek için ayrıştırıcı sütunun ekler.</span><span class="sxs-lookup"><span data-stu-id="a7138-162">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="a7138-163">Öğrenci satırları seferde olmayacaktır beri İşeAlmaTarihi null atanamaz hale getirir.</span><span class="sxs-lookup"><span data-stu-id="a7138-163">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="a7138-164">Öğrenciler için noktası yabancı anahtarlar güncelleştirmek için kullanılan geçici bir alan ekler.</span><span class="sxs-lookup"><span data-stu-id="a7138-164">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="a7138-165">Kişi tablosuna Öğrenciler kopyaladığınızda, yeni birincil anahtar değerlerinin alır.</span><span class="sxs-lookup"><span data-stu-id="a7138-165">When you copy students into the Person table they will get new primary key values.</span></span>

* <span data-ttu-id="a7138-166">Kişi tabloya Öğrenci tablodan veri kopyalar.</span><span class="sxs-lookup"><span data-stu-id="a7138-166">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="a7138-167">Bu, yeni birincil anahtar değerlerinin atanan Öğrenciler neden olur.</span><span class="sxs-lookup"><span data-stu-id="a7138-167">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="a7138-168">Öğrenciler için noktası yabancı anahtar değerleri giderir.</span><span class="sxs-lookup"><span data-stu-id="a7138-168">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="a7138-169">Yabancı anahtar kısıtlamaları ve şimdi kişi tabloya işaret eden dizinleri yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a7138-169">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="a7138-170">(Birincil anahtar türü olarak tamsayı yerine GUID kullandıysanız, Öğrenci birincil anahtar değerlerinin değiştirmek zorunda olmayacaktır ve bu adımların çoğunun atlandı.)</span><span class="sxs-lookup"><span data-stu-id="a7138-170">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="a7138-171">Çalıştırma `database update` komutu:</span><span class="sxs-lookup"><span data-stu-id="a7138-171">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="a7138-172">(Bir üretim sisteminde karşılık gelen değişiklikler yapacağınız `Down` yöntemi şimdiye kadar olan, önceki veritabanı sürümüne geri dönmek için kullanılacak durumda.</span><span class="sxs-lookup"><span data-stu-id="a7138-172">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="a7138-173">Bu öğretici, olmaz kullanıyor `Down` yöntemi.)</span><span class="sxs-lookup"><span data-stu-id="a7138-173">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE] 
> <span data-ttu-id="a7138-174">Şema değişiklikleri var olan verileri içeren bir veritabanına yaparken diğer hatalarıyla mümkündür.</span><span class="sxs-lookup"><span data-stu-id="a7138-174">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="a7138-175">Çözümlenemiyor Geçiş hataları alırsanız, bağlantı dizesindeki veritabanı adını değiştirin veya veritabanını silin.</span><span class="sxs-lookup"><span data-stu-id="a7138-175">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="a7138-176">Yeni bir veritabanı ile geçirmek için veri yok ve update-database komutunu hatasız tamamlamak daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="a7138-176">With a new database, there's no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="a7138-177">Veritabanını silmek için SSOX kullanın veya çalıştırın `database drop` CLI komutu.</span><span class="sxs-lookup"><span data-stu-id="a7138-177">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="a7138-178">Uygulanan devralma ile test</span><span class="sxs-lookup"><span data-stu-id="a7138-178">Test with inheritance implemented</span></span>

<span data-ttu-id="a7138-179">Uygulamayı çalıştırın ve çeşitli sayfalar deneyin.</span><span class="sxs-lookup"><span data-stu-id="a7138-179">Run the app and try various pages.</span></span> <span data-ttu-id="a7138-180">Önce yaptığınız gibi her şey aynı çalışır.</span><span class="sxs-lookup"><span data-stu-id="a7138-180">Everything works the same as it did before.</span></span>

<span data-ttu-id="a7138-181">İçinde **SQL Server Nesne Gezgini**, genişletin **veri bağlantıları/SchoolContext** ve ardından **tabloları**, ve Öğrenci ve Eğitmen tabloları almıştır gördüğünüz bir Kişi tablo.</span><span class="sxs-lookup"><span data-stu-id="a7138-181">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="a7138-182">Kişi Tablo Tasarımcısı'nı açın ve tüm Öğrenci ve Eğitmen tablolarında olarak kullanılan sütunlar olduğunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="a7138-182">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![SSOX Kişi tablosunda](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="a7138-184">Kişi tabloyu sağ tıklatın ve ardından **Show Table Data** ayrıştırıcı sütunun görmek için.</span><span class="sxs-lookup"><span data-stu-id="a7138-184">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Kişi tablosunda SSOX - tablo verileri](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="a7138-186">Özet</span><span class="sxs-lookup"><span data-stu-id="a7138-186">Summary</span></span>

<span data-ttu-id="a7138-187">Tablo başına hiyerarşisi devralma uyguladık `Person`, `Student`, ve `Instructor` sınıfları.</span><span class="sxs-lookup"><span data-stu-id="a7138-187">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="a7138-188">Entity Framework Çekirdek devralma hakkında daha fazla bilgi için bkz: [devralma](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="a7138-188">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="a7138-189">Sonraki öğreticide çeşitli göreceli olarak Gelişmiş Entity Framework senaryolarda nasıl ele alınacağını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="a7138-189">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a7138-190">[Önceki](concurrency.md)
[sonraki](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="a7138-190">[Previous](concurrency.md)
[Next](advanced.md)</span></span>  
