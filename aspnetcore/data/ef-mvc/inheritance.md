---
title: -Devralma - 9, 10 EF çekirdekli ASP.NET Core MVC
author: rick-anderson
description: Bu öğreticide, Entity Framework Core ASP.NET Core uygulamasını kullanarak veri modelinde, devralma uygulanması gösterilmektedir.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/inheritance
ms.openlocfilehash: a71954297f44f936893a7f1e9d3b0685f81378b9
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126710"
---
# <a name="aspnet-core-mvc-with-ef-core---inheritance---9-of-10"></a><span data-ttu-id="e467b-103">-Devralma - 9, 10 EF çekirdekli ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e467b-103">ASP.NET Core MVC with EF Core - Inheritance - 9 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e467b-104">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e467b-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e467b-105">Contoso University örnek web uygulaması, Entity Framework Core ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="e467b-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="e467b-106">Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](intro.md).</span><span class="sxs-lookup"><span data-stu-id="e467b-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="e467b-107">Önceki öğreticide eşzamanlılık özel durumları işlenir.</span><span class="sxs-lookup"><span data-stu-id="e467b-107">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="e467b-108">Bu öğreticide, veri modelinde devralma uygulanması gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e467b-108">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="e467b-109">Nesne yönelimli programlama, devralma, kod yeniden kullanımını kolaylaştırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e467b-109">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="e467b-110">Bu öğreticide, değiştireceksiniz `Instructor` ve `Student` oldukları türetilmesi sınıflara bir `Person` temel gibi özellikler içeren sınıf `LastName` eğitmenler ve öğrenciler için ortak olan.</span><span class="sxs-lookup"><span data-stu-id="e467b-110">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="e467b-111">Ekleyebilir veya herhangi bir web sayfalarını değiştirmesine olmaz ancak bazı kodları değiştireceksiniz ve bu değişiklikleri veritabanında otomatik olarak yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="e467b-111">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="e467b-112">Veritabanı tabloları devralma eşleme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="e467b-112">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="e467b-113">`Instructor` Ve `Student` sınıflarının Okul veri modelinde aynı olan çeşitli özellikler vardır:</span><span class="sxs-lookup"><span data-stu-id="e467b-113">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Öğrenci ve Eğitmenler sınıfları](inheritance/_static/no-inheritance.png)

<span data-ttu-id="e467b-115">Gereksiz kod tarafından paylaşılan özellikleri için kaldırmak istediğiniz varsayalım `Instructor` ve `Student` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="e467b-115">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="e467b-116">Veya adın bir eğitmen ya da bir öğrenci gelmediğini caring olmadan adları biçimlendirebilen hizmet yazmak istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="e467b-116">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="e467b-117">Oluşturabilirsiniz bir `Person` temel sınıfının yalnızca bu paylaşılan özelliklerini içeren ve ardından olun `Instructor` ve `Student` sınıfları, aşağıdaki çizimde gösterildiği gibi o temel sınıftan devralınan:</span><span class="sxs-lookup"><span data-stu-id="e467b-117">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Kişi sınıftan türetme Öğrenci ve Eğitmenler sınıfları](inheritance/_static/inheritance.png)

<span data-ttu-id="e467b-119">Bu devralma yapı veritabanında gösterilebilir birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="e467b-119">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="e467b-120">Öğrenciler hem de tek bir tabloyu eğitmenlerini hakkında bilgi içeren bir kişi tablo olabilir.</span><span class="sxs-lookup"><span data-stu-id="e467b-120">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="e467b-121">Bazı sütunların Eğitmenler (İşeAlmaTarihi), bazı yalnızca Öğrenciler (EnrollmentDate) bazı hem (Soyadı, FirstName) için yalnızca uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e467b-121">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="e467b-122">Genellikle, her satırı temsil eder. hangi türü belirtmek için bir ayrıştırıcı sütunu gerekir.</span><span class="sxs-lookup"><span data-stu-id="e467b-122">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="e467b-123">Örneğin, ayrıştırıcı sütununu "Eğitmen" eğitmenler ve "Öğrenci" Öğrenciler için olabilir.</span><span class="sxs-lookup"><span data-stu-id="e467b-123">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Tablo başına hiyerarşi örneği](inheritance/_static/tph.png)

<span data-ttu-id="e467b-125">Bu düzen, tek veritabanı tablosundan bir varlık devralma yapısı oluşturma, tablo başına hiyerarşi (TPH) devralma çağrılır.</span><span class="sxs-lookup"><span data-stu-id="e467b-125">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="e467b-126">Devralma yapısı gibi daha veritabanını yapmak için kullanılan bir alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="e467b-126">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="e467b-127">Örneğin, kişi tablosu yalnızca ad alanlarına sahip ve tarih alanları ayrı Eğitmen ve Öğrenci tablolarla sahip.</span><span class="sxs-lookup"><span data-stu-id="e467b-127">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Tablo başına tür devralma](inheritance/_static/tpt.png)

<span data-ttu-id="e467b-129">Bu düzen, her varlık sınıfı için bir veritabanı tablosu oluşturma, tablo başına tür (TPT) devralma çağrılır.</span><span class="sxs-lookup"><span data-stu-id="e467b-129">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="e467b-130">Henüz başka bir seçenek tüm soyut olmayan türler için tek tek tablolar eşlemektir.</span><span class="sxs-lookup"><span data-stu-id="e467b-130">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="e467b-131">Devralınan özellikler dahil olmak üzere, bir sınıfın tüm özellikler için karşılık gelen bir tablonun sütunlarını eşleyin.</span><span class="sxs-lookup"><span data-stu-id="e467b-131">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="e467b-132">Bu düzen (TPC) tablo başına somut sınıf devralma çağrılır.</span><span class="sxs-lookup"><span data-stu-id="e467b-132">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="e467b-133">TPC devralma kişi, Öğrenci ve Eğitmen sınıfları için daha önce gösterildiği gibi uygulanırsa, Öğrenci ve Eğitmenler tabloları farklı Öncekine göre devralma kullanıldıktan sonra görünür.</span><span class="sxs-lookup"><span data-stu-id="e467b-133">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="e467b-134">Karmaşık birleştirme sorgularda TPT desenleri sağladığından TPC ve TPH devralma desenleri genellikle TPT devralma desenleri daha iyi performans sunar.</span><span class="sxs-lookup"><span data-stu-id="e467b-134">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="e467b-135">Bu öğreticide, TPH devralma uygulanması gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e467b-135">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="e467b-136">TPH Entity Framework Core destekleyen tek devralma modelidir.</span><span class="sxs-lookup"><span data-stu-id="e467b-136">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="e467b-137">Ne yaparsınız oluşturmaktır bir `Person` sınıfı, değişiklik `Instructor` ve `Student` sınıfların türetilmesi için `Person`, yeni sınıfa eklemek `DbContext`ve bir geçiş oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e467b-137">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP]
> <span data-ttu-id="e467b-138">Aşağıdaki değişiklikler yapmadan önce bir kopyasını bir projeyi kaydetmeyi tercih edin.</span><span class="sxs-lookup"><span data-stu-id="e467b-138">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="e467b-139">Ardından, sorunları ve baştan gerek çalıştırdığınızda Bu öğretici için gerçekleştirilen adımlar ters veya devam eden yerine kaydedilmiş projeden tekrar başlangıcını tüm dizileri başlatmak daha kolay olacaktır.</span><span class="sxs-lookup"><span data-stu-id="e467b-139">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="e467b-140">Kişi sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="e467b-140">Create the Person class</span></span>

<span data-ttu-id="e467b-141">Modeller klasörü Person.cs oluşturma ve şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e467b-141">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="e467b-142">Öğrenci ve Eğitmenler sınıfları kişiden devral olun</span><span class="sxs-lookup"><span data-stu-id="e467b-142">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="e467b-143">İçinde *Instructor.cs*, kişi Eğitmen sınıf türetin ve anahtar ve ad alanlarını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="e467b-143">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="e467b-144">Kod, aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="e467b-144">The code will look like the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="e467b-145">İçinde aynı değişiklik *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="e467b-145">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="e467b-146">Kişi varlık türü için veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="e467b-146">Add the Person entity type to the data model</span></span>

<span data-ttu-id="e467b-147">Kişi varlık türüne eklemek *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="e467b-147">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="e467b-148">Yeni satırlar vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="e467b-148">The new lines are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="e467b-149">Entity Framework tablo başına hiyerarşi devralmayı yapılandırmak için gereken budur.</span><span class="sxs-lookup"><span data-stu-id="e467b-149">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="e467b-150">Veritabanı güncelleştirildiğinde, göreceğiniz gibi Öğrenci ve Eğitmenler tabloları yerine kişi tabloya sahip olur.</span><span class="sxs-lookup"><span data-stu-id="e467b-150">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="e467b-151">Oluşturma ve geçiş kodu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="e467b-151">Create and customize migration code</span></span>

<span data-ttu-id="e467b-152">Yaptığınız değişiklikleri kaydedin ve projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="e467b-152">Save your changes and build the project.</span></span> <span data-ttu-id="e467b-153">Ardından proje klasöründe komut penceresi açın ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="e467b-153">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="e467b-154">Çalıştırma `database update` henüz komutu.</span><span class="sxs-lookup"><span data-stu-id="e467b-154">Don't run the `database update` command yet.</span></span> <span data-ttu-id="e467b-155">Bu eğitmen tabloyu bırakmak ve kişiye Öğrenci tabloyu yeniden adlandırmak için bu komut, veri kaybına neden olur.</span><span class="sxs-lookup"><span data-stu-id="e467b-155">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="e467b-156">Mevcut verilerinizi korumak için özel kod girmenize gerek.</span><span class="sxs-lookup"><span data-stu-id="e467b-156">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="e467b-157">Açık *geçişleri /\<zaman damgası > _Inheritance.cs* değiştirin `Up` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="e467b-157">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="e467b-158">Bu kod aşağıdaki veritabanı güncelleştirme görevleri üstlenir:</span><span class="sxs-lookup"><span data-stu-id="e467b-158">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="e467b-159">Yabancı anahtar kısıtlamaları ve Öğrenci tabloya noktası dizinleri kaldırır.</span><span class="sxs-lookup"><span data-stu-id="e467b-159">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="e467b-160">Eğitmen tablo kişi olarak yeniden adlandırır ve Öğrenci verileri depolamak gerekli değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="e467b-160">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="e467b-161">Öğrenciler için boş değer atanabilir EnrollmentDate ekler.</span><span class="sxs-lookup"><span data-stu-id="e467b-161">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="e467b-162">Bir satır için bir öğrenci ya da bir eğitmen olup olmadığını belirtmek için ayrıştırıcı sütununu ekler.</span><span class="sxs-lookup"><span data-stu-id="e467b-162">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="e467b-163">Öğrenci satırları seferde olmaz beri İşeAlmaTarihi boş değer atanabilir bir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="e467b-163">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="e467b-164">Öğrenciler için işaret yabancı anahtarlar güncelleştirmek için kullanılan geçici bir alan ekler.</span><span class="sxs-lookup"><span data-stu-id="e467b-164">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="e467b-165">Kişi tabloya Öğrenciler kopyaladığınızda, yeni birincil anahtar değerlerini alırlar.</span><span class="sxs-lookup"><span data-stu-id="e467b-165">When you copy students into the Person table they will get new primary key values.</span></span>

* <span data-ttu-id="e467b-166">Kişi tabloya Öğrenci tablodan veri kopyalar.</span><span class="sxs-lookup"><span data-stu-id="e467b-166">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="e467b-167">Bu, Öğrenciler, yeni birincil anahtar değerlerini atanan neden olur.</span><span class="sxs-lookup"><span data-stu-id="e467b-167">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="e467b-168">Öğrenciler için işaret yabancı anahtar değerlerine düzeltir.</span><span class="sxs-lookup"><span data-stu-id="e467b-168">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="e467b-169">Yabancı anahtar kısıtlamaları ve artık kişi tabloya işaret eden, dizinleri yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e467b-169">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="e467b-170">(GUID yerine tamsayı birincil anahtar türü kullansaydınız, Öğrenci birincil anahtar değerlerini değiştirmek zorunda mıydı ve bu adımların birçok atlandı.)</span><span class="sxs-lookup"><span data-stu-id="e467b-170">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="e467b-171">Çalıştırma `database update` komutu:</span><span class="sxs-lookup"><span data-stu-id="e467b-171">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="e467b-172">(Üretim sisteminde karşılık gelen değişiklikleri yapacağınız `Down` durumda daha önce yaptığı, önceki veritabanı sürümüne geri dönmek için kullanılacak yöntem.</span><span class="sxs-lookup"><span data-stu-id="e467b-172">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="e467b-173">Bu öğretici, olmaz kullanıyor `Down` yöntemi.)</span><span class="sxs-lookup"><span data-stu-id="e467b-173">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE]
> <span data-ttu-id="e467b-174">Diğer hatalar mevcut veriler varsa bir veritabanında şema değişiklik yaparken almak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="e467b-174">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="e467b-175">Gideremezsiniz Geçiş hataları alırsanız, bağlantı dizesi içinde veritabanı adını değiştirin veya veritabanını silin.</span><span class="sxs-lookup"><span data-stu-id="e467b-175">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="e467b-176">Yeni bir veritabanı geçirmek için veri yok ve veritabanını güncelleştir komut hatasız tamamlanması daha olasıdır.</span><span class="sxs-lookup"><span data-stu-id="e467b-176">With a new database, there's no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="e467b-177">Veritabanını silmek için SSOX kullandığınızda veya `database drop` CLI komutu.</span><span class="sxs-lookup"><span data-stu-id="e467b-177">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="e467b-178">Uygulanan devralma ile test</span><span class="sxs-lookup"><span data-stu-id="e467b-178">Test with inheritance implemented</span></span>

<span data-ttu-id="e467b-179">Uygulamayı çalıştırın ve çeşitli sayfalar deneyin.</span><span class="sxs-lookup"><span data-stu-id="e467b-179">Run the app and try various pages.</span></span> <span data-ttu-id="e467b-180">Önce yaptığınız gibi her şey aynı çalışır.</span><span class="sxs-lookup"><span data-stu-id="e467b-180">Everything works the same as it did before.</span></span>

<span data-ttu-id="e467b-181">İçinde **SQL Server Nesne Gezgini**, genişletin **veri bağlantıları/SchoolContext** ardından **tabloları**, ve Öğrenci ve Eğitmenler tabloları almıştır gördüğünüz bir Kişi tablo.</span><span class="sxs-lookup"><span data-stu-id="e467b-181">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="e467b-182">Kişi Tablo Tasarımcısı'nı açın ve tüm Öğrenci ve Eğitmenler tablolarında kullanılabilir sütunların olduğunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="e467b-182">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Kişi SSOX tablosunda](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="e467b-184">Kişi tabloya sağ tıklayıp ardından **tablo verilerini Göster** ayrıştırıcı sütununu görmek için.</span><span class="sxs-lookup"><span data-stu-id="e467b-184">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Kişi tablosunda SSOX - tablo verileri](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="e467b-186">Özet</span><span class="sxs-lookup"><span data-stu-id="e467b-186">Summary</span></span>

<span data-ttu-id="e467b-187">Tablo başına hiyerarşi devralma için uyguladık `Person`, `Student`, ve `Instructor` sınıfları.</span><span class="sxs-lookup"><span data-stu-id="e467b-187">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="e467b-188">Entity Framework Core içinde devralma hakkında daha fazla bilgi için bkz. [devralma](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="e467b-188">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="e467b-189">Sonraki öğreticide, bir göreceli olarak Gelişmiş Entity Framework senaryoları işlemek nasıl görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="e467b-189">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="e467b-190">[Önceki](concurrency.md)
> [İleri](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="e467b-190">[Previous](concurrency.md)
[Next](advanced.md)</span></span>
