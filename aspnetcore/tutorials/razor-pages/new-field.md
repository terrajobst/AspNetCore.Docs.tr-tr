---
title: ASP.NET Core Razor sayfasına yeni bir alan ekleyin
author: rick-anderson
description: Entity Framework Core ile bir Razor sayfası için yeni bir alan ekleme işlemi açıklanır
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: f8be269887903797803257d8a21e002519102047
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089519"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="3cfd8-103">ASP.NET Core Razor sayfasına yeni bir alan ekleyin</span><span class="sxs-lookup"><span data-stu-id="3cfd8-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="3cfd8-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3cfd8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3cfd8-105">Bu bölümde kullandığınız [Entity Framework](/ef/core/get-started/aspnetcore/new-db) veritabanına Code First Migrations'modeli için yeni bir alan ekleme ve bu geçiş için değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-105">In this section you use [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="3cfd8-106">EF Code First otomatik olarak Code First bir veritabanı oluşturmak için kullanırken:</span><span class="sxs-lookup"><span data-stu-id="3cfd8-106">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="3cfd8-107">Veritabanı şeması öğesinden oluşturulan model sınıfları ile eşitlenmiş olup olmadığını izlemek için veritabanında bir tablo ekler.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-107">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="3cfd8-108">EF, model sınıfları sahip bir veritabanı eşit değilse bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-108">If the model classes aren't in sync with the DB, EF throws an exception.</span></span> 

<span data-ttu-id="3cfd8-109">Şema/modelinin eşitlenmiş otomatik doğrulama tutarsız veritabanı kod sorunlarını bulmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-109">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="3cfd8-110">Film modeli derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="3cfd8-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="3cfd8-111">Açık *Models/Movie.cs* dosya ve ekleme bir `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="3cfd8-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

<span data-ttu-id="3cfd8-112">(Ctrl + Shift + B) uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-112">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="3cfd8-113">Düzen *Pages/Movies/Index.cshtml*ve bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="3cfd8-113">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="3cfd8-114">Ekleme `Rating` silme ve ayrıntıları sayfaları alanı.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-114">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="3cfd8-115">Güncelleştirme *Create.cshtml* ile bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-115">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="3cfd8-116">Kopyalama/önceki yapıştırma `<div>` alanları güncelleştirme öğesi ve let IntelliSense yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-116">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="3cfd8-117">IntelliSense ile birlikte çalışır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="3cfd8-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Öznitelik değeri ASP R harfiyle Geliştirici yazdığını-için görünümün ikinci etiket öğesinde.](new-field/_static/cr.png)

<span data-ttu-id="3cfd8-121">Aşağıdaki kodda gösterildiği *Create.cshtml* ile bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="3cfd8-121">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="3cfd8-122">Ekleme `Rating` Düzenle sayfasında alanı.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-122">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="3cfd8-123">Uygulama DB yeni alanı içerecek şekilde güncelleştirilene kadar çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-123">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="3cfd8-124">Şimdi uygulamayı oluşturur çalıştırırsanız bir `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="3cfd8-124">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="3cfd8-125">Bu hata, veritabanının film tablonun şeması farklı olan güncelleştirilmiş film model sınıfı kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-125">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="3cfd8-126">(Yok hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="3cfd8-126">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="3cfd8-127">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="3cfd8-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="3cfd8-128">Otomatik olarak bırakın ve yeni model sınıfı şeması kullanarak veritabanını yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-128">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="3cfd8-129">Bu yaklaşım, Geliştirme döngüsünün başlarında kullanışlıdır; model ve veritabanı şeması birlikte hızla geliştirilebilen olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-129">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="3cfd8-130">Olumsuz tarafı, veritabanındaki mevcut verileri kaybetmek ' dir.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-130">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="3cfd8-131">Bu yaklaşım bir üretim veritabanında kullanmak istemiyorsanız!</span><span class="sxs-lookup"><span data-stu-id="3cfd8-131">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="3cfd8-132">Bir veritabanı üzerinde şema değişikliklerini bırakarak ve veritabanı test verileri ile otomatik olarak oluşturmak için bir Başlatıcısı kullanarak genellikle bir uygulama geliştirmek için bir üretken yoludur.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-132">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="3cfd8-133">Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="3cfd8-134">Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="3cfd8-135">Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="3cfd8-136">Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="3cfd8-137">Bu öğreticide, Code First Migrations'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-137">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="3cfd8-138">Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlar sınıfını.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="3cfd8-139">Aşağıda bir örnek değişikliği gösterilmiştir, ancak her biri için bu değişikliği yapmak istersiniz `new Movie` blok.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-139">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="3cfd8-140">Bkz: [SeedData.cs dosya tamamlandı](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="3cfd8-140">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3cfd8-141">Bkz: [SeedData.cs dosya tamamlandı](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="3cfd8-141">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span></span>

::: moniker-end

<span data-ttu-id="3cfd8-142">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-142">Build the solution.</span></span>

<a name="pmc"></a>

<span data-ttu-id="3cfd8-143">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-143">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="3cfd8-144">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="3cfd8-144">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="3cfd8-145">`Add-Migration` Komutu framework bildirir:</span><span class="sxs-lookup"><span data-stu-id="3cfd8-145">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="3cfd8-146">Karşılaştırma `Movie` ile model `Movie` DB şema.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-146">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="3cfd8-147">Yeni modeline DB şema geçişi için kod oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-147">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="3cfd8-148">Adı "Sıralama" isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-148">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="3cfd8-149">Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-149">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a>

<span data-ttu-id="3cfd8-150">DB tüm kayıtların silerseniz, başlatıcı DB çekirdeğini ve dahil `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-150">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="3cfd8-151">Tarayıcıda veya gelen silme bağlantıları kullanarak bunu yapabilirsiniz [Sql Server Nesne Gezgini](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="3cfd8-151">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="3cfd8-152">SSOX veritabanını silmek için:</span><span class="sxs-lookup"><span data-stu-id="3cfd8-152">To delete the database from SSOX:</span></span>

* <span data-ttu-id="3cfd8-153">Veritabanı içinde SSOX seçin.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="3cfd8-154">Veritabanını sağ tıklatın ve seçin *Sil*.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="3cfd8-155">Denetleme **var olan bağlantıları kapatın**.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="3cfd8-156">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-156">Select **OK**.</span></span>
* <span data-ttu-id="3cfd8-157">İçinde [PMC](xref:tutorials/razor-pages/new-field#pmc), veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="3cfd8-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="3cfd8-158">Uygulamayı çalıştırın ve kontrol edebilirsiniz oluşturma/düzenleme/görüntüleme filmlerle bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-158">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="3cfd8-159">Veritabanı çekirdek değeri oluşturulmuş değil, IIS Express durdurun ve ardından uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="3cfd8-159">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3cfd8-160">[Önceki: Arama ekleme](xref:tutorials/razor-pages/search)
> [sonraki: doğrulama ekleme](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="3cfd8-160">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
