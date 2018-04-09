---
title: ASP.NET Core Razor sayfasında yeni bir alan ekleyin
author: rick-anderson
description: Razor sayfasını Entity Framework Çekirdek ile yeni bir alan eklemek nasıl gösterir
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 2652aea2bc587074da5101e3de2bdf55d032214a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="47052-103">ASP.NET Core Razor sayfasında yeni bir alan ekleyin</span><span class="sxs-lookup"><span data-stu-id="47052-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="47052-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="47052-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="47052-105">Bu bölümde kullanacağınız [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) yeni bir alan modele eklemek ve bu geçirmek için Code First geçişleri veritabanına değiştirin.</span><span class="sxs-lookup"><span data-stu-id="47052-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="47052-106">EF Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda, Code First bir tablo veritabanı şeması öğesinden oluşturulduğu modeli sınıfları ile eşit olup olmadığını izlemenize yardımcı olması için veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="47052-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="47052-107">EF eşitlenmiş durumda değilse, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="47052-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="47052-108">Bu, tutarsız veritabanı/kod sorunları bulmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="47052-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="47052-109">Film modeline derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="47052-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="47052-110">Açık *Models/Movie.cs* dosya ve ekleme bir `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="47052-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="47052-111">(Ctrl + Shift + B) uygulaması oluşturma.</span><span class="sxs-lookup"><span data-stu-id="47052-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="47052-112">Düzen *Pages/Movies/Index.cshtml*ve ekleme bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="47052-112">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="47052-113">Ekleme `Rating` silme ve ayrıntıları sayfaları alanı.</span><span class="sxs-lookup"><span data-stu-id="47052-113">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="47052-114">Güncelleştirme *Create.cshtml* ile bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="47052-114">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="47052-115">Kopyalayıp önceki yapıştırın `<div>` alanları güncelleştir öğesi ve let IntelliSense Yardım.</span><span class="sxs-lookup"><span data-stu-id="47052-115">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="47052-116">IntelliSense çalışır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="47052-116">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Geliştirici asp öznitelik değerini Harf R yazdı-için görünümün ikinci etiket öğesindeki.](new-field/_static/cr.png)

<span data-ttu-id="47052-120">Aşağıdaki kodda gösterildiği *Create.cshtml* ile bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="47052-120">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="47052-121">Ekleme `Rating` sayfasını Düzenle alanı.</span><span class="sxs-lookup"><span data-stu-id="47052-121">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="47052-122">Uygulama DB yeni alanı içerecek şekilde güncelleştirilene kadar çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="47052-122">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="47052-123">Şimdi, uygulama atar çalıştırırsanız bir `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="47052-123">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="47052-124">Bu hata veritabanının film tablonun şeması farklı güncelleştirilmiş film model sınıfı kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="47052-124">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="47052-125">(Var. hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="47052-125">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="47052-126">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="47052-126">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="47052-127">Otomatik olarak bırakın ve yeni model sınıfı şeması kullanarak veritabanını yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="47052-127">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="47052-128">Bu yaklaşım, erken geliştirme döngüsü uygundur; hızlı bir şekilde modeli ve veritabanı şeması birlikte gelişmesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="47052-128">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="47052-129">Dezavantajı, veritabanında var olan veri kaybı ' dir.</span><span class="sxs-lookup"><span data-stu-id="47052-129">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="47052-130">Bir üretim veritabanında bu yaklaşımı kullanmak istemiyorsanız!</span><span class="sxs-lookup"><span data-stu-id="47052-130">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="47052-131">Şema değişiklikleri DB bırakarak ve otomatik olarak test verileri ile veritabanı oluşturmak için bir başlatıcı kullanarak bir uygulama geliştirmek için verimli bir şekilde görülür.</span><span class="sxs-lookup"><span data-stu-id="47052-131">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="47052-132">Açıkça modeli sınıfları eşleşecek şekilde var olan veritabanının şema değiştirin.</span><span class="sxs-lookup"><span data-stu-id="47052-132">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="47052-133">Bu yaklaşımın avantajı, verilerinizi tutmak ' dir.</span><span class="sxs-lookup"><span data-stu-id="47052-133">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="47052-134">Bu değişikliği yapmak ya da el ile veya komut dosyası bir veritabanı oluşturarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="47052-134">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="47052-135">Veritabanı şeması güncelleştirmek için Code First Migrations kullanın.</span><span class="sxs-lookup"><span data-stu-id="47052-135">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="47052-136">Bu öğretici için Code First Migrations kullanın.</span><span class="sxs-lookup"><span data-stu-id="47052-136">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="47052-137">Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlayan sınıf.</span><span class="sxs-lookup"><span data-stu-id="47052-137">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="47052-138">Bir örnek değişiklik aşağıda gösterilen, ancak her biri için bu değişikliği yapmak istersiniz `new Movie` bloğu.</span><span class="sxs-lookup"><span data-stu-id="47052-138">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="47052-139">Bkz: [SeedData.cs dosya tamamlandı](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="47052-139">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="47052-140">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="47052-140">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="47052-141">Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="47052-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="47052-142">PMC aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="47052-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="47052-143">`Add-Migration` Komutu framework bildirir:</span><span class="sxs-lookup"><span data-stu-id="47052-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="47052-144">Karşılaştırma `Movie` ile model `Movie` DB şeması.</span><span class="sxs-lookup"><span data-stu-id="47052-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="47052-145">Yeni modele DB şeması geçirmek için kod oluşturun.</span><span class="sxs-lookup"><span data-stu-id="47052-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="47052-146">Adı "Derecelendirme" rastgeledir ve geçiş dosya adı için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="47052-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="47052-147">Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="47052-147">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="47052-148">Veritabanındaki tüm kayıtları silerseniz, başlatıcı DB Çekirdek ve dahil `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="47052-148">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="47052-149">Tarayıcıda veya gelen silme bağlantılarla bunu yapabilirsiniz [Sql Server Nesne Gezgini](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="47052-149">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="47052-150">SSOX veritabanını silmek için:</span><span class="sxs-lookup"><span data-stu-id="47052-150">To delete the database from SSOX:</span></span>

* <span data-ttu-id="47052-151">Veritabanı içinde SSOX seçin.</span><span class="sxs-lookup"><span data-stu-id="47052-151">Select the database in SSOX.</span></span>
* <span data-ttu-id="47052-152">Veritabanını sağ tıklatın ve seçin *silmek*.</span><span class="sxs-lookup"><span data-stu-id="47052-152">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="47052-153">Denetleme **var olan bağlantıları kapatın**.</span><span class="sxs-lookup"><span data-stu-id="47052-153">Check **Close existing connections**.</span></span>
* <span data-ttu-id="47052-154">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="47052-154">Select **OK**.</span></span>
* <span data-ttu-id="47052-155">İçinde [PMC](xref:tutorials/razor-pages/new-field#pmc), veritabanını güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="47052-155">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="47052-156">Uygulamayı çalıştırın ve oluşturma/düzenleme/görüntüleme filmler ile yapabilecekleriniz doğrulayın bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="47052-156">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="47052-157">Veritabanı dağıtılan değil, IIS Express durdurun ve ardından uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="47052-157">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="47052-158">[Önceki: Arama ekleme](xref:tutorials/razor-pages/search)
> [sonraki: doğrulama ekleme](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="47052-158">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
