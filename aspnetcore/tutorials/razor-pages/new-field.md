---
title: ASP.NET Core Razor sayfasına yeni bir alan ekleyin
author: rick-anderson
description: Entity Framework Core ile bir Razor sayfası için yeni bir alan ekleme işlemi açıklanır
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 3799b072da04e32948b5fc78032f0575e760aa1d
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841455"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="b7a51-103">ASP.NET Core Razor sayfasına yeni bir alan ekleyin</span><span class="sxs-lookup"><span data-stu-id="b7a51-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="b7a51-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b7a51-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="b7a51-105">Bu bölümdeki [Entity Framework](/ef/core/get-started/aspnetcore/new-db) için Code First Migrations kullanılır:</span><span class="sxs-lookup"><span data-stu-id="b7a51-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="b7a51-106">Modele yeni bir alan ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b7a51-106">Add a new field to the model.</span></span>
* <span data-ttu-id="b7a51-107">Yeni alan şema değişikliği veritabanına geçirin.</span><span class="sxs-lookup"><span data-stu-id="b7a51-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="b7a51-108">EF Code First otomatik olarak Code First bir veritabanı oluşturmak için kullanırken:</span><span class="sxs-lookup"><span data-stu-id="b7a51-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="b7a51-109">Veritabanı şeması öğesinden oluşturulan model sınıfları ile eşitlenmiş olup olmadığını izlemek için veritabanında bir tablo ekler.</span><span class="sxs-lookup"><span data-stu-id="b7a51-109">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="b7a51-110">EF, model sınıfları sahip bir veritabanı eşit değilse bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b7a51-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="b7a51-111">Şema/modelinin eşitlenmiş otomatik doğrulama tutarsız veritabanı kod sorunlarını bulmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="b7a51-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="b7a51-112">Film modeli derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="b7a51-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="b7a51-113">Açık *Models/Movie.cs* dosya ve ekleme bir `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="b7a51-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="b7a51-114">Uygulama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b7a51-114">Build the app.</span></span>

<span data-ttu-id="b7a51-115">Düzen *Pages/Movies/Index.cshtml*ve bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="b7a51-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

<span data-ttu-id="b7a51-116">Aşağıdaki sayfalar güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b7a51-116">Update the following pages:</span></span>

* <span data-ttu-id="b7a51-117">Ekleme `Rating` silme ve ayrıntıları sayfaları alanı.</span><span class="sxs-lookup"><span data-stu-id="b7a51-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="b7a51-118">Güncelleştirme [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) ile bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="b7a51-118">Update [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="b7a51-119">Ekleme `Rating` Düzenle sayfasında alanı.</span><span class="sxs-lookup"><span data-stu-id="b7a51-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="b7a51-120">Uygulama DB yeni alanı içerecek şekilde güncelleştirilene kadar çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="b7a51-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="b7a51-121">Şimdi uygulamayı oluşturur çalıştırırsanız bir `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="b7a51-121">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="b7a51-122">Bu hata, veritabanının film tablonun şeması farklı olan güncelleştirilmiş film model sınıfı kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="b7a51-122">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="b7a51-123">(Yok hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="b7a51-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="b7a51-124">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="b7a51-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="b7a51-125">Otomatik olarak bırakın ve yeni model sınıfı şeması kullanarak veritabanını yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="b7a51-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="b7a51-126">Bu yaklaşım, Geliştirme döngüsünün başlarında kullanışlıdır; model ve veritabanı şeması birlikte hızla geliştirilebilen olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="b7a51-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="b7a51-127">Olumsuz tarafı, veritabanındaki mevcut verileri kaybetmek ' dir.</span><span class="sxs-lookup"><span data-stu-id="b7a51-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="b7a51-128">Bu yaklaşım, bir üretim veritabanında kullanmayın!</span><span class="sxs-lookup"><span data-stu-id="b7a51-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="b7a51-129">Bir veritabanı üzerinde şema değişikliklerini bırakarak ve veritabanı test verileri ile otomatik olarak oluşturmak için bir Başlatıcısı kullanarak genellikle bir uygulama geliştirmek için bir üretken yoludur.</span><span class="sxs-lookup"><span data-stu-id="b7a51-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="b7a51-130">Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b7a51-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="b7a51-131">Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="b7a51-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="b7a51-132">Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b7a51-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="b7a51-133">Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="b7a51-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="b7a51-134">Bu öğreticide, Code First Migrations'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="b7a51-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="b7a51-135">Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlar sınıfını.</span><span class="sxs-lookup"><span data-stu-id="b7a51-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="b7a51-136">Aşağıda bir örnek değişikliği gösterilmiştir, ancak her biri için bu değişikliği yapmak istersiniz `new Movie` blok.</span><span class="sxs-lookup"><span data-stu-id="b7a51-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="b7a51-137">Bkz: [SeedData.cs dosya tamamlandı](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="b7a51-137">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="b7a51-138">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b7a51-138">Build the solution.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7a51-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7a51-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="b7a51-140">Derecelendirme alanı için bir geçiş ekleyin</span><span class="sxs-lookup"><span data-stu-id="b7a51-140">Add a migration for the rating field</span></span>

<span data-ttu-id="b7a51-141">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="b7a51-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="b7a51-142">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="b7a51-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="b7a51-143">`Add-Migration` Komutu framework bildirir:</span><span class="sxs-lookup"><span data-stu-id="b7a51-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="b7a51-144">Karşılaştırma `Movie` ile model `Movie` DB şema.</span><span class="sxs-lookup"><span data-stu-id="b7a51-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="b7a51-145">Yeni modeline DB şema geçişi için kod oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b7a51-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="b7a51-146">Adı "Sıralama" isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b7a51-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="b7a51-147">Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="b7a51-147">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="b7a51-148">`Update-Database` Komutu veritabanı için şema değişiklikleri uygulamak için framework bildirir.</span><span class="sxs-lookup"><span data-stu-id="b7a51-148">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="b7a51-149">DB tüm kayıtların silerseniz, başlatıcı DB çekirdeğini ve dahil `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="b7a51-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="b7a51-150">Tarayıcıda veya gelen silme bağlantıları kullanarak bunu yapabilirsiniz [Sql Server Nesne Gezgini](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="b7a51-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="b7a51-151">Başka bir seçenek veritabanını silin ve veritabanını yeniden oluşturmaya geçişleri kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="b7a51-151">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="b7a51-152">SSOX veritabanında silmek için:</span><span class="sxs-lookup"><span data-stu-id="b7a51-152">To delete the database in SSOX:</span></span>

* <span data-ttu-id="b7a51-153">Veritabanı içinde SSOX seçin.</span><span class="sxs-lookup"><span data-stu-id="b7a51-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="b7a51-154">Veritabanını sağ tıklatın ve seçin *Sil*.</span><span class="sxs-lookup"><span data-stu-id="b7a51-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="b7a51-155">Denetleme **var olan bağlantıları kapatın**.</span><span class="sxs-lookup"><span data-stu-id="b7a51-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="b7a51-156">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="b7a51-156">Select **OK**.</span></span>
* <span data-ttu-id="b7a51-157">İçinde [PMC](xref:tutorials/razor-pages/new-field#pmc), veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="b7a51-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b7a51-158">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="b7a51-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="b7a51-159">Bırakın ve veritabanını yeniden oluşturun</span><span class="sxs-lookup"><span data-stu-id="b7a51-159">Drop and re-create the database</span></span>

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="b7a51-160">Veritabanını silin ve veritabanını yeniden oluşturmaya geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="b7a51-160">Delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="b7a51-161">Veritabanını silmek için veritabanı dosyasını silin (*MvcMovie.db*).</span><span class="sxs-lookup"><span data-stu-id="b7a51-161">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="b7a51-162">Ardından çalıştırın `ef database update` komutu:</span><span class="sxs-lookup"><span data-stu-id="b7a51-162">Then run the `ef database update` command:</span></span> 

```console
dotnet ef database update
```

---  
<!-- End of VS tabs -->

<span data-ttu-id="b7a51-163">Uygulamayı çalıştırın ve kontrol edebilirsiniz oluşturma/düzenleme/görüntüleme filmlerle bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="b7a51-163">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="b7a51-164">Veritabanı çekirdek değeri oluşturulmuş değil, bir kesme noktası ayarlayın `SeedData.Initialize` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b7a51-164">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7a51-165">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b7a51-165">Additional resources</span></span>

* [<span data-ttu-id="b7a51-166">Bu öğreticide YouTube sürümü</span><span class="sxs-lookup"><span data-stu-id="b7a51-166">YouTube version of this tutorial</span></span>](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> <span data-ttu-id="b7a51-167">[Önceki: Arama ekleme](xref:tutorials/razor-pages/search)
> [sonraki: Doğrulama ekleme](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="b7a51-167">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
