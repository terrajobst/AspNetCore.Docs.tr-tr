---
title: Bir ASP.NET Core uygulama için yeni bir alan ekleyin
author: rick-anderson
description: Yeni bir alan bir modele eklemek ve bu değişikliği bir veritabanına geçirmek için Entity Framework Code First Migrations kullanmayı öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 0077205e0f10037c9b24eab80337cb76f027e688
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278784"
---
# <a name="add-a-new-field-to-an-aspnet-core-app"></a><span data-ttu-id="62c00-103">Bir ASP.NET Core uygulama için yeni bir alan ekleyin</span><span class="sxs-lookup"><span data-stu-id="62c00-103">Add a new field to an ASP.NET Core app</span></span>

<span data-ttu-id="62c00-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="62c00-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="62c00-105">Bu bölümde kullanacağınız [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) yeni bir alan modele eklemek ve bu geçirmek için Code First geçişleri veritabanına değiştirin.</span><span class="sxs-lookup"><span data-stu-id="62c00-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="62c00-106">EF Code First otomatik olarak bir veritabanı oluşturmak için kullandığınızda, Code First bir tablo veritabanı şeması öğesinden oluşturulduğu modeli sınıfları ile eşit olup olmadığını izlemenize yardımcı olması için veritabanına ekler.</span><span class="sxs-lookup"><span data-stu-id="62c00-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="62c00-107">EF eşitlenmiş durumda değilse, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="62c00-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="62c00-108">Bu, tutarsız veritabanı/kod sorunları bulmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="62c00-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="62c00-109">Film modeline derecelendirme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="62c00-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="62c00-110">Açık *Models/Movie.cs* dosya ve ekleme bir `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="62c00-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="62c00-111">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="62c00-111">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="62c00-112">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="62c00-112">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>
::: moniker-end

<span data-ttu-id="62c00-113">(Ctrl + Shift + B) uygulaması oluşturma.</span><span class="sxs-lookup"><span data-stu-id="62c00-113">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="62c00-114">Yeni bir alan eklediğiniz çünkü `Movie` sınıfı, siz de bu yeni özelliği eklenecek şekilde bağlama beyaz liste güncellemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="62c00-114">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="62c00-115">İçinde *MoviesController.cs*, güncelleştirme `[Bind]` özniteliği her ikisi için de `Create` ve `Edit` dahil etmek için eylem yöntemleri `Rating` özelliği:</span><span class="sxs-lookup"><span data-stu-id="62c00-115">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="62c00-116">Ayrıca görüntülemek, oluşturmak ve yeni düzenlemek için görünümü şablonlarını güncelleştirmek gereken `Rating` tarayıcı görünümünde özelliği.</span><span class="sxs-lookup"><span data-stu-id="62c00-116">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="62c00-117">Düzen */Views/Movies/Index.cshtml* dosya ve ekleme bir `Rating` alan:</span><span class="sxs-lookup"><span data-stu-id="62c00-117">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="62c00-118">Güncelleştirme */Views/Movies/Create.cshtml* ile bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="62c00-118">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="62c00-119">Önceki "form grubu" kopyalayıp yapıştırın ve alanları güncelleştirmeye IntelliSense Yardım sağlar.</span><span class="sxs-lookup"><span data-stu-id="62c00-119">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="62c00-120">IntelliSense çalışır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="62c00-120">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="62c00-121">Not: Visual Studio 2017'ın RTM sürümü yüklemeniz gerekir [Razor dili Hizmetleri](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) Razor IntelliSense için.</span><span class="sxs-lookup"><span data-stu-id="62c00-121">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="62c00-122">Bu bir sonraki sürümde düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="62c00-122">This will be fixed in the next release.</span></span>

![Geliştirici asp öznitelik değerini Harf R yazdı-için görünümün ikinci etiket öğesindeki.](new-field/_static/cr.png)

<span data-ttu-id="62c00-126">Uygulama, yeni alanı içerecek şekilde DB güncelleştiriyoruz kadar çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="62c00-126">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="62c00-127">Şimdi çalıştırırsanız, aşağıdaki elde edersiniz `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="62c00-127">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="62c00-128">Güncelleştirilmiş film model sınıfı var olan veritabanının film tablonun şeması farklı olduğundan bu hata görüyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="62c00-128">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="62c00-129">(Veritabanı tablosunda hiçbir Derecelendirme sütunu var.)</span><span class="sxs-lookup"><span data-stu-id="62c00-129">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="62c00-130">Hatayı çözümlemek için birkaç yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="62c00-130">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="62c00-131">Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanı yeniden oluşturma Entity Framework vardır.</span><span class="sxs-lookup"><span data-stu-id="62c00-131">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="62c00-132">Bu yaklaşım erken zaman, bir test veritabanı üzerindeki etkin geliştirme yapmakta olduğunuz geliştirme döngüsü çok kolaydır; hızlı bir şekilde modeli ve veritabanı şeması birlikte gelişmesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="62c00-132">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="62c00-133">Dezavantajı rağmen veritabanında var olan veri kaybı olan — bir üretim veritabanında bu yaklaşımı kullanmak istemediğiniz için!</span><span class="sxs-lookup"><span data-stu-id="62c00-133">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="62c00-134">Otomatik olarak test verileri olan bir veritabanı oluşturmak için bir başlatıcı kullanarak bir uygulama geliştirmek için verimli bir yol görülür.</span><span class="sxs-lookup"><span data-stu-id="62c00-134">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="62c00-135">Açıkça modeli sınıfları eşleşecek şekilde var olan veritabanının şema değiştirin.</span><span class="sxs-lookup"><span data-stu-id="62c00-135">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="62c00-136">Bu yaklaşımın avantajı, verilerinizi tutmak ' dir.</span><span class="sxs-lookup"><span data-stu-id="62c00-136">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="62c00-137">Bu değişikliği yapmak ya da el ile veya komut dosyası bir veritabanı oluşturarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="62c00-137">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="62c00-138">Veritabanı şeması güncelleştirmek için Code First Migrations kullanın.</span><span class="sxs-lookup"><span data-stu-id="62c00-138">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="62c00-139">Bu öğretici için Code First Migrations kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="62c00-139">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="62c00-140">Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlayan sınıf.</span><span class="sxs-lookup"><span data-stu-id="62c00-140">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="62c00-141">Bir örnek değişiklik aşağıda gösterilen, ancak her biri için bu değişikliği yapmak istersiniz `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="62c00-141">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="62c00-142">Çözümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="62c00-142">Build the solution.</span></span>

<span data-ttu-id="62c00-143">Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="62c00-143">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![PMC menüsü](adding-model/_static/pmc.png)

<span data-ttu-id="62c00-145">PMC aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="62c00-145">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="62c00-146">`Add-Migration` Komutu geçerli incelemek için geçiş framework bildirir `Movie` geçerli model `Movie` DB şeması ve yeni modele DB geçirmek için gerekli kodu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="62c00-146">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="62c00-147">Adı "Derecelendirme" rastgeledir ve geçiş dosya adı için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="62c00-147">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="62c00-148">Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="62c00-148">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="62c00-149">Veritabanındaki tüm kayıtları silerseniz, başlatma DB Çekirdek ve dahil `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="62c00-149">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="62c00-150">Tarayıcıda veya SSOX delete bağlantılar yapın.</span><span class="sxs-lookup"><span data-stu-id="62c00-150">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="62c00-151">Uygulamayı çalıştırın ve oluşturma/düzenleme/görüntüleme filmler ile yapabilecekleriniz doğrulayın bir `Rating` alan.</span><span class="sxs-lookup"><span data-stu-id="62c00-151">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="62c00-152">Ayrıca eklemelisiniz `Rating` alanı `Edit`, `Details`, ve `Delete` görüntüleme şablonları.</span><span class="sxs-lookup"><span data-stu-id="62c00-152">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="62c00-153">[Önceki](search.md)
> [sonraki](validation.md)</span><span class="sxs-lookup"><span data-stu-id="62c00-153">[Previous](search.md)
[Next](validation.md)</span></span>  
