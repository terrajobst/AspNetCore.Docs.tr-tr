---
title: 'Öğretici: Devralma - EF çekirdekli ASP.NET MVC uygulama'
description: Bu öğreticide, Entity Framework Core ASP.NET Core uygulamasını kullanarak veri modelinde, devralma uygulanması gösterilmektedir.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/inheritance
ms.openlocfilehash: ba688b259206f21045376b6a38a266ad7528aec1
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750773"
---
# <a name="tutorial-implement-inheritance---aspnet-mvc-with-ef-core"></a><span data-ttu-id="390f9-103">Öğretici: Devralma - EF çekirdekli ASP.NET MVC uygulama</span><span class="sxs-lookup"><span data-stu-id="390f9-103">Tutorial: Implement inheritance - ASP.NET MVC with EF Core</span></span>

<span data-ttu-id="390f9-104">Önceki öğreticide eşzamanlılık özel durumları işlenir.</span><span class="sxs-lookup"><span data-stu-id="390f9-104">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="390f9-105">Bu öğreticide, veri modelinde devralma uygulanması gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="390f9-105">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="390f9-106">Nesne yönelimli programlama, devralma, kod yeniden kullanımını kolaylaştırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="390f9-106">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="390f9-107">Bu öğreticide, değiştireceksiniz `Instructor` ve `Student` oldukları türetilmesi sınıflara bir `Person` temel gibi özellikler içeren sınıf `LastName` eğitmenler ve öğrenciler için ortak olan.</span><span class="sxs-lookup"><span data-stu-id="390f9-107">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="390f9-108">Ekleyebilir veya herhangi bir web sayfalarını değiştirmesine olmaz ancak bazı kodları değiştireceksiniz ve bu değişiklikleri veritabanında otomatik olarak yansıtılır.</span><span class="sxs-lookup"><span data-stu-id="390f9-108">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

<span data-ttu-id="390f9-109">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="390f9-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="390f9-110">Devralma için veritabanı eşleme</span><span class="sxs-lookup"><span data-stu-id="390f9-110">Map inheritance to database</span></span>
> * <span data-ttu-id="390f9-111">Kişi sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="390f9-111">Create the Person class</span></span>
> * <span data-ttu-id="390f9-112">Güncelleştirme Eğitmen ve Öğrenci</span><span class="sxs-lookup"><span data-stu-id="390f9-112">Update Instructor and Student</span></span>
> * <span data-ttu-id="390f9-113">Modele Kişi Ekle</span><span class="sxs-lookup"><span data-stu-id="390f9-113">Add Person to the model</span></span>
> * <span data-ttu-id="390f9-114">Oluşturma ve geçişler güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="390f9-114">Create and update migrations</span></span>
> * <span data-ttu-id="390f9-115">Uygulama testi</span><span class="sxs-lookup"><span data-stu-id="390f9-115">Test the implementation</span></span>

## <a name="prerequisites"></a><span data-ttu-id="390f9-116">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="390f9-116">Prerequisites</span></span>

* [<span data-ttu-id="390f9-117">Eşzamanlılık işleme</span><span class="sxs-lookup"><span data-stu-id="390f9-117">Handle Concurrency</span></span>](concurrency.md)

## <a name="map-inheritance-to-database"></a><span data-ttu-id="390f9-118">Devralma için veritabanı eşleme</span><span class="sxs-lookup"><span data-stu-id="390f9-118">Map inheritance to database</span></span>

<span data-ttu-id="390f9-119">`Instructor` Ve `Student` sınıflarının Okul veri modelinde aynı olan çeşitli özellikler vardır:</span><span class="sxs-lookup"><span data-stu-id="390f9-119">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Öğrenci ve Eğitmenler sınıfları](inheritance/_static/no-inheritance.png)

<span data-ttu-id="390f9-121">Gereksiz kod tarafından paylaşılan özellikleri için kaldırmak istediğiniz varsayalım `Instructor` ve `Student` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="390f9-121">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="390f9-122">Veya adın bir eğitmen ya da bir öğrenci gelmediğini caring olmadan adları biçimlendirebilen hizmet yazmak istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="390f9-122">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="390f9-123">Oluşturabilirsiniz bir `Person` temel sınıfının yalnızca bu paylaşılan özelliklerini içeren ve ardından olun `Instructor` ve `Student` sınıfları, aşağıdaki çizimde gösterildiği gibi o temel sınıftan devralınan:</span><span class="sxs-lookup"><span data-stu-id="390f9-123">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Kişi sınıftan türetme Öğrenci ve Eğitmenler sınıfları](inheritance/_static/inheritance.png)

<span data-ttu-id="390f9-125">Bu devralma yapı veritabanında gösterilebilir birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="390f9-125">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="390f9-126">Öğrenciler hem de tek bir tabloyu eğitmenlerini hakkında bilgi içeren bir kişi tablo olabilir.</span><span class="sxs-lookup"><span data-stu-id="390f9-126">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="390f9-127">Bazı sütunların Eğitmenler (İşeAlmaTarihi), bazı yalnızca Öğrenciler (EnrollmentDate) bazı hem (Soyadı, FirstName) için yalnızca uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="390f9-127">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="390f9-128">Genellikle, her satırı temsil eder. hangi türü belirtmek için bir ayrıştırıcı sütunu gerekir.</span><span class="sxs-lookup"><span data-stu-id="390f9-128">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="390f9-129">Örneğin, ayrıştırıcı sütununu "Eğitmen" eğitmenler ve "Öğrenci" Öğrenciler için olabilir.</span><span class="sxs-lookup"><span data-stu-id="390f9-129">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Tablo başına hiyerarşi örneği](inheritance/_static/tph.png)

<span data-ttu-id="390f9-131">Bu düzen, tek veritabanı tablosundan bir varlık devralma yapısı oluşturma, tablo başına hiyerarşi (TPH) devralma çağrılır.</span><span class="sxs-lookup"><span data-stu-id="390f9-131">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="390f9-132">Devralma yapısı gibi daha veritabanını yapmak için kullanılan bir alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="390f9-132">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="390f9-133">Örneğin, kişi tablosu yalnızca ad alanlarına sahip ve tarih alanları ayrı Eğitmen ve Öğrenci tablolarla sahip.</span><span class="sxs-lookup"><span data-stu-id="390f9-133">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Tablo başına tür devralma](inheritance/_static/tpt.png)

<span data-ttu-id="390f9-135">Bu düzen, her varlık sınıfı için bir veritabanı tablosu oluşturma, tablo başına tür (TPT) devralma çağrılır.</span><span class="sxs-lookup"><span data-stu-id="390f9-135">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="390f9-136">Henüz başka bir seçenek tüm soyut olmayan türler için tek tek tablolar eşlemektir.</span><span class="sxs-lookup"><span data-stu-id="390f9-136">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="390f9-137">Devralınan özellikler dahil olmak üzere, bir sınıfın tüm özellikler için karşılık gelen bir tablonun sütunlarını eşleyin.</span><span class="sxs-lookup"><span data-stu-id="390f9-137">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="390f9-138">Bu düzen (TPC) tablo başına somut sınıf devralma çağrılır.</span><span class="sxs-lookup"><span data-stu-id="390f9-138">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="390f9-139">TPC devralma kişi, Öğrenci ve Eğitmen sınıfları için daha önce gösterildiği gibi uygulanırsa, Öğrenci ve Eğitmenler tabloları farklı Öncekine göre devralma kullanıldıktan sonra görünür.</span><span class="sxs-lookup"><span data-stu-id="390f9-139">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="390f9-140">Karmaşık birleştirme sorgularda TPT desenleri sağladığından TPC ve TPH devralma desenleri genellikle TPT devralma desenleri daha iyi performans sunar.</span><span class="sxs-lookup"><span data-stu-id="390f9-140">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="390f9-141">Bu öğreticide, TPH devralma uygulanması gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="390f9-141">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="390f9-142">TPH Entity Framework Core destekleyen tek devralma modelidir.</span><span class="sxs-lookup"><span data-stu-id="390f9-142">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="390f9-143">Ne yaparsınız oluşturmaktır bir `Person` sınıfı, değişiklik `Instructor` ve `Student` sınıfların türetilmesi için `Person`, yeni sınıfa eklemek `DbContext`ve bir geçiş oluşturun.</span><span class="sxs-lookup"><span data-stu-id="390f9-143">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP]
> <span data-ttu-id="390f9-144">Aşağıdaki değişiklikler yapmadan önce bir kopyasını bir projeyi kaydetmeyi tercih edin.</span><span class="sxs-lookup"><span data-stu-id="390f9-144">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="390f9-145">Ardından, sorunları ve baştan gerek çalıştırdığınızda Bu öğretici için gerçekleştirilen adımlar ters veya devam eden yerine kaydedilmiş projeden tekrar başlangıcını tüm dizileri başlatmak daha kolay olacaktır.</span><span class="sxs-lookup"><span data-stu-id="390f9-145">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="390f9-146">Kişi sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="390f9-146">Create the Person class</span></span>

<span data-ttu-id="390f9-147">Modeller klasörü Person.cs oluşturma ve şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="390f9-147">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="update-instructor-and-student"></a><span data-ttu-id="390f9-148">Güncelleştirme Eğitmen ve Öğrenci</span><span class="sxs-lookup"><span data-stu-id="390f9-148">Update Instructor and Student</span></span>

<span data-ttu-id="390f9-149">İçinde *Instructor.cs*, kişi Eğitmen sınıf türetin ve anahtar ve ad alanlarını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="390f9-149">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="390f9-150">Kod, aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="390f9-150">The code will look like the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="390f9-151">İçinde aynı değişiklik *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="390f9-151">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-person-to-the-model"></a><span data-ttu-id="390f9-152">Modele Kişi Ekle</span><span class="sxs-lookup"><span data-stu-id="390f9-152">Add Person to the model</span></span>

<span data-ttu-id="390f9-153">Kişi varlık türüne eklemek *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="390f9-153">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="390f9-154">Yeni satırlar vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="390f9-154">The new lines are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="390f9-155">Entity Framework tablo başına hiyerarşi devralmayı yapılandırmak için gereken budur.</span><span class="sxs-lookup"><span data-stu-id="390f9-155">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="390f9-156">Veritabanı güncelleştirildiğinde, göreceğiniz gibi Öğrenci ve Eğitmenler tabloları yerine kişi tabloya sahip olur.</span><span class="sxs-lookup"><span data-stu-id="390f9-156">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-update-migrations"></a><span data-ttu-id="390f9-157">Oluşturma ve geçişler güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="390f9-157">Create and update migrations</span></span>

<span data-ttu-id="390f9-158">Yaptığınız değişiklikleri kaydedin ve projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="390f9-158">Save your changes and build the project.</span></span> <span data-ttu-id="390f9-159">Ardından proje klasöründe komut penceresi açın ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="390f9-159">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="390f9-160">Çalıştırma `database update` henüz komutu.</span><span class="sxs-lookup"><span data-stu-id="390f9-160">Don't run the `database update` command yet.</span></span> <span data-ttu-id="390f9-161">Bu eğitmen tabloyu bırakmak ve kişiye Öğrenci tabloyu yeniden adlandırmak için bu komut, veri kaybına neden olur.</span><span class="sxs-lookup"><span data-stu-id="390f9-161">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="390f9-162">Mevcut verilerinizi korumak için özel kod girmenize gerek.</span><span class="sxs-lookup"><span data-stu-id="390f9-162">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="390f9-163">Açık *geçişleri /\<zaman damgası > _Inheritance.cs* değiştirin `Up` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="390f9-163">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="390f9-164">Bu kod aşağıdaki veritabanı güncelleştirme görevleri üstlenir:</span><span class="sxs-lookup"><span data-stu-id="390f9-164">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="390f9-165">Yabancı anahtar kısıtlamaları ve Öğrenci tabloya noktası dizinleri kaldırır.</span><span class="sxs-lookup"><span data-stu-id="390f9-165">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="390f9-166">Eğitmen tablo kişi olarak yeniden adlandırır ve Öğrenci verileri depolamak gerekli değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="390f9-166">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="390f9-167">Öğrenciler için boş değer atanabilir EnrollmentDate ekler.</span><span class="sxs-lookup"><span data-stu-id="390f9-167">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="390f9-168">Bir satır için bir öğrenci ya da bir eğitmen olup olmadığını belirtmek için ayrıştırıcı sütununu ekler.</span><span class="sxs-lookup"><span data-stu-id="390f9-168">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="390f9-169">Öğrenci satırları seferde olmaz beri İşeAlmaTarihi boş değer atanabilir bir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="390f9-169">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="390f9-170">Öğrenciler için işaret yabancı anahtarlar güncelleştirmek için kullanılan geçici bir alan ekler.</span><span class="sxs-lookup"><span data-stu-id="390f9-170">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="390f9-171">Kişi tabloya Öğrenciler kopyaladığınızda, yeni birincil anahtar değerlerini alırlar.</span><span class="sxs-lookup"><span data-stu-id="390f9-171">When you copy students into the Person table they will get new primary key values.</span></span>

* <span data-ttu-id="390f9-172">Kişi tabloya Öğrenci tablodan veri kopyalar.</span><span class="sxs-lookup"><span data-stu-id="390f9-172">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="390f9-173">Bu, Öğrenciler, yeni birincil anahtar değerlerini atanan neden olur.</span><span class="sxs-lookup"><span data-stu-id="390f9-173">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="390f9-174">Öğrenciler için işaret yabancı anahtar değerlerine düzeltir.</span><span class="sxs-lookup"><span data-stu-id="390f9-174">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="390f9-175">Yabancı anahtar kısıtlamaları ve artık kişi tabloya işaret eden, dizinleri yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="390f9-175">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="390f9-176">(GUID yerine tamsayı birincil anahtar türü kullansaydınız, Öğrenci birincil anahtar değerlerini değiştirmek zorunda mıydı ve bu adımların birçok atlandı.)</span><span class="sxs-lookup"><span data-stu-id="390f9-176">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="390f9-177">Çalıştırma `database update` komutu:</span><span class="sxs-lookup"><span data-stu-id="390f9-177">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="390f9-178">(Üretim sisteminde karşılık gelen değişiklikleri yapacağınız `Down` durumda daha önce yaptığı, önceki veritabanı sürümüne geri dönmek için kullanılacak yöntem.</span><span class="sxs-lookup"><span data-stu-id="390f9-178">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="390f9-179">Bu öğretici, olmaz kullanıyor `Down` yöntemi.)</span><span class="sxs-lookup"><span data-stu-id="390f9-179">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE]
> <span data-ttu-id="390f9-180">Diğer hatalar mevcut veriler varsa bir veritabanında şema değişiklik yaparken almak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="390f9-180">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="390f9-181">Gideremezsiniz Geçiş hataları alırsanız, bağlantı dizesi içinde veritabanı adını değiştirin veya veritabanını silin.</span><span class="sxs-lookup"><span data-stu-id="390f9-181">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="390f9-182">Yeni bir veritabanı geçirmek için veri yok ve veritabanını güncelleştir komut hatasız tamamlanması daha olasıdır.</span><span class="sxs-lookup"><span data-stu-id="390f9-182">With a new database, there's no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="390f9-183">Veritabanını silmek için SSOX kullandığınızda veya `database drop` CLI komutu.</span><span class="sxs-lookup"><span data-stu-id="390f9-183">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-the-implementation"></a><span data-ttu-id="390f9-184">Uygulama testi</span><span class="sxs-lookup"><span data-stu-id="390f9-184">Test the implementation</span></span>

<span data-ttu-id="390f9-185">Uygulamayı çalıştırın ve çeşitli sayfalar deneyin.</span><span class="sxs-lookup"><span data-stu-id="390f9-185">Run the app and try various pages.</span></span> <span data-ttu-id="390f9-186">Önce yaptığınız gibi her şey aynı çalışır.</span><span class="sxs-lookup"><span data-stu-id="390f9-186">Everything works the same as it did before.</span></span>

<span data-ttu-id="390f9-187">İçinde **SQL Server Nesne Gezgini**, genişletin **veri bağlantıları/SchoolContext** ardından **tabloları**, ve Öğrenci ve Eğitmenler tabloları almıştır gördüğünüz bir Kişi tablo.</span><span class="sxs-lookup"><span data-stu-id="390f9-187">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="390f9-188">Kişi Tablo Tasarımcısı'nı açın ve tüm Öğrenci ve Eğitmenler tablolarında kullanılabilir sütunların olduğunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="390f9-188">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Kişi SSOX tablosunda](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="390f9-190">Kişi tabloya sağ tıklayıp ardından **tablo verilerini Göster** ayrıştırıcı sütununu görmek için.</span><span class="sxs-lookup"><span data-stu-id="390f9-190">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Kişi tablosunda SSOX - tablo verileri](inheritance/_static/ssox-person-data.png)

## <a name="get-the-code"></a><span data-ttu-id="390f9-192">Kodu alma</span><span class="sxs-lookup"><span data-stu-id="390f9-192">Get the code</span></span>

[<span data-ttu-id="390f9-193">İndirme veya tamamlanmış uygulamanın görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="390f9-193">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="additional-resources"></a><span data-ttu-id="390f9-194">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="390f9-194">Additional resources</span></span>

<span data-ttu-id="390f9-195">Entity Framework Core içinde devralma hakkında daha fazla bilgi için bkz. [devralma](/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="390f9-195">For more information about inheritance in Entity Framework Core, see [Inheritance](/ef/core/modeling/inheritance).</span></span>

## <a name="next-steps"></a><span data-ttu-id="390f9-196">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="390f9-196">Next steps</span></span>

<span data-ttu-id="390f9-197">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="390f9-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="390f9-198">Veritabanı için eşlenen devralma</span><span class="sxs-lookup"><span data-stu-id="390f9-198">Mapped inheritance to database</span></span>
> * <span data-ttu-id="390f9-199">Kişi Sınıf oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="390f9-199">Created the Person class</span></span>
> * <span data-ttu-id="390f9-200">Güncelleştirilmiş bir eğitmen ve Öğrenci</span><span class="sxs-lookup"><span data-stu-id="390f9-200">Updated Instructor and Student</span></span>
> * <span data-ttu-id="390f9-201">Eklenen kişi modeli</span><span class="sxs-lookup"><span data-stu-id="390f9-201">Added Person to the model</span></span>
> * <span data-ttu-id="390f9-202">Oluşturulan ve güncelleştirme geçişleri</span><span class="sxs-lookup"><span data-stu-id="390f9-202">Created and update migrations</span></span>
> * <span data-ttu-id="390f9-203">Test uygulaması</span><span class="sxs-lookup"><span data-stu-id="390f9-203">Tested the implementation</span></span>

<span data-ttu-id="390f9-204">Çeşitli oldukça gelişmiş Entity Framework senaryoları ele öğrenmek için sonraki öğreticiye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="390f9-204">Advance to the next tutorial to learn how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="390f9-205">Sonraki: Gelişmiş konular</span><span class="sxs-lookup"><span data-stu-id="390f9-205">Next: Advanced topics</span></span>](advanced.md)
