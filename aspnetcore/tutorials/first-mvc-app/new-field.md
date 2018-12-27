---
title: Bir ASP.NET Core MVC uygulaması için yeni bir alan ekleyin
author: rick-anderson
description: Bir model için yeni bir alan ekleyin ve bu değişiklik veritabanına geçirmek için Entity Framework Code First Migrations'ı kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/25/2018
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 3c29b01cc97dbfd93c21131c3d96ed309715da1a
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53382023"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="b5144-103">Bir ASP.NET Core MVC uygulaması için yeni bir alan ekleyin</span><span class="sxs-lookup"><span data-stu-id="b5144-103">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="b5144-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b5144-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b5144-105">Bu bölümdeki [Entity Framework](/ef/core/get-started/aspnetcore/new-db) için Code First Migrations kullanılır:</span><span class="sxs-lookup"><span data-stu-id="b5144-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="b5144-106">Modele yeni bir alan ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b5144-106">Add a new field to the model.</span></span>
* <span data-ttu-id="b5144-107">Yeni alan veritabanına geçirin.</span><span class="sxs-lookup"><span data-stu-id="b5144-107">Migrate the new field to the database.</span></span>

<span data-ttu-id="b5144-108">EF Code First için otomatik olarak kullanıldığında, Code First bir veritabanı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="b5144-108">When EF Code First is used to automatically create a database, Code First:</span></span>

* <span data-ttu-id="b5144-109">Bir tablo veritabanı şeması izlemek için veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="b5144-109">Adds a table to the database to  track the schema of the database.</span></span>
* <span data-ttu-id="b5144-110">Veritabanı öğesinden oluşturulan model sınıfları ile eşitlenmiş olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="b5144-110">Verify the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="b5144-111">EF, bunlar eşit değilse bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b5144-111">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="b5144-112">Bu, tutarsız veritabanı kod sorunlarını bulmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="b5144-112">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="add-a-rating-property-to-the-movie-model"></a><span data-ttu-id="b5144-113">Film modeli için bir derecelendirme özelliği Ekle</span><span class="sxs-lookup"><span data-stu-id="b5144-113">Add a Rating Property to the Movie Model</span></span>

<span data-ttu-id="b5144-114">Ekleme bir `Rating` özelliğini *Models/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="b5144-114">Add a `Rating` property to *Models/Movie.cs*:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="b5144-115">(Ctrl + Shift + B) uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b5144-115">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="b5144-116">Yeni bir alan eklediğiniz çünkü `Movie` sınıfı için gereksinim duyduğunuz bağlama beyaz liste bu yeni özellik dahil olacak şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="b5144-116">Because you've added a new field to the `Movie` class, you need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="b5144-117">İçinde *MoviesController.cs*, güncelleştirme `[Bind]` özniteliği hem de `Create` ve `Edit` dahil etmek için eylem yöntemleri `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="b5144-117">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="b5144-118">Görünüm şablonları görüntülemek, oluşturmak ve yeni düzenlemek için güncelleştirme `Rating` Tarayıcı Görünümü özelliği.</span><span class="sxs-lookup"><span data-stu-id="b5144-118">Update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="b5144-119">Düzen */Views/Movies/Index.cshtml* dosya ve ekleme bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="b5144-119">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="b5144-120">Güncelleştirme */Views/Movies/Create.cshtml* ile bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="b5144-120">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> 

<!-- VS -------------------------->
# <a name="visual-studio--visual-studio-for-mactabvisual-studiovisual-studio-mac"></a>[<span data-ttu-id="b5144-121">Visual Studio / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="b5144-121">Visual Studio / Visual Studio for Mac</span></span>](#tab/visual-studio+visual-studio-mac)

<span data-ttu-id="b5144-122">Önceki "form grubu" kopyala/yapıştır ve alanları güncelleştirin IntelliSense Yardım sağlar.</span><span class="sxs-lookup"><span data-stu-id="b5144-122">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="b5144-123">IntelliSense ile birlikte çalışır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="b5144-123">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Öznitelik değeri ASP R harfiyle Geliştirici yazdığını-için görünümün ikinci etiket öğesinde.](new-field/_static/cr.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b5144-127">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b5144-127">Visual Studio Code</span></span>](#tab/visual-studio-code)
<!-- This tab intentionally left blank. -->
---  
<!-- End of VS tabs -->

<span data-ttu-id="b5144-128">Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlar sınıfını.</span><span class="sxs-lookup"><span data-stu-id="b5144-128">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="b5144-129">Aşağıda bir örnek değişikliği gösterilmiştir, ancak her biri için bu değişikliği yapmak istersiniz `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="b5144-129">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="b5144-130">Uygulama DB yeni alanı içerecek şekilde güncelleştirilene kadar çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="b5144-130">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="b5144-131">Şimdi aşağıdaki çalıştırıldıysa `SqlException` oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="b5144-131">If it's run now, the following `SqlException` is thrown:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="b5144-132">Güncelleştirilmiş film model sınıfı var olan veritabanının film tablo şemasından farklı olduğundan, bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="b5144-132">This error occurs because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="b5144-133">(Yok hiçbir `Rating` veritabanı tablosundaki sütun.)</span><span class="sxs-lookup"><span data-stu-id="b5144-133">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="b5144-134">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="b5144-134">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="b5144-135">Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanını yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="b5144-135">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="b5144-136">Bu yaklaşım bir test veritabanında etkin geliştirme işi yaparken zaman Geliştirme döngüsünün başlarında çok kullanışlıdır; model ve veritabanı şeması birlikte hızla geliştirilebilen olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="b5144-136">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="b5144-137">Olumsuz tarafı, yine de veritabanında var olan veri kaybı olan — bir üretim veritabanında bu yaklaşımı kullanmak istemediğiniz şekilde!</span><span class="sxs-lookup"><span data-stu-id="b5144-137">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="b5144-138">Bir başlatıcı bir veritabanı test verileri ile otomatik olarak oluşturmak için genellikle bir uygulama geliştirmek için üretken bir şekilde kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="b5144-138">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="b5144-139">İyi bir yaklaşım erken geliştirmeye yönelik ve SQLite kullanırken budur.</span><span class="sxs-lookup"><span data-stu-id="b5144-139">This is a good approach for early development and when using SQLite.</span></span>

2. <span data-ttu-id="b5144-140">Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b5144-140">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="b5144-141">Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="b5144-141">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="b5144-142">Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b5144-142">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="b5144-143">Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="b5144-143">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="b5144-144">Bu öğreticide, Code First Migrations kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b5144-144">For this tutorial, Code First Migrations is used.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b5144-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b5144-145">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b5144-146">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="b5144-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![PMC menüsü](adding-model/_static/pmc.png)

<span data-ttu-id="b5144-148">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="b5144-148">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="b5144-149">`Add-Migration` Komut Framework'e geçerli incelemek için geçiş `Movie` geçerli model `Movie` DB şema ve DB yeni modeline geçirme için gereken kodu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b5144-149">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b5144-150">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="b5144-150">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="b5144-151">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b5144-151">Run the following command:</span></span>

```cli
dotnet ef migrations add Rating
dotnet ef database update
```

---  
<!-- End of VS tabs -->

<span data-ttu-id="b5144-152">Adı "Sıralama" isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b5144-152">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="b5144-153">Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="b5144-153">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="b5144-154">Veritabanındaki tüm kayıtları silinen, Initialize yöntemi DB çekirdeğini ve dahil `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="b5144-154">If all the records in the DB are deleted, the initialize method will seed the DB and include the `Rating` field.</span></span>

<span data-ttu-id="b5144-155">Uygulamayı çalıştırın ve kontrol edebilirsiniz oluşturma/düzenleme/görüntüleme filmlerle bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="b5144-155">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="b5144-156">Eklemeniz gerekir `Rating` alanı `Edit`, `Details`, ve `Delete` şablonlarını görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="b5144-156">You should add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b5144-157">[Önceki](search.md)
> [İleri](validation.md)</span><span class="sxs-lookup"><span data-stu-id="b5144-157">[Previous](search.md)
[Next](validation.md)</span></span>  
