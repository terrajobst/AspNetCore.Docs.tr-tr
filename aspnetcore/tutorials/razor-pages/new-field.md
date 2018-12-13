---
title: ASP.NET Core Razor sayfasına yeni bir alan ekleyin
author: rick-anderson
description: Entity Framework Core ile bir Razor sayfası için yeni bir alan ekleme işlemi açıklanır
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 9b3ad5f6c4b1c9b5f016f5591127c8d1b213948d
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329139"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="91351-103">ASP.NET Core Razor sayfasına yeni bir alan ekleyin</span><span class="sxs-lookup"><span data-stu-id="91351-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="91351-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="91351-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="91351-105">Bu bölümdeki [Entity Framework](/ef/core/get-started/aspnetcore/new-db) için Code First Migrations kullanılır:</span><span class="sxs-lookup"><span data-stu-id="91351-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="91351-106">Modele yeni bir alan ekleyin.</span><span class="sxs-lookup"><span data-stu-id="91351-106">Add a new field to the model.</span></span>
* <span data-ttu-id="91351-107">Yeni alan şema değişikliği veritabanına geçirin.</span><span class="sxs-lookup"><span data-stu-id="91351-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="91351-108">EF Code First otomatik olarak Code First bir veritabanı oluşturmak için kullanırken:</span><span class="sxs-lookup"><span data-stu-id="91351-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="91351-109">Veritabanı şeması öğesinden oluşturulan model sınıfları ile eşitlenmiş olup olmadığını izlemek için veritabanında bir tablo ekler.</span><span class="sxs-lookup"><span data-stu-id="91351-109">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="91351-110">EF, model sınıfları sahip bir veritabanı eşit değilse bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="91351-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="91351-111">Şema/modelinin eşitlenmiş otomatik doğrulama tutarsız veritabanı kod sorunlarını bulmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="91351-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="91351-112">Film modeli derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="91351-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="91351-113">Açık *Models/Movie.cs* dosya ve ekleme bir `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="91351-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="91351-114">Uygulama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="91351-114">Build the app.</span></span>

<span data-ttu-id="91351-115">Düzen *Pages/Movies/Index.cshtml*ve bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="91351-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

<span data-ttu-id="91351-116">Aşağıdaki sayfalar güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="91351-116">Update the following pages:</span></span>

* <span data-ttu-id="91351-117">Ekleme `Rating` silme ve ayrıntıları sayfaları alanı.</span><span class="sxs-lookup"><span data-stu-id="91351-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="91351-118">Güncelleştirme [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) ile bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="91351-118">Update [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="91351-119">Ekleme `Rating` Düzenle sayfasında alanı.</span><span class="sxs-lookup"><span data-stu-id="91351-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="91351-120">Uygulama DB yeni alanı içerecek şekilde güncelleştirilene kadar çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="91351-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="91351-121">Şimdi uygulamayı oluşturur çalıştırırsanız bir `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="91351-121">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="91351-122">Bu hata, veritabanının film tablonun şeması farklı olan güncelleştirilmiş film model sınıfı kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="91351-122">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="91351-123">(Yok hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="91351-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="91351-124">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="91351-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="91351-125">Otomatik olarak bırakın ve yeni model sınıfı şeması kullanarak veritabanını yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="91351-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="91351-126">Bu yaklaşım, Geliştirme döngüsünün başlarında kullanışlıdır; model ve veritabanı şeması birlikte hızla geliştirilebilen olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="91351-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="91351-127">Olumsuz tarafı, veritabanındaki mevcut verileri kaybetmek ' dir.</span><span class="sxs-lookup"><span data-stu-id="91351-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="91351-128">Bu yaklaşım, bir üretim veritabanında kullanmayın!</span><span class="sxs-lookup"><span data-stu-id="91351-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="91351-129">Bir veritabanı üzerinde şema değişikliklerini bırakarak ve veritabanı test verileri ile otomatik olarak oluşturmak için bir Başlatıcısı kullanarak genellikle bir uygulama geliştirmek için bir üretken yoludur.</span><span class="sxs-lookup"><span data-stu-id="91351-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="91351-130">Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin.</span><span class="sxs-lookup"><span data-stu-id="91351-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="91351-131">Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="91351-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="91351-132">Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.</span><span class="sxs-lookup"><span data-stu-id="91351-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="91351-133">Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="91351-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="91351-134">Bu öğreticide, Code First Migrations'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="91351-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="91351-135">Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlar sınıfını.</span><span class="sxs-lookup"><span data-stu-id="91351-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="91351-136">Aşağıda bir örnek değişikliği gösterilmiştir, ancak her biri için bu değişikliği yapmak istersiniz `new Movie` blok.</span><span class="sxs-lookup"><span data-stu-id="91351-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="91351-137">Bkz: [SeedData.cs dosya tamamlandı](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="91351-137">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="91351-138">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="91351-138">Build the solution.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="91351-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91351-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="91351-140">Derecelendirme alanı için bir geçiş ekleyin</span><span class="sxs-lookup"><span data-stu-id="91351-140">Add a migration for the rating field</span></span>

<span data-ttu-id="91351-141">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="91351-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="91351-142">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="91351-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="91351-143">`Add-Migration` Komutu framework bildirir:</span><span class="sxs-lookup"><span data-stu-id="91351-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="91351-144">Karşılaştırma `Movie` ile model `Movie` DB şema.</span><span class="sxs-lookup"><span data-stu-id="91351-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="91351-145">Yeni modeline DB şema geçişi için kod oluşturun.</span><span class="sxs-lookup"><span data-stu-id="91351-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="91351-146">Adı "Sıralama" isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91351-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="91351-147">Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="91351-147">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="91351-148">`Update-Database` Komutu veritabanı için şema değişiklikleri uygulamak için framework bildirir.</span><span class="sxs-lookup"><span data-stu-id="91351-148">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="91351-149">DB tüm kayıtların silerseniz, başlatıcı DB çekirdeğini ve dahil `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="91351-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="91351-150">Tarayıcıda veya gelen silme bağlantıları kullanarak bunu yapabilirsiniz [Sql Server Nesne Gezgini](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="91351-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="91351-151">Başka bir seçenek veritabanını silin ve veritabanını yeniden oluşturmaya geçişleri kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="91351-151">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="91351-152">SSOX veritabanında silmek için:</span><span class="sxs-lookup"><span data-stu-id="91351-152">To delete the database in SSOX:</span></span>

* <span data-ttu-id="91351-153">Veritabanı içinde SSOX seçin.</span><span class="sxs-lookup"><span data-stu-id="91351-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="91351-154">Veritabanını sağ tıklatın ve seçin *Sil*.</span><span class="sxs-lookup"><span data-stu-id="91351-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="91351-155">Denetleme **var olan bağlantıları kapatın**.</span><span class="sxs-lookup"><span data-stu-id="91351-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="91351-156">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="91351-156">Select **OK**.</span></span>
* <span data-ttu-id="91351-157">İçinde [PMC](xref:tutorials/razor-pages/new-field#pmc), veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="91351-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="91351-158">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="91351-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<!-- copy/paste this tab to the next. Not worth an include  -->

<span data-ttu-id="91351-159">Aşağıdaki .NET Core CLI komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="91351-159">Run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add Rating
dotnet ef database update
```

<span data-ttu-id="91351-160">`ef migrations add` Komutu framework bildirir:</span><span class="sxs-lookup"><span data-stu-id="91351-160">The `ef migrations add` command tells the framework to:</span></span>

* <span data-ttu-id="91351-161">Karşılaştırma `Movie` ile model `Movie` DB şema.</span><span class="sxs-lookup"><span data-stu-id="91351-161">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="91351-162">Yeni modeline DB şema geçişi için kod oluşturun.</span><span class="sxs-lookup"><span data-stu-id="91351-162">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="91351-163">Adı "Sıralama" isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91351-163">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="91351-164">Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="91351-164">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="91351-165">`ef database update` Komutu veritabanı için şema değişiklikleri uygulamak için framework bildirir.</span><span class="sxs-lookup"><span data-stu-id="91351-165">The `ef database update` command tells the framework to apply the schema changes to the database.</span></span>

<span data-ttu-id="91351-166">DB tüm kayıtların silerseniz, başlatıcı DB çekirdeğini ve dahil `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="91351-166">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="91351-167">Delete bağlantıları tarayıcıda veya bir SQLite aracını kullanarak yapın.</span><span class="sxs-lookup"><span data-stu-id="91351-167">You can do this with the delete links in the browser or by using a SQLite tool.</span></span>

<span data-ttu-id="91351-168">Başka bir seçenek veritabanını silin ve veritabanını yeniden oluşturmaya geçişleri kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="91351-168">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="91351-169">Veritabanını silmek için veritabanı dosyasını silin (*MvcMovie.db*).</span><span class="sxs-lookup"><span data-stu-id="91351-169">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="91351-170">Ardından çalıştırın `ef database update` komutu:</span><span class="sxs-lookup"><span data-stu-id="91351-170">Then run the `ef database update` command:</span></span> 

```console
dotnet ef database update
```

> [!NOTE]
> <span data-ttu-id="91351-171">Birçok şema değiştirme işlemlerini EF Core SQLite sağlayıcı tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="91351-171">Many schema change operations are not supported by the EF Core SQLite provider.</span></span> <span data-ttu-id="91351-172">Örneğin, sütun ekleme desteklenir, ancak bir sütun kaldırılması desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="91351-172">For example, adding a column is supported, but removing a column is not supported.</span></span> <span data-ttu-id="91351-173">Bir sütunu kaldırmak için bir geçiş eklerseniz `ef migrations add` komut başarılı ancak `ef database update` komutu başarısız oluyor.</span><span class="sxs-lookup"><span data-stu-id="91351-173">If you add a migration to remove a column, the `ef migrations add` command succeeds but the `ef database update` command fails.</span></span> <span data-ttu-id="91351-174">Bazı kısıtlamalar nedeniyle geçici olarak bir tablo yeniden oluşturma gerçekleştirmek için geçiş kodu el ile yazarak çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="91351-174">You can work around some of the limitations by manually writing migrations code to perform a table rebuild.</span></span> <span data-ttu-id="91351-175">Bir tablo yeniden oluşturma, varolan bir tabloyu yeniden adlandırma, yeni bir tablo oluşturma, yeni tabloya veri kopyalama ve eski tablo bırakılırken içerir.</span><span class="sxs-lookup"><span data-stu-id="91351-175">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="91351-176">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="91351-176">For more information, see the following resources:</span></span>
> * [<span data-ttu-id="91351-177">SQLite EF Core veritabanı sağlayıcısı sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="91351-177">SQLite EF Core Database Provider Limitations</span></span>](/ef/core/providers/sqlite/limitations)
> * [<span data-ttu-id="91351-178">Geçiş kodu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="91351-178">Customize migration code</span></span>](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [<span data-ttu-id="91351-179">Veri çekirdeği oluşturma</span><span class="sxs-lookup"><span data-stu-id="91351-179">Data seeding</span></span>](/ef/core/modeling/data-seeding)

---  
<!-- End of VS tabs -->

<span data-ttu-id="91351-180">Uygulamayı çalıştırın ve kontrol edebilirsiniz oluşturma/düzenleme/görüntüleme filmlerle bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="91351-180">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="91351-181">Veritabanı çekirdek değeri oluşturulmuş değil, bir kesme noktası ayarlayın `SeedData.Initialize` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="91351-181">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="91351-182">[Önceki: Arama ekleme](xref:tutorials/razor-pages/search)
> [sonraki: Doğrulama ekleme](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="91351-182">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
