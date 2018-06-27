---
title: Model bir ASP.NET Core MVC uygulamasına ekleme
author: rick-anderson
description: Bir model için basit bir ASP.NET Core uygulama ekleyin.
ms.author: riande
ms.date: 09/18/2017
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 82b8338f10cb4d58ae06bdb70583e1563c2e6b64
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961431"
---
[!INCLUDE [adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="ebe5a-103">Bir sınıfa eklemek *modelleri* adlı klasörü *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="ebe5a-103">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="ebe5a-104">Aşağıdaki kodu ekleyin *Models/Movie.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ebe5a-104">Add the following code to the *Models/Movie.cs* file:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="ebe5a-105">`ID` Alan veritabanı için birincil anahtarı gerekli.</span><span class="sxs-lookup"><span data-stu-id="ebe5a-105">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="ebe5a-106">Herhangi bir hata yoktur ve son olarak eklediğiniz doğrulamak üzere uygulamasını derleme bir **M**odel için **M**VC uygulama.</span><span class="sxs-lookup"><span data-stu-id="ebe5a-106">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="ebe5a-107">Proje için askılamayı hazırlama</span><span class="sxs-lookup"><span data-stu-id="ebe5a-107">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="ebe5a-108">Aşağıdaki vurgulanmış şekilde NuGet paketlerini eklemek *MvcMovie.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ebe5a-108">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   [!code-csharp[](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="ebe5a-109">Dosyayı kaydedin ve seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.</span><span class="sxs-lookup"><span data-stu-id="ebe5a-109">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="ebe5a-110">Oluşturma bir *Models/MvcMovieContext.cs* dosya ve aşağıdakileri ekleyin `MvcMovieContext` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ebe5a-110">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   [!code-csharp[](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- <span data-ttu-id="ebe5a-111">Açık *haline* dosya ve iki kullanımları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ebe5a-111">Open the *Startup.cs* file and add two usings:</span></span>

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- <span data-ttu-id="ebe5a-112">Veritabanı bağlamı Ekle *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ebe5a-112">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="ebe5a-113">Bu, hangi modeli sınıfları veri modelinde bulunan Entity Framework bildirir.</span><span class="sxs-lookup"><span data-stu-id="ebe5a-113">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="ebe5a-114">Bir tanımladığınız *varlık kümesini* veritabanında bir filmi tablo olarak temsil edilir film nesnelerin.</span><span class="sxs-lookup"><span data-stu-id="ebe5a-114">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="ebe5a-115">Bir hata doğrulamak için projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ebe5a-115">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="ebe5a-116">İskele MovieController</span><span class="sxs-lookup"><span data-stu-id="ebe5a-116">Scaffold the MovieController</span></span>

<span data-ttu-id="ebe5a-117">Proje klasöründe bir terminal penceresi açın ve aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ebe5a-117">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="ebe5a-118">Yapı iskelesi altyapısı aşağıdaki oluşturur:</span><span class="sxs-lookup"><span data-stu-id="ebe5a-118">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="ebe5a-119">Film denetleyicisi (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="ebe5a-119">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="ebe5a-120">Razor Görünüm Oluştur, Sil, ayrıntıları, düzenleme ve dizin sayfalar için dosyaları (*görünümler/filmler/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="ebe5a-120">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="ebe5a-121">Otomatik olarak oluşturulmasını [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve görünümler olarak bilinir *iskele*.</span><span class="sxs-lookup"><span data-stu-id="ebe5a-121">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="ebe5a-122">En kısa sürede bir filmi veritabanı yönetmenizi sağlayan bir tam olarak işlevsel bir web uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ebe5a-122">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="ebe5a-123">Bir veritabanı ve görüntüleme, düzenleme, güncelleştirme ve verileri silmek için sayfaları artık sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="ebe5a-123">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="ebe5a-124">Sonraki öğreticide biz veritabanı ile çalışması.</span><span class="sxs-lookup"><span data-stu-id="ebe5a-124">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="ebe5a-125">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ebe5a-125">Additional resources</span></span>

* [<span data-ttu-id="ebe5a-126">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="ebe5a-126">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="ebe5a-127">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="ebe5a-127">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="ebe5a-128">[Önceki - Görünüm ekleme](adding-view.md)
> [sonraki - SQLite ile çalışma](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="ebe5a-128">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
