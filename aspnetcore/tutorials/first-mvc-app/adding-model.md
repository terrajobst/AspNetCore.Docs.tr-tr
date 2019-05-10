---
title: Bir ASP.NET Core MVC uygulaması için bir model ekleme
author: rick-anderson
description: Bir model için basit bir ASP.NET Core uygulamasını ekleyin.
ms.author: riande
ms.date: 02/25/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 0580c2cb7717a89d0d16cd3f431a63db78fa27cb
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900857"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="43085-103">Bir ASP.NET Core MVC uygulaması için bir model ekleme</span><span class="sxs-lookup"><span data-stu-id="43085-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="43085-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="43085-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="43085-105">Bu bölümde, bir veritabanında filmler yönetmek için sınıflar ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43085-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="43085-106">Bu sınıflar olacaktır "**M**odel" parçası **M**VC uygulama.</span><span class="sxs-lookup"><span data-stu-id="43085-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="43085-107">Bu sınıflar ile kullandığınız [Entity Framework Core](/ef/core) bir veritabanıyla çalışmak için (EF Core).</span><span class="sxs-lookup"><span data-stu-id="43085-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="43085-108">EF Core, yazmanız gereken veri erişim kodu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="43085-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="43085-109">Oluşturduğunuz modeli sınıfları POCO sınıfları olarak bilinen (gelen **P**larak **O**ld **C**LR **O**nesnelerin) herhangi bir bağımlılık sahiptirler yoktur çünkü EF Core.</span><span class="sxs-lookup"><span data-stu-id="43085-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="43085-110">Bunlar yalnızca veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="43085-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="43085-111">Bu öğreticide model sınıfları ilk yazma ve EF Core veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="43085-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="43085-112">Burada ele alınmamaktadır alternatif bir yaklaşım, varolan bir veritabanından model sınıfları oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="43085-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="43085-113">Bu yaklaşımı hakkında daha fazla bilgi için bkz. [ASP.NET Core - mevcut veritabanı](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="43085-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

## <a name="add-a-data-model-class"></a><span data-ttu-id="43085-114">Bir veri modeli sınıfı Ekle</span><span class="sxs-lookup"><span data-stu-id="43085-114">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43085-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43085-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43085-116">Sağ *modelleri* klasör > **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="43085-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="43085-117">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="43085-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="43085-118">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="43085-118">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="43085-119">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="43085-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="43085-120">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="43085-120">Scaffold the movie model</span></span>

<span data-ttu-id="43085-121">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="43085-121">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="43085-122">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="43085-122">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43085-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43085-123">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43085-124">İçinde **Çözüm Gezgini**, sağ *denetleyicileri* klasör **> Ekle > Yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="43085-124">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Yukarıdaki adımı görünümü](adding-model/_static/add_controller21.png)

<span data-ttu-id="43085-126">İçinde **İskele Ekle** iletişim kutusunda **MVC denetleyici Entity Framework kullanarak görünümler ile > Ekle**.</span><span class="sxs-lookup"><span data-stu-id="43085-126">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![İskele iletişim kutusu Ekle](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="43085-128">Tamamlamak **denetleyici Ekle** iletişim:</span><span class="sxs-lookup"><span data-stu-id="43085-128">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="43085-129">**Model sınıfı:** *Film (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="43085-129">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="43085-130">**Veri bağlamı sınıfı:** Seçin **+** simgesi ve varsayılan ekleme **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="43085-130">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Veri bağlamı Ekle](adding-model/_static/dc.png)

* <span data-ttu-id="43085-132">**Görünümler:** Varsayılan her bir seçeneğin işaretli bırakın</span><span class="sxs-lookup"><span data-stu-id="43085-132">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="43085-133">**Denetleyici adı:** Varsayılan tutun *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="43085-133">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="43085-134">Seçin **Ekle**</span><span class="sxs-lookup"><span data-stu-id="43085-134">Select **Add**</span></span>

![Denetleyici Ekle iletişim kutusu](adding-model/_static/add_controller2.png)

<span data-ttu-id="43085-136">Visual Studio oluşturur:</span><span class="sxs-lookup"><span data-stu-id="43085-136">Visual Studio creates:</span></span>

* <span data-ttu-id="43085-137">Bir Entity Framework Core [veritabanı bağlamı sınıfının](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="43085-137">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="43085-138">Denetleyici filmler (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="43085-138">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="43085-139">Razor görünümü oluştur, Sil, Ayrıntılar, düzenleme ve dizin sayfaları için dosyaları (*görünümler/filmler/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="43085-139">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="43085-140">Veritabanı bağlamı otomatik olarak oluşturulmasını ve [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem metotları ve görünümleri olarak bilinir *yapı iskelesi*.</span><span class="sxs-lookup"><span data-stu-id="43085-140">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43085-141">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43085-141">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="43085-142">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="43085-142">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="43085-143">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="43085-143">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="43085-144">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="43085-144">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="43085-145">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43085-145">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="43085-146">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="43085-146">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="43085-147">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="43085-147">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="43085-148">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="43085-148">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="43085-149">Uygulamayı çalıştırın ve tıklayarak **Mvc film** bağlantı, size bir hata aşağıdakine benzer:</span><span class="sxs-lookup"><span data-stu-id="43085-149">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

<span data-ttu-id="43085-150">Veritabanını oluşturmak gereken ve EF Core kullandığınız [geçişler](xref:data/ef-mvc/migrations) Bunu yapmak için özellik.</span><span class="sxs-lookup"><span data-stu-id="43085-150">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="43085-151">Geçişler, veri modelinizi eşleşen bir veritabanı oluşturun ve verilerinizi değişiklikleri model veritabanı şeması güncelleştirmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="43085-151">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="43085-152">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="43085-152">Initial migration</span></span>

<span data-ttu-id="43085-153">Bu bölümde, aşağıdaki görevler tamamlanır:</span><span class="sxs-lookup"><span data-stu-id="43085-153">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="43085-154">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43085-154">Add an initial migration.</span></span>
* <span data-ttu-id="43085-155">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="43085-155">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43085-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43085-156">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="43085-157">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu** (PMC).</span><span class="sxs-lookup"><span data-stu-id="43085-157">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![PMC menüsü](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="43085-159">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="43085-159">In the PMC, enter the following commands:</span></span>

   ```console
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="43085-160">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="43085-160">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="43085-161">Belirtilen model veritabanı şeması dayanır `MvcMovieContext` sınıfı (içinde *Data/MvcMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="43085-161">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="43085-162">`Initial` Geçiş adı olmayan bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="43085-162">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="43085-163">Herhangi bir ad kullanılabilir, ancak kural olarak, geçiş tanımlayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="43085-163">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="43085-164">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="43085-164">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="43085-165">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="43085-165">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="43085-166">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="43085-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="43085-167">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="43085-167">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="43085-168">Belirtilen model veritabanı şeması dayanır `MvcMovieContext` sınıfı (içinde *Data/MvcMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="43085-168">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="43085-169">`InitialCreate` Geçiş adı olmayan bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="43085-169">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="43085-170">Herhangi bir ad kullanılabilir, ancak kural olarak, geçiş tanımlayan bir ad seçilir.</span><span class="sxs-lookup"><span data-stu-id="43085-170">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="43085-171">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="43085-171">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="43085-172">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="43085-172">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="43085-173">Hizmetler (örneğin, EF Core DB bağlamı) DI, uygulama başlatma sırasında kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="43085-173">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="43085-174">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="43085-174">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="43085-175">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="43085-175">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43085-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43085-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="43085-177">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve DI kapsayıcı ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="43085-177">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="43085-178">Aşağıdaki inceleyin `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="43085-178">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="43085-179">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="43085-179">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="43085-180">`MvcMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="43085-180">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="43085-181">Veri bağlamı (`MvcMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="43085-181">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="43085-182">Hangi varlıkları veri modelinde yer alan veri bağlamını belirtir:</span><span class="sxs-lookup"><span data-stu-id="43085-182">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="43085-183">Yukarıdaki kod oluşturur bir [olan DB\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="43085-183">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="43085-184">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="43085-184">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="43085-185">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="43085-185">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="43085-186">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="43085-186">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="43085-187">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="43085-187">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="43085-188">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="43085-188">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="43085-189">Bir DB bağlamı oluşturduğunuz ve DI kapsayıcı ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="43085-189">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="43085-190">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="43085-190">Test the app</span></span>

* <span data-ttu-id="43085-191">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="43085-191">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="43085-192">Bir veritabanı özel aşağıdakine benzer alırsanız:</span><span class="sxs-lookup"><span data-stu-id="43085-192">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="43085-193">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="43085-193">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="43085-194">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="43085-194">Test the **Create** link.</span></span> <span data-ttu-id="43085-195">Veri göndermek ve girin.</span><span class="sxs-lookup"><span data-stu-id="43085-195">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="43085-196">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="43085-196">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="43085-197">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="43085-197">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="43085-198">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="43085-198">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="43085-199">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="43085-199">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="43085-200">İnceleme `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="43085-200">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="43085-201">Önceki vurgulanmış kodu eklenmesini film veritabanı bağlamı gösterir [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı:</span><span class="sxs-lookup"><span data-stu-id="43085-201">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="43085-202">`services.AddDbContext<MvcMovieContext>(options =>` Veritabanı ve bağlantı dizesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="43085-202">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="43085-203">`=>` olan bir [lambda işleci](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="43085-203">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="43085-204">Açık *Controllers/MoviesController.cs* dosya ve oluşturucu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="43085-204">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="43085-205">Oluşturucu kullanan [bağımlılık ekleme](xref:fundamentals/dependency-injection) veritabanı bağlamı eklemesine (`MvcMovieContext`) içine denetleyici.</span><span class="sxs-lookup"><span data-stu-id="43085-205">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="43085-206">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="43085-206">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="43085-207">Kesin olarak modelleri ve @model anahtar sözcüğü</span><span class="sxs-lookup"><span data-stu-id="43085-207">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="43085-208">Bu öğreticide daha önce nasıl bir denetleyici veri veya nesneleri kullanarak bir görünüm geçirebilirsiniz gördüğünüz `ViewData` sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="43085-208">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="43085-209">`ViewData` Sözlüktür bilgi bir görünüme iletmek için kullanışlı bir geç bağlanan yol sağlayan bir dinamik Nesne.</span><span class="sxs-lookup"><span data-stu-id="43085-209">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="43085-210">MVC, model nesneleri bir görünüme kesin geçirme özelliği yazılan de sağlar.</span><span class="sxs-lookup"><span data-stu-id="43085-210">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="43085-211">Bu türü kesin belirlenmiş bir yaklaşım daha iyi derleme zamanında kodunuzu denetimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="43085-211">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="43085-212">(Bu, türü kesin belirlenmiş bir model geçirdiğini) Bu yaklaşımı kullanılan yapı iskelesi mekanizması `MoviesController` sınıfı ve metotları ve görünümleri oluştururken görünümleri.</span><span class="sxs-lookup"><span data-stu-id="43085-212">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="43085-213">Oluşturulan inceleyin `Details` yönteminde *Controllers/MoviesController.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="43085-213">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="43085-214">`id` Parametre rota verileri genel olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="43085-214">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="43085-215">Örneğin `https://localhost:5001/movies/details/1` ayarlar:</span><span class="sxs-lookup"><span data-stu-id="43085-215">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="43085-216">Denetleyiciye `movies` denetleyici (ilk URL kesimini).</span><span class="sxs-lookup"><span data-stu-id="43085-216">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="43085-217">Eyleme `details` (ikinci URL kesimini).</span><span class="sxs-lookup"><span data-stu-id="43085-217">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="43085-218">1 (son URL kesimini) kimliği.</span><span class="sxs-lookup"><span data-stu-id="43085-218">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="43085-219">Ayrıca, geçirebilirsiniz `id` ile bir sorgu dizesi şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="43085-219">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="43085-220">`id` Parametresi olarak tanımlanmış olan bir [boş değer atanabilir tür](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) durumda bir kimlik değeri sağlanmadı.</span><span class="sxs-lookup"><span data-stu-id="43085-220">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="43085-221">A [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) için geçirilen `FirstOrDefaultAsync` rota verileri veya sorgu dizesi değeriyle eşleşen film varlıkları seçin.</span><span class="sxs-lookup"><span data-stu-id="43085-221">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="43085-222">Bir filmi bulunursa örneği `Movie` modeline geçirilir `Details` görüntüle:</span><span class="sxs-lookup"><span data-stu-id="43085-222">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="43085-223">İçeriğini incelemek *Views/Movies/Details.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="43085-223">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="43085-224">Ekleyerek bir `@model` deyimi görünüm dosyası üst kısmındaki görünümü bekliyor nesne türünü belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="43085-224">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="43085-225">Aşağıdaki film denetleyicisi oluşturduğunuzda `@model` deyimi otomatik olarak dahil edilen en üstündeki *Details.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="43085-225">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="43085-226">Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen film erişmenize olanak sağlayan bir `Model` türü kesin belirlenmiş bir nesne.</span><span class="sxs-lookup"><span data-stu-id="43085-226">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="43085-227">Örneğin, *Details.cshtml* görünümü, kod, her filmin alanına geçirir `DisplayNameFor` ve `DisplayFor` HTML Yardımcıları ile kesin olarak belirlenmiş `Model` nesne.</span><span class="sxs-lookup"><span data-stu-id="43085-227">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="43085-228">`Create` Ve `Edit` metotları ve görünümleri de başarılı bir `Movie` model nesnesi.</span><span class="sxs-lookup"><span data-stu-id="43085-228">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="43085-229">İnceleme *Index.cshtml* görünümü ve `Index` denetleyici filmler yöntemi.</span><span class="sxs-lookup"><span data-stu-id="43085-229">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="43085-230">Kodun nasıl oluşturduğunu fark bir `List` nesne çağırdığında `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="43085-230">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="43085-231">Bu kod geçirir `Movies` gelen listesinde `Index` eylem yöntemine görünümü:</span><span class="sxs-lookup"><span data-stu-id="43085-231">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="43085-232">Denetleyici filmler oluşturduğunuzda otomatik olarak iskele kurma özelliği aşağıdaki dahil `@model` en üstündeki deyimi *Index.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="43085-232">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="43085-233">`@model` Yönergesi kullanarak görünüm tarafından geçirilen denetleyici filmler listesini erişmenize olanak sağlayan bir `Model` türü kesin belirlenmiş bir nesne.</span><span class="sxs-lookup"><span data-stu-id="43085-233">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="43085-234">Örneğin, *Index.cshtml* görüntülemek, filmlerle kodunu döner bir `foreach` deyimi kesin olarak belirlenmiş üzerinden `Model` nesnesi:</span><span class="sxs-lookup"><span data-stu-id="43085-234">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="43085-235">Çünkü `Model` nesne türü kesin belirlenmiş (olarak bir `IEnumerable<Movie>` nesne), Döngüdeki her bir öğe olarak yazılan `Movie`.</span><span class="sxs-lookup"><span data-stu-id="43085-235">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="43085-236">Diğer avantajlar arasında bu derleme zamanında kodu denetimi Al anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="43085-236">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43085-237">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="43085-237">Additional resources</span></span>

* [<span data-ttu-id="43085-238">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="43085-238">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="43085-239">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="43085-239">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="43085-240">[Önceki Görünüm ekleme](adding-view.md)
> [sonraki SQL ile çalışma](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="43085-240">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>
