---
title: Bir ASP.NET Core MVC uygulaması için bir model ekleme
author: rick-anderson
description: Bir model için basit bir ASP.NET Core uygulamasını ekleyin.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 5a820789ee3a761025d09aa78f3c42e59fc5fa38
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011384"
---
[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="d3273-103">Sağ *modelleri* klasör > **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="d3273-103">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="d3273-104">Sınıf adı **film** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d3273-104">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="d3273-105">`ID` Alan veritabanı için birincil anahtar tarafından gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d3273-105">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="d3273-106">Herhangi bir hata yoksa doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="d3273-106">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="d3273-107">Artık bir **M**odel içinde **M**VC uygulama.</span><span class="sxs-lookup"><span data-stu-id="d3273-107">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="d3273-108">Bir denetleyici iskele kurma özelliği</span><span class="sxs-lookup"><span data-stu-id="d3273-108">Scaffolding a controller</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d3273-109">İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasör **> Ekle > Yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="d3273-109">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller21.png)

<span data-ttu-id="d3273-111">İçinde **İskele Ekle** iletişim kutusunda, dokunun **MVC denetleyici Entity Framework kullanarak görünümler ile > Ekle**.</span><span class="sxs-lookup"><span data-stu-id="d3273-111">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![İskele iletişim kutusu Ekle](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="d3273-113">İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasör **> Ekle > denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="d3273-113">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller.png)

<span data-ttu-id="d3273-115">Varsa **MVC bağımlılıkları Ekle** iletişim kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="d3273-115">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="d3273-116">[Visual Studio en son sürüme güncelleştirme](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="d3273-116">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="d3273-117">Visual Studio sürümlerini 15.5 önce bu iletişim kutusunu göster.</span><span class="sxs-lookup"><span data-stu-id="d3273-117">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="d3273-118">Güncelleştiremiyorsanız, seçin **Ekle**ve ekleme denetleyicisi adımları tekrar uygulayın.</span><span class="sxs-lookup"><span data-stu-id="d3273-118">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="d3273-119">İçinde **İskele Ekle** iletişim kutusunda, dokunun **MVC denetleyici Entity Framework kullanarak görünümler ile > Ekle**.</span><span class="sxs-lookup"><span data-stu-id="d3273-119">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![İskele iletişim kutusu Ekle](adding-model/_static/add_scaffold2.png)

::: moniker-end

<span data-ttu-id="d3273-121">Tamamlamak **denetleyici Ekle** iletişim:</span><span class="sxs-lookup"><span data-stu-id="d3273-121">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="d3273-122">**Model sınıfı:** *film (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="d3273-122">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="d3273-123">**Veri bağlamı sınıfı:** seçin **+** simgesi ve varsayılan ekleme **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="d3273-123">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Veri bağlamı Ekle](adding-model/_static/dc.png)

* <span data-ttu-id="d3273-125">**Görünümler:** varsayılan her bir seçeneğin işaretli koru</span><span class="sxs-lookup"><span data-stu-id="d3273-125">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="d3273-126">**Denetleyici adı:** varsayılan tutun *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="d3273-126">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="d3273-127">Dokunun **Ekle**</span><span class="sxs-lookup"><span data-stu-id="d3273-127">Tap **Add**</span></span>

![Denetleyici Ekle iletişim kutusu](adding-model/_static/add_controller2.png)

<span data-ttu-id="d3273-129">Visual Studio oluşturur:</span><span class="sxs-lookup"><span data-stu-id="d3273-129">Visual Studio creates:</span></span>

* <span data-ttu-id="d3273-130">Bir Entity Framework Core [veritabanı bağlamı sınıfının](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="d3273-130">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="d3273-131">Denetleyici filmler (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="d3273-131">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="d3273-132">Razor görünümü oluştur, Sil, Ayrıntılar, düzenleme ve dizin sayfaları için dosyaları (<em>görünümler/filmler/&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="d3273-132">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="d3273-133">Veritabanı bağlamı otomatik olarak oluşturulmasını ve [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem metotları ve görünümleri olarak bilinir *yapı iskelesi*.</span><span class="sxs-lookup"><span data-stu-id="d3273-133">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="d3273-134">Kısa süre içinde bir film veritabanı yönetmenizi sağlayan bir tam işlevli bir web uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d3273-134">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="d3273-135">Uygulamayı çalıştırın ve tıklayarak **Mvc film** bağlantı, size bir hata aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="d3273-135">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="d3273-136">Veritabanını oluşturmak gereken ve EF Core kullanacağınız [geçişler](xref:data/ef-mvc/migrations) Bunu yapmak için özellik.</span><span class="sxs-lookup"><span data-stu-id="d3273-136">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="d3273-137">Geçişler, veri modelinizi eşleşen bir veritabanı oluşturun ve verilerinizi değişiklikleri model veritabanı şeması güncelleştirmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="d3273-137">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="d3273-138">EF araçları Ekle ve ilk geçiş gerçekleştirin</span><span class="sxs-lookup"><span data-stu-id="d3273-138">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="d3273-139">Bu bölümde için Paket Yöneticisi Konsolu (PMC'yi) kullanmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="d3273-139">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="d3273-140">Entity Framework Core araçları paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d3273-140">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="d3273-141">Bu paket, geçişler ekleyin ve veritabanını güncellemek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d3273-141">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="d3273-142">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d3273-142">Add an initial migration.</span></span>
* <span data-ttu-id="d3273-143">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="d3273-143">Update the database with the initial migration.</span></span>

<span data-ttu-id="d3273-144">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="d3273-144">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<span data-ttu-id="d3273-145"><!-- following image shared with uid: tutorials/razor-pages/model --> ![PMC menüsü](adding-model/_static/pmc.png)</span><span class="sxs-lookup"><span data-stu-id="d3273-145"><!-- following image shared with uid: tutorials/razor-pages/model --> ![PMC menu](adding-model/_static/pmc.png)</span></span>

<span data-ttu-id="d3273-146">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="d3273-146">In the PMC, enter the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

``` PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="d3273-147">Aşağıdaki hata iletisini yoksay, sonraki öğreticide sorunu:</span><span class="sxs-lookup"><span data-stu-id="d3273-147">Ignore the following error message, we fix it in the next tutorial:</span></span>

<span data-ttu-id="d3273-148">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span><span class="sxs-lookup"><span data-stu-id="d3273-148">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span></span>  
      <span data-ttu-id="d3273-149">*Varlık türü 'Film' ondalık 'Fiyat' sütunu için hiç türü belirtildi. Bu, bunlar varsayılan kesinlik ve ölçek uygun değilse sessizce kesilebilir değerleri neden olur. Açıkça 'ForHasColumnType()' kullanarak tüm değerleri uyum SQL server sütun türü belirtin.*</span><span class="sxs-lookup"><span data-stu-id="d3273-149">*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="d3273-150">**Not:** ile ilgili bir hata alırsanız `Install-Package` komutu, NuGet Paket Yöneticisi'ni açın ve arama `Microsoft.EntityFrameworkCore.Tools` paket.</span><span class="sxs-lookup"><span data-stu-id="d3273-150">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="d3273-151">Bu, paketi yüklemek veya zaten yüklü olup olmadığını denetlemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="d3273-151">This allows you to install the package or check if it's already installed.</span></span> <span data-ttu-id="d3273-152">Alternatif olarak, [CLI yaklaşım](#cli) PMC'yi sorununuz varsa.</span><span class="sxs-lookup"><span data-stu-id="d3273-152">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

::: moniker-end

<span data-ttu-id="d3273-153">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d3273-153">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="d3273-154">Belirtilen model şeması dayanır `DbContext`(içinde *Data/MvcMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="d3273-154">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="d3273-155">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d3273-155">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="d3273-156">Herhangi bir adı kullanabilirsiniz, ancak bu kurala göre geçiş tanımlayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="d3273-156">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="d3273-157">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="d3273-157">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="d3273-158">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri /\<zaman damgası > _Initial.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d3273-158">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="d3273-159">PMC yerine komut satırı arabirimi (CLI) kullanarak önceki adımları gerçekleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d3273-159">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="d3273-160">Ekleme [EF Core Araçları](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) için *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="d3273-160">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="d3273-161">(Proje dizininde) konsolundan aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d3273-161">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  <span data-ttu-id="d3273-162">Uygulamayı çalıştırma ve hata iletisi varsa:</span><span class="sxs-lookup"><span data-stu-id="d3273-162">If you run the app and get the error:</span></span>

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

<span data-ttu-id="d3273-163">Büyük olasılıkla değil çalıştırdıktan `dotnet ef database update`.</span><span class="sxs-lookup"><span data-stu-id="d3273-163">You probably have not run `dotnet ef database update`.</span></span>

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Kimliği, fiyat, yayın tarihi ve başlık için kullanılabilir özellikleri listeleyen bir Model öğesi üzerinde IntelliSense bağlamsal menü](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="d3273-165">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d3273-165">Additional resources</span></span>

* [<span data-ttu-id="d3273-166">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="d3273-166">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="d3273-167">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="d3273-167">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="d3273-168">[Önceki Görünüm ekleme](adding-view.md)
> [sonraki SQL ile çalışma](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="d3273-168">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
