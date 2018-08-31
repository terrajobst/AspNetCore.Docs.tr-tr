---
title: Bir ASP.NET Core MVC uygulaması için yeni bir alan ekleyin
author: rick-anderson
description: Bir model için yeni bir alan ekleyin ve bu değişiklik veritabanına geçirmek için Entity Framework Code First Migrations'ı kullanmayı öğrenin.
ms.author: riande
ms.date: 10/06/2017
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 74f7a98143c80504d534c5ee4fd06b3dd076a2f2
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312237"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="bbad2-103">Bir ASP.NET Core MVC uygulaması için yeni bir alan ekleyin</span><span class="sxs-lookup"><span data-stu-id="bbad2-103">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="bbad2-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bbad2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bbad2-105">Bu bölümde kullanacaksınız [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) veritabanına Code First Migrations'modeli için yeni bir alan ekleme ve bu geçiş için değiştirin.</span><span class="sxs-lookup"><span data-stu-id="bbad2-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="bbad2-106">EF Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda, Code First bir tablo veritabanı şeması öğesinden oluşturulan model sınıfları ile eşitlenmiş olup olmadığını izlenmesine yardımcı olması için veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="bbad2-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="bbad2-107">EF, bunlar eşit değilse bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bbad2-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="bbad2-108">Bu, tutarsız veritabanı kod sorunlarını bulmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="bbad2-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="bbad2-109">Film modeli derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="bbad2-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="bbad2-110">Açık *Models/Movie.cs* dosya ve ekleme bir `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="bbad2-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]
::: moniker-end

<span data-ttu-id="bbad2-111">(Ctrl + Shift + B) uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bbad2-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="bbad2-112">Yeni bir alan eklediğiniz çünkü `Movie` sınıfı da gereken bağlama beyaz liste bu yeni özellik dahil olacak şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="bbad2-112">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="bbad2-113">İçinde *MoviesController.cs*, güncelleştirme `[Bind]` özniteliği hem de `Create` ve `Edit` dahil etmek için eylem yöntemleri `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="bbad2-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="bbad2-114">Ayrıca görüntülemek, oluşturmak ve bunları yeni düzenleme görünümü şablonları güncelleştirmeye gerek duyduğunuz `Rating` Tarayıcı Görünümü özelliği.</span><span class="sxs-lookup"><span data-stu-id="bbad2-114">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="bbad2-115">Düzen */Views/Movies/Index.cshtml* dosya ve ekleme bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="bbad2-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="bbad2-116">Güncelleştirme */Views/Movies/Create.cshtml* ile bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="bbad2-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="bbad2-117">Önceki "form grubu" kopyala/yapıştır ve alanları güncelleştirin IntelliSense Yardım sağlar.</span><span class="sxs-lookup"><span data-stu-id="bbad2-117">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="bbad2-118">IntelliSense ile birlikte çalışır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="bbad2-118">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="bbad2-119">Not: Visual Studio 2017'in RTM sürümü yüklemeniz gerekir [Razor dil Hizmetleri](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) Razor IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="bbad2-119">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="bbad2-120">Bu, sonraki sürümde düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="bbad2-120">This will be fixed in the next release.</span></span>

![Öznitelik değeri ASP R harfiyle Geliştirici yazdığını-için görünümün ikinci etiket öğesinde.](new-field/_static/cr.png)

<span data-ttu-id="bbad2-124">Uygulama, yeni alan eklemek için bir veritabanı güncelleştiriyoruz kadar çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="bbad2-124">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="bbad2-125">Şimdi çalıştırırsanız, aşağıdaki elde edecekleriniz `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="bbad2-125">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="bbad2-126">Güncelleştirilmiş film modeli sınıfı var olan veritabanının film tablonun şeması farklı olduğu için bu hatayı görüyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="bbad2-126">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="bbad2-127">(Veritabanı tablosunda hiçbir derecesi sütun var.)</span><span class="sxs-lookup"><span data-stu-id="bbad2-127">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="bbad2-128">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="bbad2-128">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="bbad2-129">Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanını yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="bbad2-129">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="bbad2-130">Bu yaklaşım bir test veritabanında etkin geliştirme işi yaparken zaman Geliştirme döngüsünün başlarında çok kullanışlıdır; model ve veritabanı şeması birlikte hızla geliştirilebilen olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="bbad2-130">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="bbad2-131">Olumsuz tarafı, yine de veritabanında var olan veri kaybı olan — bir üretim veritabanında bu yaklaşımı kullanmak istemediğiniz şekilde!</span><span class="sxs-lookup"><span data-stu-id="bbad2-131">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="bbad2-132">Bir başlatıcı bir veritabanı test verileri ile otomatik olarak oluşturmak için genellikle bir uygulama geliştirmek için üretken bir şekilde kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="bbad2-132">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="bbad2-133">Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin.</span><span class="sxs-lookup"><span data-stu-id="bbad2-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="bbad2-134">Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="bbad2-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="bbad2-135">Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.</span><span class="sxs-lookup"><span data-stu-id="bbad2-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="bbad2-136">Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="bbad2-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="bbad2-137">Bu öğreticide, Code First Migrations kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="bbad2-137">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="bbad2-138">Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlar sınıfını.</span><span class="sxs-lookup"><span data-stu-id="bbad2-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="bbad2-139">Aşağıda bir örnek değişikliği gösterilmiştir, ancak her biri için bu değişikliği yapmak istersiniz `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="bbad2-139">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="bbad2-140">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bbad2-140">Build the solution.</span></span>

<span data-ttu-id="bbad2-141">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="bbad2-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![PMC menüsü](adding-model/_static/pmc.png)

<span data-ttu-id="bbad2-143">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="bbad2-143">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="bbad2-144">`Add-Migration` Komut Framework'e geçerli incelemek için geçiş `Movie` geçerli model `Movie` DB şema ve DB yeni modeline geçirme için gereken kodu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bbad2-144">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="bbad2-145">Adı "Sıralama" isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bbad2-145">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="bbad2-146">Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="bbad2-146">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="bbad2-147">DB tüm kayıtların silerseniz, başlatma DB çekirdeğini ve dahil `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="bbad2-147">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="bbad2-148">Tarayıcıda veya SSOX silme bağlantıları kullanarak bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bbad2-148">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="bbad2-149">Uygulamayı çalıştırın ve kontrol edebilirsiniz oluşturma/düzenleme/görüntüleme filmlerle bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="bbad2-149">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="bbad2-150">De eklemeniz gerekir `Rating` alanı `Edit`, `Details`, ve `Delete` şablonlarını görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="bbad2-150">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bbad2-151">[Önceki](search.md)
> [İleri](validation.md)</span><span class="sxs-lookup"><span data-stu-id="bbad2-151">[Previous](search.md)
[Next](validation.md)</span></span>  
