---
title: "Bir model, bir ASP.NET Core MVC uygulamasına ekleniyor."
author: rick-anderson
description: "Bir model için basit bir ASP.NET Core uygulama ekleyin."
manager: wpickett
ms.author: riande
ms.date: 09/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 81511b05a3cc11a58b93452d3c6e5305e7ee4357
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="9f5ff-103">Bir sınıfa eklemek *modelleri* adlı klasörü *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="9f5ff-103">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="9f5ff-104">Aşağıdaki kodu ekleyin *Models/Movie.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9f5ff-104">Add the following code to the *Models/Movie.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="9f5ff-105">`ID` Alan veritabanı için birincil anahtarı gerekli.</span><span class="sxs-lookup"><span data-stu-id="9f5ff-105">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="9f5ff-106">Herhangi bir hata yoktur ve son olarak eklediğiniz doğrulamak üzere uygulamasını derleme bir **M**odel için **M**VC uygulama.</span><span class="sxs-lookup"><span data-stu-id="9f5ff-106">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="9f5ff-107">Proje için askılamayı hazırlama</span><span class="sxs-lookup"><span data-stu-id="9f5ff-107">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="9f5ff-108">Aşağıdaki vurgulanmış şekilde NuGet paketlerini eklemek *MvcMovie.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9f5ff-108">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="9f5ff-109">Dosyayı kaydedin ve seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.</span><span class="sxs-lookup"><span data-stu-id="9f5ff-109">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="9f5ff-110">Oluşturma bir *Models/MvcMovieContext.cs* dosya ve aşağıdakileri ekleyin `MvcMovieContext` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="9f5ff-110">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- <span data-ttu-id="9f5ff-111">Açık *haline* dosya ve iki kullanımları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9f5ff-111">Open the *Startup.cs* file and add two usings:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- <span data-ttu-id="9f5ff-112">Veritabanı bağlamı Ekle *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="9f5ff-112">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="9f5ff-113">Bu, hangi modeli sınıfları veri modelinde bulunan Entity Framework bildirir.</span><span class="sxs-lookup"><span data-stu-id="9f5ff-113">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="9f5ff-114">Bir tanımladığınız *varlık kümesini* veritabanında bir filmi tablo olarak temsil edilir film nesnelerin.</span><span class="sxs-lookup"><span data-stu-id="9f5ff-114">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="9f5ff-115">Bir hata doğrulamak için projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9f5ff-115">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="9f5ff-116">İskele MovieController</span><span class="sxs-lookup"><span data-stu-id="9f5ff-116">Scaffold the MovieController</span></span>

<span data-ttu-id="9f5ff-117">Proje klasöründe bir terminal penceresi açın ve aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9f5ff-117">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="9f5ff-118">Yapı iskelesi altyapısı aşağıdaki oluşturur:</span><span class="sxs-lookup"><span data-stu-id="9f5ff-118">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="9f5ff-119">Film denetleyicisi (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="9f5ff-119">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="9f5ff-120">Razor Görünüm Oluştur, Sil, ayrıntıları, düzenleme ve dizin sayfalar için dosyaları (*görünümler/filmler/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="9f5ff-120">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="9f5ff-121">Otomatik olarak oluşturulmasını [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve görünümler olarak bilinir *iskele*.</span><span class="sxs-lookup"><span data-stu-id="9f5ff-121">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="9f5ff-122">En kısa sürede bir filmi veritabanı yönetmenizi sağlayan bir tam olarak işlevsel bir web uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9f5ff-122">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="9f5ff-123">Bir veritabanı ve görüntüleme, düzenleme, güncelleştirme ve verileri silmek için sayfaları artık sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="9f5ff-123">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="9f5ff-124">Sonraki öğreticide biz veritabanı ile çalışması.</span><span class="sxs-lookup"><span data-stu-id="9f5ff-124">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="9f5ff-125">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9f5ff-125">Additional resources</span></span>

* [<span data-ttu-id="9f5ff-126">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="9f5ff-126">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="9f5ff-127">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="9f5ff-127">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="9f5ff-128">[Önceki - Görünüm ekleme](adding-view.md)
[sonraki - SQLite ile çalışma](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="9f5ff-128">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
