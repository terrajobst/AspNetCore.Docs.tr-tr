---
title: ASP.NET Core Razor sayfasına yeni bir alan ekleyin
author: rick-anderson
description: Entity Framework Core ile bir Razor sayfası için yeni bir alan ekleme işlemi açıklanır
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: e280bc9553113982a1f1a77eabab32575c905237
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862297"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="09883-103">ASP.NET Core Razor sayfasına yeni bir alan ekleyin</span><span class="sxs-lookup"><span data-stu-id="09883-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="09883-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="09883-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="09883-105">Bu bölümdeki [Entity Framework](/ef/core/get-started/aspnetcore/new-db) için Code First Migrations kullanılır:</span><span class="sxs-lookup"><span data-stu-id="09883-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="09883-106">Modele yeni bir alan ekleyin.</span><span class="sxs-lookup"><span data-stu-id="09883-106">Add a new field to the model.</span></span>
* <span data-ttu-id="09883-107">Yeni alan şema değişikliği veritabanına geçirin.</span><span class="sxs-lookup"><span data-stu-id="09883-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="09883-108">EF Code First otomatik olarak Code First bir veritabanı oluşturmak için kullanırken:</span><span class="sxs-lookup"><span data-stu-id="09883-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="09883-109">Veritabanı şeması öğesinden oluşturulan model sınıfları ile eşitlenmiş olup olmadığını izlemek için veritabanında bir tablo ekler.</span><span class="sxs-lookup"><span data-stu-id="09883-109">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="09883-110">EF, model sınıfları sahip bir veritabanı eşit değilse bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="09883-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="09883-111">Şema/modelinin eşitlenmiş otomatik doğrulama tutarsız veritabanı kod sorunlarını bulmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="09883-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="09883-112">Film modeli derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="09883-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="09883-113">Açık *Models/Movie.cs* dosya ve ekleme bir `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="09883-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="09883-114">Uygulama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="09883-114">Build the app.</span></span>

<span data-ttu-id="09883-115">Düzen *Pages/Movies/Index.cshtml*ve bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="09883-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

<span data-ttu-id="09883-116">Aşağıdaki sayfalar güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="09883-116">Update the following pages:</span></span>

* <span data-ttu-id="09883-117">Ekleme `Rating` silme ve ayrıntıları sayfaları alanı.</span><span class="sxs-lookup"><span data-stu-id="09883-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="09883-118">Güncelleştirme [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) ile bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="09883-118">Update [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="09883-119">Ekleme `Rating` Düzenle sayfasında alanı.</span><span class="sxs-lookup"><span data-stu-id="09883-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="09883-120">Uygulama DB yeni alanı içerecek şekilde güncelleştirilene kadar çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="09883-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="09883-121">Şimdi uygulamayı oluşturur çalıştırırsanız bir `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="09883-121">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="09883-122">Bu hata, veritabanının film tablonun şeması farklı olan güncelleştirilmiş film model sınıfı kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="09883-122">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="09883-123">(Yok hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="09883-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="09883-124">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="09883-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="09883-125">Otomatik olarak bırakın ve yeni model sınıfı şeması kullanarak veritabanını yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="09883-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="09883-126">Bu yaklaşım, Geliştirme döngüsünün başlarında kullanışlıdır; model ve veritabanı şeması birlikte hızla geliştirilebilen olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="09883-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="09883-127">Olumsuz tarafı, veritabanındaki mevcut verileri kaybetmek ' dir.</span><span class="sxs-lookup"><span data-stu-id="09883-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="09883-128">Bu yaklaşım, bir üretim veritabanında kullanmayın!</span><span class="sxs-lookup"><span data-stu-id="09883-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="09883-129">Bir veritabanı üzerinde şema değişikliklerini bırakarak ve veritabanı test verileri ile otomatik olarak oluşturmak için bir Başlatıcısı kullanarak genellikle bir uygulama geliştirmek için bir üretken yoludur.</span><span class="sxs-lookup"><span data-stu-id="09883-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="09883-130">Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin.</span><span class="sxs-lookup"><span data-stu-id="09883-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="09883-131">Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="09883-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="09883-132">Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.</span><span class="sxs-lookup"><span data-stu-id="09883-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="09883-133">Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="09883-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="09883-134">Bu öğreticide, Code First Migrations'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="09883-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="09883-135">Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlar sınıfını.</span><span class="sxs-lookup"><span data-stu-id="09883-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="09883-136">Aşağıda bir örnek değişikliği gösterilmiştir, ancak her biri için bu değişikliği yapmak istersiniz `new Movie` blok.</span><span class="sxs-lookup"><span data-stu-id="09883-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="09883-137">Bkz: [SeedData.cs dosya tamamlandı](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="09883-137">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="09883-138">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="09883-138">Build the solution.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="09883-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09883-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="09883-140">Derecelendirme alanı için bir geçiş ekleyin</span><span class="sxs-lookup"><span data-stu-id="09883-140">Add a migration for the rating field</span></span>

<span data-ttu-id="09883-141">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="09883-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="09883-142">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="09883-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="09883-143">`Add-Migration` Komutu framework bildirir:</span><span class="sxs-lookup"><span data-stu-id="09883-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="09883-144">Karşılaştırma `Movie` ile model `Movie` DB şema.</span><span class="sxs-lookup"><span data-stu-id="09883-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="09883-145">Yeni modeline DB şema geçişi için kod oluşturun.</span><span class="sxs-lookup"><span data-stu-id="09883-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="09883-146">Adı "Sıralama" isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="09883-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="09883-147">Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="09883-147">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a>

<span data-ttu-id="09883-148">DB tüm kayıtların silerseniz, başlatıcı DB çekirdeğini ve dahil `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="09883-148">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="09883-149">Tarayıcıda veya gelen silme bağlantıları kullanarak bunu yapabilirsiniz [Sql Server Nesne Gezgini](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="09883-149">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="09883-150">SSOX veritabanını silmek için:</span><span class="sxs-lookup"><span data-stu-id="09883-150">To delete the database from SSOX:</span></span>

* <span data-ttu-id="09883-151">Veritabanı içinde SSOX seçin.</span><span class="sxs-lookup"><span data-stu-id="09883-151">Select the database in SSOX.</span></span>
* <span data-ttu-id="09883-152">Veritabanını sağ tıklatın ve seçin *Sil*.</span><span class="sxs-lookup"><span data-stu-id="09883-152">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="09883-153">Denetleme **var olan bağlantıları kapatın**.</span><span class="sxs-lookup"><span data-stu-id="09883-153">Check **Close existing connections**.</span></span>
* <span data-ttu-id="09883-154">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="09883-154">Select **OK**.</span></span>
* <span data-ttu-id="09883-155">İçinde [PMC](xref:tutorials/razor-pages/new-field#pmc), veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="09883-155">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="09883-156">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="09883-156">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="09883-157"><!-- copy/paste this tab to the next. Not worth an include  --> SQLite geçişleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="09883-157"><!-- copy/paste this tab to the next. Not worth an include  --> SQLite doesn't support migrations.</span></span>

* <span data-ttu-id="09883-158">Veritabanını silin veya veritabanı adını değiştirmek *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="09883-158">Delete the database or change the database name in the *appsettings.json* file.</span></span>
* <span data-ttu-id="09883-159">Silme *geçişler* klasörü (ve klasördeki tüm dosyaları).</span><span class="sxs-lookup"><span data-stu-id="09883-159">Delete the *Migrations* folder (and all the files in the folder).</span></span>

<span data-ttu-id="09883-160">Aşağıdaki .NET Core CLI komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="09883-160">Run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add Rating
dotnet ef database update
```

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="09883-161">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09883-161">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="09883-162">SQLite geçişleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="09883-162">SQLite doesn't support migrations.</span></span>

* <span data-ttu-id="09883-163">Veritabanını silin veya veritabanı adını değiştirmek *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="09883-163">Delete the database or change the database name in the *appsettings.json* file.</span></span>
* <span data-ttu-id="09883-164">Silme *geçişler* klasörü (ve klasördeki tüm dosyaları).</span><span class="sxs-lookup"><span data-stu-id="09883-164">Delete the *Migrations* folder (and all the files in the folder).</span></span>

<span data-ttu-id="09883-165">Aşağıdaki .NET Core CLI komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="09883-165">Run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add Rating
dotnet ef database update
```

---  
<!-- End of VS tabs -->

<span data-ttu-id="09883-166">Uygulamayı çalıştırın ve kontrol edebilirsiniz oluşturma/düzenleme/görüntüleme filmlerle bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="09883-166">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="09883-167">Veritabanı çekirdek değeri oluşturulmuş değil, bir kesme noktası ayarlayın `SeedData.Initialize` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="09883-167">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="09883-168">[Önceki: Arama ekleme](xref:tutorials/razor-pages/search)
> [sonraki: doğrulama ekleme](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="09883-168">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
