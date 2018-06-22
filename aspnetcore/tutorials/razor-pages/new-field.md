---
title: ASP.NET Core Razor sayfasında yeni bir alan ekleyin
author: rick-anderson
description: Razor sayfasını Entity Framework Çekirdek ile yeni bir alan eklemek nasıl gösterir
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: d9bf8c7cea20bf38aacf432465d7b33514bcd64d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277299"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="5f5b0-103">ASP.NET Core Razor sayfasında yeni bir alan ekleyin</span><span class="sxs-lookup"><span data-stu-id="5f5b0-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="5f5b0-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5f5b0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5f5b0-105">Bu bölümde kullandığınız [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) yeni bir alan modele eklemek ve bu geçirmek için Code First geçişleri veritabanına değiştirin.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-105">In this section you use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="5f5b0-106">EF Code First otomatik olarak Code First bir veritabanı oluşturmak için kullanırken:</span><span class="sxs-lookup"><span data-stu-id="5f5b0-106">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="5f5b0-107">Bir tablo veritabanı şeması öğesinden oluşturulduğu modeli sınıfları ile eşit olup olmadığını izlemek için veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-107">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="5f5b0-108">EF modeli sınıfları DB ile eşitlenmiş durumda değilse, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-108">If the model classes aren't in sync with the DB, EF throws an exception.</span></span> 

<span data-ttu-id="5f5b0-109">Şema/model eşitlenmiş otomatik doğrulanması tutarsız veritabanı/kod sorunları bulmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-109">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="5f5b0-110">Film modeline derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="5f5b0-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="5f5b0-111">Açık *Models/Movie.cs* dosya ve ekleme bir `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="5f5b0-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>
::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="5f5b0-112">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="5f5b0-112">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="5f5b0-113">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="5f5b0-113">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span></span>

::: moniker-end

<span data-ttu-id="5f5b0-114">(Ctrl + Shift + B) uygulaması oluşturma.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-114">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="5f5b0-115">Düzen *Pages/Movies/Index.cshtml*ve ekleme bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="5f5b0-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="5f5b0-116">Ekleme `Rating` silme ve ayrıntıları sayfaları alanı.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-116">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="5f5b0-117">Güncelleştirme *Create.cshtml* ile bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-117">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="5f5b0-118">Kopyalayıp önceki yapıştırın `<div>` alanları güncelleştir öğesi ve let IntelliSense Yardım.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-118">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="5f5b0-119">IntelliSense çalışır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="5f5b0-119">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Geliştirici asp öznitelik değerini Harf R yazdı-için görünümün ikinci etiket öğesindeki.](new-field/_static/cr.png)

<span data-ttu-id="5f5b0-123">Aşağıdaki kodda gösterildiği *Create.cshtml* ile bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="5f5b0-123">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="5f5b0-124">Ekleme `Rating` sayfasını Düzenle alanı.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-124">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="5f5b0-125">Uygulama DB yeni alanı içerecek şekilde güncelleştirilene kadar çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-125">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="5f5b0-126">Şimdi, uygulama atar çalıştırırsanız bir `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="5f5b0-126">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="5f5b0-127">Bu hata veritabanının film tablonun şeması farklı güncelleştirilmiş film model sınıfı kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-127">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="5f5b0-128">(Var. hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="5f5b0-128">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="5f5b0-129">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="5f5b0-129">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="5f5b0-130">Otomatik olarak bırakın ve yeni model sınıfı şeması kullanarak veritabanını yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-130">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="5f5b0-131">Bu yaklaşım, erken geliştirme döngüsü uygundur; hızlı bir şekilde modeli ve veritabanı şeması birlikte gelişmesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-131">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="5f5b0-132">Dezavantajı, veritabanında var olan veri kaybı ' dir.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-132">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="5f5b0-133">Bir üretim veritabanında bu yaklaşımı kullanmak istemiyorsanız!</span><span class="sxs-lookup"><span data-stu-id="5f5b0-133">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="5f5b0-134">Şema değişiklikleri DB bırakarak ve otomatik olarak test verileri ile veritabanı oluşturmak için bir başlatıcı kullanarak bir uygulama geliştirmek için verimli bir şekilde görülür.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-134">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="5f5b0-135">Açıkça modeli sınıfları eşleşecek şekilde var olan veritabanının şema değiştirin.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-135">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="5f5b0-136">Bu yaklaşımın avantajı, verilerinizi tutmak ' dir.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-136">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="5f5b0-137">Bu değişikliği yapmak ya da el ile veya komut dosyası bir veritabanı oluşturarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-137">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="5f5b0-138">Veritabanı şeması güncelleştirmek için Code First Migrations kullanın.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-138">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="5f5b0-139">Bu öğretici için Code First Migrations kullanın.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-139">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="5f5b0-140">Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlayan sınıf.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-140">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="5f5b0-141">Bir örnek değişiklik aşağıda gösterilen, ancak her biri için bu değişikliği yapmak istersiniz `new Movie` bloğu.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-141">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="5f5b0-142">Bkz: [SeedData.cs dosya tamamlandı](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="5f5b0-142">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="5f5b0-143">Bkz: [SeedData.cs dosya tamamlandı](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="5f5b0-143">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span></span>
::: moniker-end

<span data-ttu-id="5f5b0-144">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-144">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="5f5b0-145">Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-145">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="5f5b0-146">PMC aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="5f5b0-146">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="5f5b0-147">`Add-Migration` Komutu framework bildirir:</span><span class="sxs-lookup"><span data-stu-id="5f5b0-147">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="5f5b0-148">Karşılaştırma `Movie` ile model `Movie` DB şeması.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-148">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="5f5b0-149">Yeni modele DB şeması geçirmek için kod oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-149">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="5f5b0-150">Adı "Derecelendirme" rastgeledir ve geçiş dosya adı için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-150">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="5f5b0-151">Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-151">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="5f5b0-152">Veritabanındaki tüm kayıtları silerseniz, başlatıcı DB Çekirdek ve dahil `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-152">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="5f5b0-153">Tarayıcıda veya gelen silme bağlantılarla bunu yapabilirsiniz [Sql Server Nesne Gezgini](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="5f5b0-153">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="5f5b0-154">SSOX veritabanını silmek için:</span><span class="sxs-lookup"><span data-stu-id="5f5b0-154">To delete the database from SSOX:</span></span>

* <span data-ttu-id="5f5b0-155">Veritabanı içinde SSOX seçin.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-155">Select the database in SSOX.</span></span>
* <span data-ttu-id="5f5b0-156">Veritabanını sağ tıklatın ve seçin *silmek*.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-156">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="5f5b0-157">Denetleme **var olan bağlantıları kapatın**.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-157">Check **Close existing connections**.</span></span>
* <span data-ttu-id="5f5b0-158">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-158">Select **OK**.</span></span>
* <span data-ttu-id="5f5b0-159">İçinde [PMC](xref:tutorials/razor-pages/new-field#pmc), veritabanını güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="5f5b0-159">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="5f5b0-160">Uygulamayı çalıştırın ve oluşturma/düzenleme/görüntüleme filmler ile yapabilecekleriniz doğrulayın bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-160">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="5f5b0-161">Veritabanı dağıtılan değil, IIS Express durdurun ve ardından uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5f5b0-161">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5f5b0-162">[Önceki: Arama ekleme](xref:tutorials/razor-pages/search)
> [sonraki: doğrulama ekleme](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="5f5b0-162">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
