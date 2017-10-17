---
title: "Bir ASP.NET Core MVC uygulamasına bir modeli ekleme"
author: rick-anderson
description: "Bir model için basit bir ASP.NET Core uygulama ekleyin."
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 7469546494ec54bfe36bc5bd2f5f9702889ddf4a
ms.sourcegitcommit: 2e61e287e220eddd5f3f4cd9147aa6417cfd9236
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="8107c-104">Not: ASP.NET Core 2.0 şablonlarını içeren *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="8107c-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="8107c-105">Çözüm Gezgini'nde sağ tıklayın **MvcMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="8107c-105">In Solution Explorer, right click the **MvcMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="8107c-106">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="8107c-106">Name the folder *Models*.</span></span>

<span data-ttu-id="8107c-107">Sağ tıklayın *modelleri* klasörü > **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="8107c-107">Right click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="8107c-108">Sınıf adını **film** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8107c-108">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="8107c-109">`ID` Alan veritabanı için birincil anahtarı gerekli.</span><span class="sxs-lookup"><span data-stu-id="8107c-109">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="8107c-110">Herhangi bir hata yoksa doğrulamak için projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8107c-110">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="8107c-111">Artık elinizde bir **M**odeldeki, **M**VC uygulama.</span><span class="sxs-lookup"><span data-stu-id="8107c-111">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="8107c-112">Bir denetleyici iskele kurma</span><span class="sxs-lookup"><span data-stu-id="8107c-112">Scaffolding a controller</span></span>

<span data-ttu-id="8107c-113">İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörü **> Ekle > denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="8107c-113">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller.png)

<span data-ttu-id="8107c-115">İçinde **MVC bağımlılıkları ekleyin** iletişim kutusunda **en az bağımlılıkları**seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="8107c-115">In the **Add MVC Dependencies** dialog, select **Minimal Dependencies**, and select **Add**.</span></span>

![Yukarıdaki adımı görünümü](adding-model/_static/add_depend.png)

<span data-ttu-id="8107c-117">Visual Studio bir denetleyici iskele için gerekli bağımlılıkların ekler, ancak denetleyicisi oluşturulmadı.</span><span class="sxs-lookup"><span data-stu-id="8107c-117">Visual Studio adds the dependencies needed to scaffold a controller, but the controller itself is not created.</span></span> <span data-ttu-id="8107c-118">Sonraki Invoke **> Ekle > denetleyicisi** denetleyiciyi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8107c-118">The next invoke of **> Add > Controller** creates the controller.</span></span> 

<span data-ttu-id="8107c-119">İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörü **> Ekle > denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="8107c-119">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller.png)

<span data-ttu-id="8107c-121">İçinde **İskele Ekle** iletişim, dokunun **görünümleri ile MVC Entity Framework kullanarak denetleyicisi > Ekle**.</span><span class="sxs-lookup"><span data-stu-id="8107c-121">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![İskele iletişim ekleyin](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="8107c-123">Tamamlamak **denetleyici Ekle** iletişim:</span><span class="sxs-lookup"><span data-stu-id="8107c-123">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="8107c-124">**Model sınıfı:** *film (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="8107c-124">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="8107c-125">**Veri bağlamı sınıfı:** seçin  **+**  simgesini ve varsayılan ekleyin **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="8107c-125">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Veri bağlamı Ekle](adding-model/_static/dc.png)

* <span data-ttu-id="8107c-127">**Görünümleri:** varsayılan her seçeneğinin işaretli koru</span><span class="sxs-lookup"><span data-stu-id="8107c-127">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="8107c-128">**Denetleyici adı:** varsayılan tutmak *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="8107c-128">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="8107c-129">Dokunun **Ekle**</span><span class="sxs-lookup"><span data-stu-id="8107c-129">Tap **Add**</span></span>

![Denetleyici Ekle iletişim kutusu](adding-model/_static/add_controller2.png)

<span data-ttu-id="8107c-131">Visual Studio oluşturur:</span><span class="sxs-lookup"><span data-stu-id="8107c-131">Visual Studio creates:</span></span>

* <span data-ttu-id="8107c-132">Bir Entity Framework Çekirdek [veritabanı bağlamı sınıfının](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="8107c-132">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="8107c-133">Film denetleyicisi (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="8107c-133">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="8107c-134">Razor Görünüm Oluştur, Sil, ayrıntıları, düzenleme ve dizin sayfalar için dosyaları (*görünümler/filmler/&ast;.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="8107c-134">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="8107c-135">Veritabanı bağlamı otomatik olarak oluşturulmasını ve [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve görünümler olarak bilinir *iskele*.</span><span class="sxs-lookup"><span data-stu-id="8107c-135">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="8107c-136">En kısa sürede bir filmi veritabanı yönetmenizi sağlayan bir tam olarak işlevsel bir web uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8107c-136">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="8107c-137">Uygulamayı çalıştırın ve tıklayın, **Mvc film** bağlantı, alacaksınız hata aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="8107c-137">If you run the app and click on the **Mvc Movie** link, you'll get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="8107c-138">Veritabanını oluşturmak gereken ve EF çekirdek kullanacağınız [geçişler](xref:data/ef-mvc/migrations) Bunu yapmak için özellik.</span><span class="sxs-lookup"><span data-stu-id="8107c-138">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="8107c-139">Geçişler veri modelinizi eşleşen bir veritabanı oluşturun ve verilerinizi değişiklikleri model veritabanı şemasını güncelleştirmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="8107c-139">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="8107c-140">EF araç ekleyin ve başlangıç geçiş gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="8107c-140">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="8107c-141">Bu bölümde için Paket Yöneticisi Konsolu (PMC) kullanmanız:</span><span class="sxs-lookup"><span data-stu-id="8107c-141">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="8107c-142">Entity Framework Çekirdek araçları paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8107c-142">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="8107c-143">Bu paket, geçişler eklemek ve veritabanını güncelleştirmek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="8107c-143">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="8107c-144">İlk geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8107c-144">Add an initial migration.</span></span>
* <span data-ttu-id="8107c-145">Veritabanı ile ilk geçiş güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="8107c-145">Update the database with the initial migration.</span></span>

<span data-ttu-id="8107c-146">Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="8107c-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC menüsü](adding-model/_static/pmc.png)

<span data-ttu-id="8107c-148">PMC aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="8107c-148">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="8107c-149">**Not:** ile bir hata alırsanız, `Install-Package` komutu, NuGet Paket Yöneticisi'ni açın ve arama `Microsoft.EntityFrameworkCore.Tools` paket.</span><span class="sxs-lookup"><span data-stu-id="8107c-149">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="8107c-150">Bu, paket yüklemek veya zaten yüklü olup olmadığını denetlemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="8107c-150">This allows you to install the package or check if it is already installed.</span></span> <span data-ttu-id="8107c-151">Alternatif olarak, bkz: [CLI yaklaşım](#cli) PMC sorununuz varsa.</span><span class="sxs-lookup"><span data-stu-id="8107c-151">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="8107c-152">`Add-Migration` Komutu ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8107c-152">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="8107c-153">Belirtilen model şeması dayalı `DbContext`(içinde *Data/MvcMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="8107c-153">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="8107c-154">`Initial` Bağımsız değişkeni geçişler adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8107c-154">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="8107c-155">Herhangi bir ad kullanabilirsiniz, ancak kurala göre geçiş açıklayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="8107c-155">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="8107c-156">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8107c-156">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="8107c-157">`Update-Database` Komutu çalıştırır `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosyası bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8107c-157">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="8107c-158"><a name="cli"></a>Komut satırı arabirimi (CLI) kullanarak önceki adımları yerine PMC gerçekleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="8107c-158"><a name="cli"></a> You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="8107c-159">Ekleme [EF çekirdek araç](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) için *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="8107c-159">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="8107c-160">(Proje dizininde) konsolundan aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="8107c-160">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Kimliği, fiyat, yayın tarihi ve başlık için kullanılabilir özellikleri listeleyen bir Model öğesi bağlam menüsünde IntelliSense](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="8107c-162">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8107c-162">Additional resources</span></span>

* [<span data-ttu-id="8107c-163">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="8107c-163">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="8107c-164">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="8107c-164">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="8107c-165">[Önceki bir görünümü ekleme](adding-view.md)
[sonraki SQL ile çalışma](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="8107c-165">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
