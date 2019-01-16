---
title: Bir ASP.NET Core MVC uygulaması için bir model ekleme
author: rick-anderson
description: Bir model için basit bir ASP.NET Core uygulamasını ekleyin.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 062a248ffdf8e30ed01a72e0a555c1c9a1ab1b6d
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341618"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="6624a-103">Bir ASP.NET Core MVC uygulaması için bir model ekleme</span><span class="sxs-lookup"><span data-stu-id="6624a-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="6624a-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6624a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="6624a-105">Bu bölümde, bir veritabanında filmler yönetmek için sınıflar ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6624a-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="6624a-106">Bu sınıflar olacaktır "**M**odel" parçası **M**VC uygulama.</span><span class="sxs-lookup"><span data-stu-id="6624a-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="6624a-107">Bu sınıflar ile kullandığınız [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core).</span><span class="sxs-lookup"><span data-stu-id="6624a-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="6624a-108">EF Core, yazmanız gereken veri erişim kodu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="6624a-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="6624a-109">Oluşturduğunuz modeli sınıfları POCO sınıfları olarak bilinen (gelen **P**larak **O**ld **C**LR **O**nesnelerin) herhangi bir bağımlılık sahiptirler yoktur çünkü EF Core.</span><span class="sxs-lookup"><span data-stu-id="6624a-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="6624a-110">Bunlar yalnızca veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="6624a-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="6624a-111">Bu öğreticide model sınıfları ilk yazma ve EF Core veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6624a-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="6624a-112">Burada ele alınmamaktadır alternatif bir yaklaşım, varolan bir veritabanından model sınıfları oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="6624a-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="6624a-113">Bu yaklaşımı hakkında daha fazla bilgi için bkz. [ASP.NET Core - mevcut veritabanı](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="6624a-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

## <a name="add-a-data-model-class"></a><span data-ttu-id="6624a-114">Bir veri modeli sınıfı Ekle</span><span class="sxs-lookup"><span data-stu-id="6624a-114">Add a data model class</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6624a-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6624a-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6624a-116">Sağ *modelleri* klasör > **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="6624a-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="6624a-117">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="6624a-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6624a-118">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="6624a-118">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="6624a-119">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="6624a-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---  
<!-- End of VS tabs -->

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="6624a-120">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="6624a-120">Scaffold the movie model</span></span>

<span data-ttu-id="6624a-121">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="6624a-121">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="6624a-122">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6624a-122">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6624a-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6624a-123">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6624a-124">İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasör **> Ekle > Yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="6624a-124">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller21.png)

<span data-ttu-id="6624a-126">İçinde **İskele Ekle** iletişim kutusunda **MVC denetleyici Entity Framework kullanarak görünümler ile > Ekle**.</span><span class="sxs-lookup"><span data-stu-id="6624a-126">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![İskele iletişim kutusu Ekle](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="6624a-128">Tamamlamak **denetleyici Ekle** iletişim:</span><span class="sxs-lookup"><span data-stu-id="6624a-128">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="6624a-129">**Model sınıfı:** *Film (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="6624a-129">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="6624a-130">**Veri bağlamı sınıfı:** Seçin **+** simgesi ve varsayılan ekleme **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="6624a-130">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Veri bağlamı Ekle](adding-model/_static/dc.png)

* <span data-ttu-id="6624a-132">**Görünümler:** Varsayılan her bir seçeneğin işaretli bırakın</span><span class="sxs-lookup"><span data-stu-id="6624a-132">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="6624a-133">**Denetleyici adı:** Varsayılan tutun *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="6624a-133">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="6624a-134">Seçin **Ekle**</span><span class="sxs-lookup"><span data-stu-id="6624a-134">Select **Add**</span></span>

![Denetleyici Ekle iletişim kutusu](adding-model/_static/add_controller2.png)

<span data-ttu-id="6624a-136">Visual Studio oluşturur:</span><span class="sxs-lookup"><span data-stu-id="6624a-136">Visual Studio creates:</span></span>

* <span data-ttu-id="6624a-137">Bir Entity Framework Core [veritabanı bağlamı sınıfının](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="6624a-137">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="6624a-138">Denetleyici filmler (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="6624a-138">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="6624a-139">Razor görünümü oluştur, Sil, Ayrıntılar, düzenleme ve dizin sayfaları için dosyaları (<em>görünümler/filmler/&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="6624a-139">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="6624a-140">Veritabanı bağlamı otomatik olarak oluşturulmasını ve [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem metotları ve görünümleri olarak bilinir *yapı iskelesi*.</span><span class="sxs-lookup"><span data-stu-id="6624a-140">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6624a-141">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6624a-141">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="6624a-142">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="6624a-142">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6624a-143">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="6624a-143">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="6624a-144">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6624a-144">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6624a-145">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6624a-145">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6624a-146">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="6624a-146">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6624a-147">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="6624a-147">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="6624a-148">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6624a-148">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="6624a-149">Uygulamayı çalıştırın ve tıklayarak **Mvc film** bağlantı, size bir hata aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="6624a-149">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

<span data-ttu-id="6624a-150">Veritabanını oluşturmak gereken ve EF Core kullandığınız [geçişler](xref:data/ef-mvc/migrations) Bunu yapmak için özellik.</span><span class="sxs-lookup"><span data-stu-id="6624a-150">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="6624a-151">Geçişler, veri modelinizi eşleşen bir veritabanı oluşturun ve verilerinizi değişiklikleri model veritabanı şeması güncelleştirmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="6624a-151">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="6624a-152">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="6624a-152">Initial migration</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6624a-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6624a-153">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6624a-154">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="6624a-154">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="6624a-155">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6624a-155">Add an initial migration.</span></span>
* <span data-ttu-id="6624a-156">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="6624a-156">Update the database with the initial migration.</span></span>

<span data-ttu-id="6624a-157">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="6624a-157">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="6624a-159">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="6624a-159">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="6624a-160">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6624a-160">The `Add-Migration` command generates code to create the initial database schema.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6624a-161">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="6624a-161">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]
<span data-ttu-id="6624a-162">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6624a-162">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

---  
<!-- End of VS tabs -->

<span data-ttu-id="6624a-163">Yukarıdaki komutlarda aşağıdaki uyarı oluştur: "Hiçbir türü ondalık sütunu 'Fiyat' varlık türünün 'Film' için belirtildi.</span><span class="sxs-lookup"><span data-stu-id="6624a-163">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="6624a-164">Bu, bunlar varsayılan kesinlik ve ölçek uygun değilse sessizce kesilebilir değerleri neden olur.</span><span class="sxs-lookup"><span data-stu-id="6624a-164">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="6624a-165">"Açıkça 'HasColumnType()' kullanarak tüm değerleri uyum SQL server sütun türü belirtin."</span><span class="sxs-lookup"><span data-stu-id="6624a-165">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="6624a-166">Bu uyarıyı yoksayabilirsiniz, bir sonraki öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="6624a-166">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="6624a-167">Belirtilen model şeması dayanır `DbContext` (içinde *Models/MvcMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="6624a-167">The schema is based on the model specified in the `DbContext` (In the *Models/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="6624a-168">`InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6624a-168">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="6624a-169">Herhangi bir ad kullanılabilir, ancak bir adı seçili kural gereği, geçiş açıklar.</span><span class="sxs-lookup"><span data-stu-id="6624a-169">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="6624a-170">`ef database update` Komutu çalıştırmaları `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="6624a-170">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="6624a-171">`Up` Yöntemi veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6624a-171">The `Up` method creates the database.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6624a-172">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6624a-172">Visual Studio</span></span>](#tab/visual-studio)

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="6624a-173">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="6624a-173">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="6624a-174">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6624a-174">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6624a-175">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="6624a-175">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="6624a-176">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6624a-176">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="6624a-177">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6624a-177">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="6624a-178">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="6624a-178">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="6624a-179">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6624a-179">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6624a-180">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="6624a-180">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="6624a-181">`MvcMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="6624a-181">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="6624a-182">Veri bağlamı (`MvcMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="6624a-182">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="6624a-183">Hangi varlıkları veri modelinde yer alan veri bağlamını belirtir:</span><span class="sxs-lookup"><span data-stu-id="6624a-183">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="6624a-184">Yukarıdaki kod oluşturur bir [ `DbSet<Movie>` ](/dotnet/api/microsoft.entityframeworkcore.dbset-1) varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="6624a-184">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="6624a-185">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6624a-185">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="6624a-186">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6624a-186">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="6624a-187">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="6624a-187">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="6624a-188">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="6624a-188">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6624a-189">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="6624a-189">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="6624a-190">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6624a-190">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6624a-191">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="6624a-191">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="6624a-192">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6624a-192">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="6624a-193">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6624a-193">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="6624a-194">Bir DB bağlamı oluşturduğunuz ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="6624a-194">You created a DB context and registered it with the dependency injection container.</span></span>

---

<span data-ttu-id="6624a-195">Belirtilen model şeması dayanır `MvcMovieContext` (içinde *Data/MvcMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="6624a-195">The schema is based on the model specified in the `MvcMovieContext` (In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="6624a-196">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6624a-196">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="6624a-197">Herhangi bir ad kullanılabilir, ancak kurala göre geçiş tanımlayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6624a-197">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="6624a-198">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="6624a-198">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="6624a-199">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6624a-199">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="6624a-200">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="6624a-200">Test the app</span></span>

* <span data-ttu-id="6624a-201">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="6624a-201">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="6624a-202">Bir veritabanı özel aşağıdakine benzer alırsanız:</span><span class="sxs-lookup"><span data-stu-id="6624a-202">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="6624a-203">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="6624a-203">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="6624a-204">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="6624a-204">Test the **Create** link.</span></span>

  > [!NOTE]
  > <span data-ttu-id="6624a-205">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="6624a-205">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="6624a-206">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="6624a-206">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="6624a-207">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="6624a-207">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="6624a-208">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="6624a-208">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="6624a-209">İnceleme `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="6624a-209">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="6624a-210">Önceki vurgulanmış kodu eklenmesini film veritabanı bağlamı gösterir [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı:</span><span class="sxs-lookup"><span data-stu-id="6624a-210">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="6624a-211">`services.AddDbContext<MvcMovieContext>(options =>` Veritabanı ve bağlantı dizesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="6624a-211">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="6624a-212">`=>` olan bir [lambda işleci](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="6624a-212">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="6624a-213">Açık *Controllers/MoviesController.cs* dosya ve oluşturucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="6624a-213">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="6624a-214">Oluşturucu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) veritabanı bağlamı eklemesine (`MvcMovieContext `) içine denetleyici.</span><span class="sxs-lookup"><span data-stu-id="6624a-214">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext `) into the controller.</span></span> <span data-ttu-id="6624a-215">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="6624a-215">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="6624a-216">Kesin olarak modelleri ve @model anahtar sözcüğü</span><span class="sxs-lookup"><span data-stu-id="6624a-216">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="6624a-217">Bu öğreticide daha önce nasıl bir denetleyici veri veya nesneleri kullanarak bir görünüm geçirebilirsiniz gördüğünüz `ViewData` sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="6624a-217">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="6624a-218">`ViewData` Sözlüktür bilgi bir görünüme iletmek için kullanışlı bir geç bağlanan yol sağlayan bir dinamik Nesne.</span><span class="sxs-lookup"><span data-stu-id="6624a-218">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="6624a-219">MVC, model nesneleri bir görünüme kesin geçirme özelliği yazılan de sağlar.</span><span class="sxs-lookup"><span data-stu-id="6624a-219">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="6624a-220">Bu türü kesin belirlenmiş bir yaklaşım daha iyi derleme zamanında kodunuzu denetimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6624a-220">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="6624a-221">(Bu, türü kesin belirlenmiş bir model geçirdiğini) Bu yaklaşımı kullanılan yapı iskelesi mekanizması `MoviesController` sınıfı ve metotları ve görünümleri oluştururken görünümleri.</span><span class="sxs-lookup"><span data-stu-id="6624a-221">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="6624a-222">Oluşturulan inceleyin `Details` yönteminde *Controllers/MoviesController.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="6624a-222">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="6624a-223">`id` Parametre rota verileri genel olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="6624a-223">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="6624a-224">Örneğin `https://localhost:5001/movies/details/1` ayarlar:</span><span class="sxs-lookup"><span data-stu-id="6624a-224">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="6624a-225">Denetleyiciye `movies` denetleyici (ilk URL kesimini).</span><span class="sxs-lookup"><span data-stu-id="6624a-225">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="6624a-226">Eyleme `details` (ikinci URL kesimini).</span><span class="sxs-lookup"><span data-stu-id="6624a-226">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="6624a-227">1 (son URL kesimini) kimliği.</span><span class="sxs-lookup"><span data-stu-id="6624a-227">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="6624a-228">Ayrıca, geçirebilirsiniz `id` ile bir sorgu dizesi şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="6624a-228">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="6624a-229">`id` Parametresi olarak tanımlanmış olan bir [boş değer atanabilir tür](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) durumda bir kimlik değeri sağlanmadı.</span><span class="sxs-lookup"><span data-stu-id="6624a-229">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="6624a-230">A [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) için geçirilen `FirstOrDefaultAsync` rota verileri veya sorgu dizesi değeriyle eşleşen film varlıkları seçin.</span><span class="sxs-lookup"><span data-stu-id="6624a-230">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="6624a-231">Bir filmi bulunursa örneği `Movie` modeline geçirilir `Details` görüntüle:</span><span class="sxs-lookup"><span data-stu-id="6624a-231">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="6624a-232">İçeriğini incelemek *Views/Movies/Details.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="6624a-232">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="6624a-233">Ekleyerek bir `@model` deyimi görünüm dosyası üst kısmındaki görünümü bekliyor nesne türünü belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6624a-233">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="6624a-234">Aşağıdaki film denetleyicisi oluşturduğunuzda `@model` deyimi otomatik olarak dahil edilen en üstündeki *Details.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="6624a-234">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="6624a-235">Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen film erişmenize olanak sağlayan bir `Model` türü kesin belirlenmiş bir nesne.</span><span class="sxs-lookup"><span data-stu-id="6624a-235">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="6624a-236">Örneğin, *Details.cshtml* görünümü, kod, her filmin alanına geçirir `DisplayNameFor` ve `DisplayFor` HTML Yardımcıları ile kesin olarak belirlenmiş `Model` nesne.</span><span class="sxs-lookup"><span data-stu-id="6624a-236">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="6624a-237">`Create` Ve `Edit` metotları ve görünümleri de başarılı bir `Movie` model nesnesi.</span><span class="sxs-lookup"><span data-stu-id="6624a-237">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="6624a-238">İnceleme *Index.cshtml* görünümü ve `Index` denetleyici filmler yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6624a-238">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="6624a-239">Kodun nasıl oluşturduğunu fark bir `List` nesne çağırdığında `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6624a-239">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="6624a-240">Bu kod geçirir `Movies` gelen listesinde `Index` eylem yöntemine görünümü:</span><span class="sxs-lookup"><span data-stu-id="6624a-240">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="6624a-241">Denetleyici filmler oluşturduğunuzda otomatik olarak iskele kurma özelliği aşağıdaki dahil `@model` en üstündeki deyimi *Index.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="6624a-241">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="6624a-242">`@model` Yönergesi kullanarak görünüm tarafından geçirilen denetleyici filmler listesini erişmenize olanak sağlayan bir `Model` türü kesin belirlenmiş bir nesne.</span><span class="sxs-lookup"><span data-stu-id="6624a-242">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="6624a-243">Örneğin, *Index.cshtml* görüntülemek, filmlerle kodunu döner bir `foreach` deyimi kesin olarak belirlenmiş üzerinden `Model` nesnesi:</span><span class="sxs-lookup"><span data-stu-id="6624a-243">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="6624a-244">Çünkü `Model` nesne türü kesin belirlenmiş (olarak bir `IEnumerable<Movie>` nesne), Döngüdeki her bir öğe olarak yazılan `Movie`.</span><span class="sxs-lookup"><span data-stu-id="6624a-244">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="6624a-245">Diğer avantajlar arasında bu derleme zamanında kodu denetimi Al anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="6624a-245">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6624a-246">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6624a-246">Additional resources</span></span>

* [<span data-ttu-id="6624a-247">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="6624a-247">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="6624a-248">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="6624a-248">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="6624a-249">[Önceki Görünüm ekleme](adding-view.md)
> [sonraki SQL ile çalışma](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="6624a-249">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
