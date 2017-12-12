---
title: "Yeni bir alan için bir Razor sayfası ekleme"
author: rick-anderson
description: "Razor sayfasını Entity Framework Çekirdek ile yeni bir alan eklemek nasıl gösterir"
keywords: "ASP.NET Core, Entity Framework Çekirdek geçişleri"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 128b69513976a56104524bb803f2b8cb1daf1967
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="b0d2d-104">Yeni bir alan için bir Razor sayfası ekleme</span><span class="sxs-lookup"><span data-stu-id="b0d2d-104">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="b0d2d-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b0d2d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b0d2d-106">Bu bölümde kullanacağınız [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) yeni bir alan modele eklemek ve bu geçirmek için Code First geçişleri veritabanına değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-106">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="b0d2d-107">EF Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda, Code First bir tablo veritabanı şeması öğesinden oluşturulduğu modeli sınıfları ile eşit olup olmadığını izlemenize yardımcı olması için veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-107">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="b0d2d-108">EF eşitlenmiş durumda değilse, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-108">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="b0d2d-109">Bu, tutarsız veritabanı/kod sorunları bulmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-109">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="b0d2d-110">Film modeline derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="b0d2d-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="b0d2d-111">Açık *Models/Movie.cs* dosya ve ekleme bir `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="b0d2d-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="b0d2d-112">(Ctrl + Shift + B) uygulaması oluşturma.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-112">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="b0d2d-113">Düzen *Pages/Movies/Index.cshtml*ve ekleme bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="b0d2d-113">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="b0d2d-114">Ekleme `Rating` silme ve ayrıntıları sayfaları alanı.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-114">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="b0d2d-115">Güncelleştirme *Create.cshtml* ile bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-115">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="b0d2d-116">Kopyalayıp önceki yapıştırın `<div>` alanları güncelleştir öğesi ve let IntelliSense Yardım.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-116">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="b0d2d-117">IntelliSense çalışır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="b0d2d-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Geliştirici asp öznitelik değerini Harf R yazdı-için görünümün ikinci etiket öğesindeki.](new-field/_static/cr.png)

<span data-ttu-id="b0d2d-121">Aşağıdaki kodda gösterildiği *Create.cshtml* ile bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="b0d2d-121">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="b0d2d-122">Ekleme `Rating` sayfasını Düzenle alanı.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-122">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="b0d2d-123">Uygulama DB yeni alanı içerecek şekilde güncelleştirilene kadar çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-123">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="b0d2d-124">Şimdi, uygulama atar çalıştırırsanız bir `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="b0d2d-124">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="b0d2d-125">Bu hata veritabanının film tablonun şeması farklı güncelleştirilmiş film model sınıfı kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-125">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="b0d2d-126">(Var. hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="b0d2d-126">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="b0d2d-127">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="b0d2d-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="b0d2d-128">Otomatik olarak bırakın ve yeni model sınıfı şeması kullanarak veritabanını yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-128">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="b0d2d-129">Bu yaklaşım, erken geliştirme döngüsü uygundur; hızlı bir şekilde modeli ve veritabanı şeması birlikte gelişmesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-129">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="b0d2d-130">Dezavantajı, veritabanında var olan veri kaybı ' dir.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-130">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="b0d2d-131">Bir üretim veritabanında bu yaklaşımı kullanmak istemiyorsanız!</span><span class="sxs-lookup"><span data-stu-id="b0d2d-131">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="b0d2d-132">Şema değişiklikleri DB bırakarak ve otomatik olarak test verileri ile veritabanı oluşturmak için bir başlatıcı kullanarak bir uygulama geliştirmek için verimli bir şekilde görülür.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-132">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="b0d2d-133">Açıkça modeli sınıfları eşleşecek şekilde var olan veritabanının şema değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="b0d2d-134">Bu yaklaşımın avantajı, verilerinizi tutmak ' dir.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="b0d2d-135">Bu değişikliği yapmak ya da el ile veya komut dosyası bir veritabanı oluşturarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="b0d2d-136">Veritabanı şeması güncelleştirmek için Code First Migrations kullanın.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="b0d2d-137">Bu öğretici için Code First Migrations kullanın.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-137">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="b0d2d-138">Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlayan sınıf.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="b0d2d-139">Bir örnek değişiklik aşağıda gösterilen, ancak her biri için bu değişikliği yapmak istersiniz `new Movie` bloğu.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-139">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="b0d2d-140">Bkz: [SeedData.cs dosya tamamlandı](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="b0d2d-140">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="b0d2d-141">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-141">Build the solution.</span></span>

<a name="pmc"></a><span data-ttu-id="b0d2d-142">Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-142">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="b0d2d-143">PMC aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="b0d2d-143">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="b0d2d-144">`Add-Migration` Komutu framework bildirir:</span><span class="sxs-lookup"><span data-stu-id="b0d2d-144">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="b0d2d-145">Karşılaştırma `Movie` ile model `Movie` DB şeması.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-145">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="b0d2d-146">Yeni modele DB şeması geçirmek için kod oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-146">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="b0d2d-147">Adı "Derecelendirme" rastgeledir ve geçiş dosya adı için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-147">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="b0d2d-148">Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-148">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a><span data-ttu-id="b0d2d-149">Veritabanındaki tüm kayıtları silerseniz, başlatıcı DB Çekirdek ve dahil `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="b0d2d-150">Tarayıcıda veya gelen silme bağlantılarla bunu yapabilirsiniz [Sql Server Nesne Gezgini](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="b0d2d-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="b0d2d-151">SSOX veritabanını silmek için:</span><span class="sxs-lookup"><span data-stu-id="b0d2d-151">To delete the database from SSOX:</span></span>

* <span data-ttu-id="b0d2d-152">Veritabanı içinde SSOX seçin.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-152">Select the database in SSOX.</span></span>
* <span data-ttu-id="b0d2d-153">Veritabanını sağ tıklatın ve seçin *silmek*.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-153">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="b0d2d-154">Denetleme **var olan bağlantıları kapatın**.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-154">Check **Close existing connections**.</span></span>
* <span data-ttu-id="b0d2d-155">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-155">Select **OK**.</span></span>
* <span data-ttu-id="b0d2d-156">İçinde [PMC](xref:tutorials/razor-pages/new-field#pmc), veritabanını güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b0d2d-156">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="b0d2d-157">Uygulamayı çalıştırın ve oluşturma/düzenleme/görüntüleme filmler ile yapabilecekleriniz doğrulayın bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-157">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="b0d2d-158">Veritabanı sağlanmış olmadığı, IIS Express durdurun ve ardından uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b0d2d-158">If the database is not seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b0d2d-159">[Önceki: Arama ekleme](xref:tutorials/razor-pages/search)
[sonraki: doğrulama ekleme](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="b0d2d-159">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
