---
title: Bir ASP.NET Core MVC uygulaması için yeni bir alan ekleyin
author: rick-anderson
description: Bir model için yeni bir alan ekleyin ve bu değişiklik veritabanına geçirmek için Entity Framework Code First Migrations'ı kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: cf8bb67703b564a711105123117498c94ab44e68
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750517"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="714eb-103">Bir ASP.NET Core MVC uygulaması için yeni bir alan ekleyin</span><span class="sxs-lookup"><span data-stu-id="714eb-103">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="714eb-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="714eb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="714eb-105">Bu bölümdeki [Entity Framework](/ef/core/get-started/aspnetcore/new-db) için Code First Migrations kullanılır:</span><span class="sxs-lookup"><span data-stu-id="714eb-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="714eb-106">Modele yeni bir alan ekleyin.</span><span class="sxs-lookup"><span data-stu-id="714eb-106">Add a new field to the model.</span></span>
* <span data-ttu-id="714eb-107">Yeni alan veritabanına geçirin.</span><span class="sxs-lookup"><span data-stu-id="714eb-107">Migrate the new field to the database.</span></span>

<span data-ttu-id="714eb-108">EF Code First için otomatik olarak kullanıldığında, Code First bir veritabanı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="714eb-108">When EF Code First is used to automatically create a database, Code First:</span></span>

* <span data-ttu-id="714eb-109">Bir tablo veritabanı şeması izlemek için veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="714eb-109">Adds a table to the database to  track the schema of the database.</span></span>
* <span data-ttu-id="714eb-110">Veritabanı öğesinden oluşturulan model sınıfları ile eşitlenmiş olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="714eb-110">Verify the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="714eb-111">EF, bunlar eşit değilse bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="714eb-111">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="714eb-112">Bu, tutarsız veritabanı kod sorunlarını bulmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="714eb-112">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="add-a-rating-property-to-the-movie-model"></a><span data-ttu-id="714eb-113">Film modeli için bir derecelendirme özelliği Ekle</span><span class="sxs-lookup"><span data-stu-id="714eb-113">Add a Rating Property to the Movie Model</span></span>

<span data-ttu-id="714eb-114">Ekleme bir `Rating` özelliğini *Models/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="714eb-114">Add a `Rating` property to *Models/Movie.cs*:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="714eb-115">(Ctrl + Shift + B) uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="714eb-115">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="714eb-116">Yeni bir alan eklediğiniz çünkü `Movie` sınıfı için gereksinim duyduğunuz bağlama beyaz liste bu yeni özellik dahil olacak şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="714eb-116">Because you've added a new field to the `Movie` class, you need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="714eb-117">İçinde *MoviesController.cs*, güncelleştirme `[Bind]` özniteliği hem de `Create` ve `Edit` dahil etmek için eylem yöntemleri `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="714eb-117">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("Id,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="714eb-118">Görünüm şablonları görüntülemek, oluşturmak ve yeni düzenlemek için güncelleştirme `Rating` Tarayıcı Görünümü özelliği.</span><span class="sxs-lookup"><span data-stu-id="714eb-118">Update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="714eb-119">Düzen */Views/Movies/Index.cshtml* dosya ve ekleme bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="714eb-119">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGenreRating.cshtml?highlight=16,38&range=24-64)]

<span data-ttu-id="714eb-120">Güncelleştirme */Views/Movies/Create.cshtml* ile bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="714eb-120">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

# <a name="visual-studio--visual-studio-for-mactabvisual-studiovisual-studio-mac"></a>[<span data-ttu-id="714eb-121">Visual Studio / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="714eb-121">Visual Studio / Visual Studio for Mac</span></span>](#tab/visual-studio+visual-studio-mac)

<span data-ttu-id="714eb-122">Önceki "form grubu" kopyala/yapıştır ve alanları güncelleştirin IntelliSense Yardım sağlar.</span><span class="sxs-lookup"><span data-stu-id="714eb-122">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="714eb-123">IntelliSense ile birlikte çalışır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="714eb-123">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Öznitelik değeri ASP R harfiyle Geliştirici yazdığını-için görünümün ikinci etiket öğesinde.](new-field/_static/cr.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="714eb-127">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="714eb-127">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- This tab intentionally left blank. -->

---

<span data-ttu-id="714eb-128">Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlar sınıfını.</span><span class="sxs-lookup"><span data-stu-id="714eb-128">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="714eb-129">Aşağıda bir örnek değişikliği gösterilmiştir, ancak her biri için bu değişikliği yapmak istersiniz `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="714eb-129">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="714eb-130">Uygulama DB yeni alanı içerecek şekilde güncelleştirilene kadar çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="714eb-130">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="714eb-131">Şimdi aşağıdaki çalıştırıldıysa `SqlException` oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="714eb-131">If it's run now, the following `SqlException` is thrown:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="714eb-132">Güncelleştirilmiş film model sınıfı var olan veritabanının film tablo şemasından farklı olduğundan, bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="714eb-132">This error occurs because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="714eb-133">(Yok hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="714eb-133">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="714eb-134">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="714eb-134">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="714eb-135">Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanını yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="714eb-135">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="714eb-136">Bu yaklaşım bir test veritabanında etkin geliştirme işi yaparken zaman Geliştirme döngüsünün başlarında çok kullanışlıdır; model ve veritabanı şeması birlikte hızla geliştirilebilen olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="714eb-136">This approach is very convenient early in the development cycle when you're doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="714eb-137">Olumsuz tarafı, yine de veritabanında var olan veri kaybı olan — bir üretim veritabanında bu yaklaşımı kullanmak istemediğiniz şekilde!</span><span class="sxs-lookup"><span data-stu-id="714eb-137">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="714eb-138">Bir başlatıcı bir veritabanı test verileri ile otomatik olarak oluşturmak için genellikle bir uygulama geliştirmek için üretken bir şekilde kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="714eb-138">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="714eb-139">İyi bir yaklaşım erken geliştirmeye yönelik ve SQLite kullanırken budur.</span><span class="sxs-lookup"><span data-stu-id="714eb-139">This is a good approach for early development and when using SQLite.</span></span>

2. <span data-ttu-id="714eb-140">Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin.</span><span class="sxs-lookup"><span data-stu-id="714eb-140">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="714eb-141">Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="714eb-141">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="714eb-142">Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.</span><span class="sxs-lookup"><span data-stu-id="714eb-142">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="714eb-143">Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="714eb-143">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="714eb-144">Bu öğreticide, Code First Migrations kullanılır.</span><span class="sxs-lookup"><span data-stu-id="714eb-144">For this tutorial, Code First Migrations is used.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="714eb-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="714eb-145">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="714eb-146">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="714eb-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![PMC menüsü](adding-model/_static/pmc.png)

<span data-ttu-id="714eb-148">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="714eb-148">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="714eb-149">`Add-Migration` Komut Framework'e geçerli incelemek için geçiş `Movie` geçerli model `Movie` DB şema ve DB yeni modeline geçirme için gereken kodu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="714eb-149">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span>

<span data-ttu-id="714eb-150">Adı "Sıralama" isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="714eb-150">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="714eb-151">Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="714eb-151">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="714eb-152">Veritabanındaki tüm kayıtları silinen, Initialize yöntemi DB çekirdeğini ve dahil `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="714eb-152">If all the records in the DB are deleted, the initialize method will seed the DB and include the `Rating` field.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="714eb-153">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="714eb-153">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="714eb-154">Veritabanını silin ve veritabanını yeniden oluşturmaya geçişleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="714eb-154">Delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="714eb-155">Veritabanını silmek için veritabanı dosyasını silin (*MvcMovie.db*).</span><span class="sxs-lookup"><span data-stu-id="714eb-155">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="714eb-156">Ardından çalıştırın `ef database update` komutu:</span><span class="sxs-lookup"><span data-stu-id="714eb-156">Then run the `ef database update` command:</span></span>

```console
dotnet ef database update
```

---
<!-- End of VS tabs -->

<span data-ttu-id="714eb-157">Uygulamayı çalıştırın ve kontrol edebilirsiniz oluşturma/düzenleme/görüntüleme filmlerle bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="714eb-157">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="714eb-158">Eklemeniz gerekir `Rating` alanı `Edit`, `Details`, ve `Delete` şablonlarını görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="714eb-158">You should add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="714eb-159">[Önceki](search.md)
> [İleri](validation.md)</span><span class="sxs-lookup"><span data-stu-id="714eb-159">[Previous](search.md)
[Next](validation.md)</span></span>
