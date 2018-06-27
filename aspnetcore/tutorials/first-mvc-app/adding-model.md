---
title: Model bir ASP.NET Core MVC uygulamasına ekleme
author: rick-anderson
description: Bir model için basit bir ASP.NET Core uygulama ekleyin.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 28a63498bc1a3c7b6ad6be038209dacdb49e44ee
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960674"
---
[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="55bf9-103">Sağ *modelleri* klasörü > **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="55bf9-103">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="55bf9-104">Sınıf adını **film** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="55bf9-104">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="55bf9-105">`ID` Alan veritabanı için birincil anahtarı gerekli.</span><span class="sxs-lookup"><span data-stu-id="55bf9-105">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="55bf9-106">Herhangi bir hata yoksa doğrulamak için projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="55bf9-106">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="55bf9-107">Artık elinizde bir **M**odeldeki, **M**VC uygulama.</span><span class="sxs-lookup"><span data-stu-id="55bf9-107">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="55bf9-108">Bir denetleyici iskele kurma</span><span class="sxs-lookup"><span data-stu-id="55bf9-108">Scaffolding a controller</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="55bf9-109">İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörü **> Ekle > Yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="55bf9-109">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller21.png)

<span data-ttu-id="55bf9-111">İçinde **İskele Ekle** iletişim, dokunun **görünümleri ile MVC Entity Framework kullanarak denetleyicisi > Ekle**.</span><span class="sxs-lookup"><span data-stu-id="55bf9-111">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![İskele iletişim ekleyin](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="55bf9-113">İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasörü **> Ekle > denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="55bf9-113">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller.png)

<span data-ttu-id="55bf9-115">Varsa **MVC bağımlılıkları ekleyin** iletişim kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="55bf9-115">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="55bf9-116">[Visual Studio en son sürüme güncelleştirin](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="55bf9-116">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="55bf9-117">Visual Studio sürümleri 15,5 önce bu iletişim kutusunu göster.</span><span class="sxs-lookup"><span data-stu-id="55bf9-117">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="55bf9-118">Update yapılamıyor, seçin **Ekle**ve ardından Ekle denetleyicisi adımları yeniden uygulayın.</span><span class="sxs-lookup"><span data-stu-id="55bf9-118">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="55bf9-119">İçinde **İskele Ekle** iletişim, dokunun **görünümleri ile MVC Entity Framework kullanarak denetleyicisi > Ekle**.</span><span class="sxs-lookup"><span data-stu-id="55bf9-119">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![İskele iletişim ekleyin](adding-model/_static/add_scaffold2.png)

::: moniker-end

<span data-ttu-id="55bf9-121">Tamamlamak **denetleyici Ekle** iletişim:</span><span class="sxs-lookup"><span data-stu-id="55bf9-121">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="55bf9-122">**Model sınıfı:** *film (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="55bf9-122">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="55bf9-123">**Veri bağlamı sınıfı:** seçin **+** simgesini ve varsayılan ekleyin **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="55bf9-123">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Veri bağlamı Ekle](adding-model/_static/dc.png)

* <span data-ttu-id="55bf9-125">**Görünümleri:** varsayılan her seçeneğinin işaretli koru</span><span class="sxs-lookup"><span data-stu-id="55bf9-125">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="55bf9-126">**Denetleyici adı:** varsayılan tutmak *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="55bf9-126">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="55bf9-127">Dokunun **Ekle**</span><span class="sxs-lookup"><span data-stu-id="55bf9-127">Tap **Add**</span></span>

![Denetleyici Ekle iletişim kutusu](adding-model/_static/add_controller2.png)

<span data-ttu-id="55bf9-129">Visual Studio oluşturur:</span><span class="sxs-lookup"><span data-stu-id="55bf9-129">Visual Studio creates:</span></span>

* <span data-ttu-id="55bf9-130">Bir Entity Framework Çekirdek [veritabanı bağlamı sınıfının](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="55bf9-130">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="55bf9-131">Film denetleyicisi (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="55bf9-131">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="55bf9-132">Razor Görünüm Oluştur, Sil, ayrıntıları, düzenleme ve dizin sayfalar için dosyaları (<em>görünümler/filmler/&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="55bf9-132">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="55bf9-133">Veritabanı bağlamı otomatik olarak oluşturulmasını ve [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve görünümler olarak bilinir *iskele*.</span><span class="sxs-lookup"><span data-stu-id="55bf9-133">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="55bf9-134">En kısa sürede bir filmi veritabanı yönetmenizi sağlayan bir tam olarak işlevsel bir web uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55bf9-134">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="55bf9-135">Uygulamayı çalıştırın ve tıklayın, **Mvc film** bağlantı, aldığınız hata aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="55bf9-135">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="55bf9-136">Veritabanını oluşturmak gereken ve EF çekirdek kullanacağınız [geçişler](xref:data/ef-mvc/migrations) Bunu yapmak için özellik.</span><span class="sxs-lookup"><span data-stu-id="55bf9-136">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="55bf9-137">Geçişler veri modelinizi eşleşen bir veritabanı oluşturun ve verilerinizi değişiklikleri model veritabanı şemasını güncelleştirmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="55bf9-137">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="55bf9-138">EF araç ekleyin ve başlangıç geçiş gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="55bf9-138">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="55bf9-139">Bu bölümde için Paket Yöneticisi Konsolu (PMC) kullanmanız:</span><span class="sxs-lookup"><span data-stu-id="55bf9-139">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="55bf9-140">Entity Framework Çekirdek araçları paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="55bf9-140">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="55bf9-141">Bu paket, geçişler eklemek ve veritabanını güncelleştirmek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="55bf9-141">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="55bf9-142">İlk geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="55bf9-142">Add an initial migration.</span></span>
* <span data-ttu-id="55bf9-143">Veritabanı ile ilk geçiş güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="55bf9-143">Update the database with the initial migration.</span></span>

<span data-ttu-id="55bf9-144">Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="55bf9-144">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<span data-ttu-id="55bf9-145"><!-- following image shared with uid: tutorials/razor-pages/model --> ![PMC menüsü](adding-model/_static/pmc.png)</span><span class="sxs-lookup"><span data-stu-id="55bf9-145"><!-- following image shared with uid: tutorials/razor-pages/model --> ![PMC menu](adding-model/_static/pmc.png)</span></span>

<span data-ttu-id="55bf9-146">PMC aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="55bf9-146">In the PMC, enter the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"
``` PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="55bf9-147">Aşağıdaki hata iletisini yoksaymak, biz sonraki öğreticide düzeltin:</span><span class="sxs-lookup"><span data-stu-id="55bf9-147">Ignore the following error message, we fix it in the next tutorial:</span></span>

<span data-ttu-id="55bf9-148">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span><span class="sxs-lookup"><span data-stu-id="55bf9-148">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span></span>  
      <span data-ttu-id="55bf9-149">*Varlık türü 'Film' ondalık 'Fiyat' sütunu için hiç türü belirtildi. Bu, bunlar varsayılan duyarlık ve ölçek uymuyorsa sessizce kesilecek değerleri neden olur. Açıkça 'ForHasColumnType()' kullanarak tüm değerleri uyum SQL server sütun türü belirtin.*</span><span class="sxs-lookup"><span data-stu-id="55bf9-149">*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="55bf9-150">**Not:** ile bir hata alırsanız, `Install-Package` komutu, NuGet Paket Yöneticisi'ni açın ve arama `Microsoft.EntityFrameworkCore.Tools` paket.</span><span class="sxs-lookup"><span data-stu-id="55bf9-150">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="55bf9-151">Bu, paket yüklemek veya zaten yüklü olup olmadığını denetlemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="55bf9-151">This allows you to install the package or check if it's already installed.</span></span> <span data-ttu-id="55bf9-152">Alternatif olarak, bkz: [CLI yaklaşım](#cli) PMC sorununuz varsa.</span><span class="sxs-lookup"><span data-stu-id="55bf9-152">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

::: moniker-end

<span data-ttu-id="55bf9-153">`Add-Migration` Komutu ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="55bf9-153">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="55bf9-154">Belirtilen model şeması dayalı `DbContext`(içinde *Data/MvcMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="55bf9-154">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="55bf9-155">`Initial` Bağımsız değişkeni geçişler adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="55bf9-155">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="55bf9-156">Herhangi bir ad kullanabilirsiniz, ancak kurala göre geçiş açıklayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="55bf9-156">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="55bf9-157">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="55bf9-157">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="55bf9-158">`Update-Database` Komutu çalıştırır `Up` yönteminde *geçişleri /\<zaman damgası > _Initial.cs* dosyası bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="55bf9-158">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="55bf9-159">Komut satırı arabirimi (CLI) kullanarak önceki adımları yerine PMC gerçekleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="55bf9-159">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="55bf9-160">Ekleme [EF çekirdek araç](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) için *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="55bf9-160">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="55bf9-161">(Proje dizininde) konsolundan aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="55bf9-161">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  <span data-ttu-id="55bf9-162">Uygulamayı çalıştırın ve alma hatası ise:</span><span class="sxs-lookup"><span data-stu-id="55bf9-162">If you run the app and get the error:</span></span>

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

<span data-ttu-id="55bf9-163">Büyük olasılıkla değil çalıştırdığınız `dotnet ef database update`.</span><span class="sxs-lookup"><span data-stu-id="55bf9-163">You probably have not run `dotnet ef database update`.</span></span>

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]
::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Kimliği, fiyat, yayın tarihi ve başlık için kullanılabilir özellikleri listeleyen bir Model öğesi bağlam menüsünde IntelliSense](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="55bf9-165">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="55bf9-165">Additional resources</span></span>

* [<span data-ttu-id="55bf9-166">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="55bf9-166">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="55bf9-167">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="55bf9-167">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="55bf9-168">[Önceki bir görünümü ekleme](adding-view.md)
> [sonraki SQL ile çalışma](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="55bf9-168">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
