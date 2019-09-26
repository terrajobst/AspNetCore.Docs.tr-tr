---
title: ASP.NET Core MVC uygulamasına model ekleme
author: rick-anderson
description: Basit bir ASP.NET Core uygulamasına model ekleyin.
ms.author: riande
ms.date: 8/15/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 5ad31a2536ad70590eaa767cf20068512241f36b
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71295468"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="34e3d-103">ASP.NET Core MVC uygulamasına model ekleme</span><span class="sxs-lookup"><span data-stu-id="34e3d-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="34e3d-104">[Rick Anderson](https://twitter.com/RickAndMSFT) ve [Tom Dykstra](https://github.com/tdykstra) tarafından</span><span class="sxs-lookup"><span data-stu-id="34e3d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="34e3d-105">Bu bölümde, bir veritabanında film yönetmeye yönelik sınıflar eklersiniz.</span><span class="sxs-lookup"><span data-stu-id="34e3d-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="34e3d-106">Bu sınıflar, **d**VC uygulamasının "**d**odel" parçası olacaktır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="34e3d-107">Bu sınıfları bir veritabanıyla çalışmak için [Entity Framework Core](/ef/core) (EF Core) ile birlikte kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="34e3d-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="34e3d-108">EF Core, yazmanız gereken veri erişim kodunu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="34e3d-109">Oluşturduğunuz model sınıfları, EF Core hiçbir bağımlılığı olmadığından, POCO sınıfları olarak bilinir ( **P**Lain **O**).</span><span class="sxs-lookup"><span data-stu-id="34e3d-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="34e3d-110">Yalnızca veritabanında depolanacak verilerin özelliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="34e3d-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="34e3d-111">Bu öğreticide, önce model sınıflarını yazdığınızda EF Core veritabanını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="34e3d-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="34e3d-112">Burada kapsanmayan alternatif bir yaklaşım var olan bir veritabanından model sınıfları oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="34e3d-113">Bu yaklaşım hakkında daha fazla bilgi için bkz. [ASP.NET Core-var olan veritabanı](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="34e3d-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="34e3d-114">Veri modeli sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="34e3d-114">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e3d-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="34e3d-116">**Sınıf**Ekle > > *modeller* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="34e3d-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="34e3d-117">Dosyayı *Movie.cs*olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="34e3d-117">Name the file *Movie.cs*.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="34e3d-118">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-118">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="34e3d-119">*Modeller* klasörüne *Movie.cs* adlı bir dosya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-119">Add a file named *Movie.cs* to the *Models* folder.</span></span>

---

<span data-ttu-id="34e3d-120">*Movie.cs* dosyasını aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-120">Update the *Movie.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/Movie.cs)]

<span data-ttu-id="34e3d-121">Sınıfı, birincil anahtar `Id` için veritabanı için gerekli olan bir alanı içerir. `Movie`</span><span class="sxs-lookup"><span data-stu-id="34e3d-121">The `Movie` class contains an `Id` field, which is required by the database for the primary key.</span></span>

<span data-ttu-id="34e3d-122">`Date`Üzerindeki [](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) `ReleaseDate` DataType özniteliği, veri türünü belirtir ().</span><span class="sxs-lookup"><span data-stu-id="34e3d-122">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute on `ReleaseDate` specifies the type of the data (`Date`).</span></span> <span data-ttu-id="34e3d-123">Bu öznitelikle:</span><span class="sxs-lookup"><span data-stu-id="34e3d-123">With this attribute:</span></span>

  * <span data-ttu-id="34e3d-124">Kullanıcının Tarih alanına saat bilgilerini girmesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-124">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="34e3d-125">Zaman bilgisi değil yalnızca tarih görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-125">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="34e3d-126">[Veri açıklamaları](/dotnet/api/system.componentmodel.dataannotations) sonraki bir öğreticide ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-126">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="34e3d-127">NuGet paketleri Ekle</span><span class="sxs-lookup"><span data-stu-id="34e3d-127">Add NuGet packages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e3d-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-128">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="34e3d-129">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** (PMC) öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-129">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

![PMC menüsü](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="34e3d-131">PMC 'de şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="34e3d-131">In the PMC, run the following command:</span></span>

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="34e3d-132">Yukarıdaki komut, EF Core SQL Server sağlayıcısını ekler.</span><span class="sxs-lookup"><span data-stu-id="34e3d-132">The preceding command adds the EF Core SQL Server provider.</span></span> <span data-ttu-id="34e3d-133">Sağlayıcı paketi, EF Core paketini bir bağımlılık olarak yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="34e3d-133">The provider package installs the EF Core package as a dependency.</span></span> <span data-ttu-id="34e3d-134">Ek paketler, öğreticinin sonraki bölümlerinde bulunan yapı iskelesi adımında otomatik olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-134">Additional packages are installed automatically in the scaffolding step later in the tutorial.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="34e3d-135">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-135">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

---

<a name="dc"></a>

## <a name="create-a-database-context-class"></a><span data-ttu-id="34e3d-136">Veritabanı bağlamı sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="34e3d-136">Create a database context class</span></span>

<span data-ttu-id="34e3d-137">`Movie` Model için EF Core işlevselliği (oluşturma, okuma, güncelleştirme, silme) koordine etmek için bir veritabanı bağlamı sınıfı gerekir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-137">A database context class is needed to coordinate EF Core functionality (Create, Read, Update, Delete) for the `Movie` model.</span></span> <span data-ttu-id="34e3d-138">Veritabanı bağlamı [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) öğesinden türetilir ve veri modeline dahil edilecek varlıkları belirtir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-138">The database context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and specifies the entities to include in the data model.</span></span>

<span data-ttu-id="34e3d-139">Bir *veri* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="34e3d-139">Create a *Data* folder.</span></span>

<span data-ttu-id="34e3d-140">Aşağıdaki kodla bir *Data/MvcMovieContext. cs* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-140">Add a *Data/MvcMovieContext.cs* file with the following code:</span></span> 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="34e3d-141">Önceki kod, varlık kümesi için bir [dbset\<filmi >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="34e3d-141">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="34e3d-142">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-142">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="34e3d-143">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-143">An entity corresponds to a row in the table.</span></span>

<a name="reg"></a>

## <a name="register-the-database-context"></a><span data-ttu-id="34e3d-144">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="34e3d-144">Register the database context</span></span>

<span data-ttu-id="34e3d-145">ASP.NET Core, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="34e3d-145">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="34e3d-146">Hizmetlerin (EF Core DB bağlamı gibi) uygulama başlatma sırasında DI ile kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-146">Services (such as the EF Core DB context) must be registered with DI during application startup.</span></span> <span data-ttu-id="34e3d-147">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-147">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="34e3d-148">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-148">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span> <span data-ttu-id="34e3d-149">Bu bölümde, veritabanı bağlamını dı kapsayıcısına kaydedersiniz.</span><span class="sxs-lookup"><span data-stu-id="34e3d-149">In this section, you register the database context with the DI container.</span></span>

<span data-ttu-id="34e3d-150">Aşağıdaki `using` deyimlerini *Startup.cs*üst kısmına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-150">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="34e3d-151">Aşağıdaki Vurgulanan kodu içine `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-151">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e3d-152">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-152">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="34e3d-153">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-153">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

---

<span data-ttu-id="34e3d-154">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="34e3d-154">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="34e3d-155">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="34e3d-155">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="cs"></a>

## <a name="add-a-database-connection-string"></a><span data-ttu-id="34e3d-156">Veritabanı bağlantı dizesi Ekle</span><span class="sxs-lookup"><span data-stu-id="34e3d-156">Add a database connection string</span></span>

<span data-ttu-id="34e3d-157">*AppSettings. JSON* dosyasına bir bağlantı dizesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-157">Add a connection string to the *appsettings.json* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e3d-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-158">Visual Studio</span></span>](#tab/visual-studio)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="34e3d-159">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-159">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

---

<span data-ttu-id="34e3d-160">Projeyi derleyici hatalarına yönelik bir denetim olarak derleyin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-160">Build the project as a check for compiler errors.</span></span>

## <a name="scaffold-movie-pages"></a><span data-ttu-id="34e3d-161">Yapı iskelesi film sayfaları</span><span class="sxs-lookup"><span data-stu-id="34e3d-161">Scaffold movie pages</span></span>

<span data-ttu-id="34e3d-162">Film modeli için oluşturma, okuma, güncelleştirme ve silme (CRUD) sayfaları üretmek için scafkatlama aracını kullanın.</span><span class="sxs-lookup"><span data-stu-id="34e3d-162">Use the scaffolding tool to produce Create, Read, Update, and Delete (CRUD) pages for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e3d-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-163">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="34e3d-164">**Çözüm Gezgini**, *denetleyiciler* klasörüne sağ tıklayıp **yeni > yapı iskelesi > öğesi ekleyin**.</span><span class="sxs-lookup"><span data-stu-id="34e3d-164">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Yukarıdaki adımın görünümü](adding-model/_static/add_controller21.png)

<span data-ttu-id="34e3d-166">**Yapı Ekle** iletişim kutusunda, **Entity Framework > Ekle ' yi kullanarak views ile MVC denetleyicisi '** ni seçin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-166">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Yapı Iskelesi Ekle iletişim kutusu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="34e3d-168">**Denetleyici Ekle** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="34e3d-168">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="34e3d-169">**Model Sınıfı:** *Film (MvcMovie. modeller)*</span><span class="sxs-lookup"><span data-stu-id="34e3d-169">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="34e3d-170">**Veri bağlamı sınıfı:** *MvcMovieContext (MvcMovie. Data)*</span><span class="sxs-lookup"><span data-stu-id="34e3d-170">**Data context class:** *MvcMovieContext (MvcMovie.Data)*</span></span>

![Veri bağlamı Ekle](adding-model/_static/dc3.png)

* <span data-ttu-id="34e3d-172">**Görünümler:** Her seçeneğin varsayılan kısmını işaretli tut</span><span class="sxs-lookup"><span data-stu-id="34e3d-172">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="34e3d-173">**Denetleyici adı:** Varsayılan *MoviesController* tut</span><span class="sxs-lookup"><span data-stu-id="34e3d-173">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="34e3d-174">**Ekle** 'yi seçin</span><span class="sxs-lookup"><span data-stu-id="34e3d-174">Select **Add**</span></span>

<span data-ttu-id="34e3d-175">Visual Studio şunları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="34e3d-175">Visual Studio creates:</span></span>

* <span data-ttu-id="34e3d-176">Bir filmler denetleyicisi (*denetleyiciler/MoviesController. cs*)</span><span class="sxs-lookup"><span data-stu-id="34e3d-176">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="34e3d-177">Razor oluşturma, silme, ayrıntılar, düzenleme ve dizin sayfaları için dosyaları görüntüleme (*Görünümler/filmler/\*. cshtml*)</span><span class="sxs-lookup"><span data-stu-id="34e3d-177">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="34e3d-178">Bu dosyaların otomatik olarak oluşturulması, *Yapı iskelesi*olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-178">The automatic creation of these files is known as *scaffolding*.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="34e3d-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="34e3d-179">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="34e3d-180">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="34e3d-180">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="34e3d-181">Linux 'ta, scafkatlama aracı yolunu dışarı aktarın:</span><span class="sxs-lookup"><span data-stu-id="34e3d-181">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="34e3d-182">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="34e3d-182">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="34e3d-183">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-183">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="34e3d-184">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="34e3d-184">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="34e3d-185">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="34e3d-185">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of tabs                  -->

<span data-ttu-id="34e3d-186">Veritabanı mevcut olmadığından, scafkatmış sayfaları henüz kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="34e3d-186">You can't use the scaffolded pages yet because the database doesn't exist.</span></span> <span data-ttu-id="34e3d-187">Uygulamayı çalıştırır ve **film uygulaması** bağlantısına tıklarsanız, *veritabanı* açılamıyor veya *böyle bir tablo yok: Film* hata iletisi.</span><span class="sxs-lookup"><span data-stu-id="34e3d-187">If you run the app and click on the **Movie App** link, you get a *Cannot open database* or *no such table: Movie* error message.</span></span>

<a name="migration"></a>

## <a name="initial-migration"></a><span data-ttu-id="34e3d-188">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="34e3d-188">Initial migration</span></span>

<span data-ttu-id="34e3d-189">Veritabanını oluşturmak için EF Core [geçişleri](xref:data/ef-mvc/migrations) özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="34e3d-189">Use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to create the database.</span></span> <span data-ttu-id="34e3d-190">Geçişler, veri modelinizle eşleşecek bir veritabanı oluşturmanıza ve güncelleştirmenize olanak sağlayan bir araç kümesidir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-190">Migrations is a set of tools that let you create and update a database to match your data model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e3d-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-191">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="34e3d-192">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** (PMC) öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-192">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

<span data-ttu-id="34e3d-193">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-193">In the PMC, enter the following commands:</span></span>

```console
Add-Migration InitialCreate
Update-Database
```

* <span data-ttu-id="34e3d-194">`Add-Migration InitialCreate`: Bir *geçişler/{timestamp} _ınitialcreate. cs* geçiş dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="34e3d-194">`Add-Migration InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="34e3d-195">`InitialCreate` Bağımsız değişkeni geçiş adıdır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-195">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="34e3d-196">Herhangi bir ad kullanılabilir, ancak kurala göre, geçişi açıklayan bir ad seçilidir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-196">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="34e3d-197">Bu ilk geçiş olduğundan, oluşturulan sınıf veritabanı şemasını oluşturmak için kod içerir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-197">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="34e3d-198">Veritabanı şeması, `MvcMovieContext` sınıfında belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-198">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span>

* <span data-ttu-id="34e3d-199">`Update-Database`: Veritabanını, önceki komutun oluşturulduğu en son geçişe güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-199">`Update-Database`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="34e3d-200">Bu komut, veritabanını `Up` oluşturan *geçişler/{Time-damga} _ınitialcreate. cs* dosyasındaki yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-200">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

  <span data-ttu-id="34e3d-201">Database Update komutu aşağıdaki uyarıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="34e3d-201">The database update command generates the following warning:</span></span> 

  > <span data-ttu-id="34e3d-202">' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="34e3d-202">No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="34e3d-203">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="34e3d-203">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="34e3d-204">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açıkça belirtin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-204">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.</span></span>

  <span data-ttu-id="34e3d-205">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-205">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

[!INCLUDE [more information on the PMC tools for EF Core](~/includes/ef-pmc.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="34e3d-206">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-206">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="34e3d-207">Aşağıdaki .NET Core CLI komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="34e3d-207">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* <span data-ttu-id="34e3d-208">`ef migrations add InitialCreate`: Bir *geçişler/{timestamp} _ınitialcreate. cs* geçiş dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="34e3d-208">`ef migrations add InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="34e3d-209">`InitialCreate` Bağımsız değişkeni geçiş adıdır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-209">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="34e3d-210">Herhangi bir ad kullanılabilir, ancak kurala göre, geçişi açıklayan bir ad seçilidir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-210">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="34e3d-211">Bu ilk geçiş olduğundan, oluşturulan sınıf veritabanı şemasını oluşturmak için kod içerir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-211">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="34e3d-212">Veritabanı şeması `MvcMovieContext` sınıfında belirtilen modeli temel alır ( *Data/mvcmoviecontext. cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="34e3d-212">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span>

* <span data-ttu-id="34e3d-213">`ef database update`: Veritabanını, önceki komutun oluşturulduğu en son geçişe güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-213">`ef database update`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="34e3d-214">Bu komut, veritabanını `Up` oluşturan *geçişler/{Time-damga} _ınitialcreate. cs* dosyasındaki yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-214">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [ more information on the CLI tools for EF Core](~/includes/ef-cli.md)]

---

### <a name="the-initialcreate-class"></a><span data-ttu-id="34e3d-215">Initialcreate sınıfı</span><span class="sxs-lookup"><span data-stu-id="34e3d-215">The InitialCreate class</span></span>

<span data-ttu-id="34e3d-216">*Geçişleri/{timestamp} _ınitialcreate. cs* geçiş dosyasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-216">Examine the *Migrations/{timestamp}_InitialCreate.cs* migration file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Migrations/20190805165915_InitialCreate.cs?name=snippet)]

 <span data-ttu-id="34e3d-217">Yöntemi, film tablosunu oluşturur ve birincil anahtar `Id` olarak yapılandırır. `Up`</span><span class="sxs-lookup"><span data-stu-id="34e3d-217">The `Up` method creates the Movie table and configures `Id` as the primary key.</span></span> <span data-ttu-id="34e3d-218">Yöntemi, `Up` geçiş tarafından yapılan şema değişikliklerini geri alır. `Down`</span><span class="sxs-lookup"><span data-stu-id="34e3d-218">The `Down` method reverts the schema changes made by the `Up` migration.</span></span>

<a name="test"></a>

## <a name="test-the-app"></a><span data-ttu-id="34e3d-219">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="34e3d-219">Test the app</span></span>

* <span data-ttu-id="34e3d-220">Uygulamayı çalıştırın ve **film uygulaması** bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="34e3d-220">Run the app and click the **Movie App** link.</span></span>

  <span data-ttu-id="34e3d-221">Aşağıdakilerden birine benzer bir özel durum alırsanız:</span><span class="sxs-lookup"><span data-stu-id="34e3d-221">If you get an exception similar to one of the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e3d-222">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-222">Visual Studio</span></span>](#tab/visual-studio)

  ```console
  SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="34e3d-223">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-223">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

  ```console
  SqliteException: SQLite Error 1: 'no such table: Movie'.
  ```

---
  <span data-ttu-id="34e3d-224">Muhtemelen [geçişler adımını](#migration)kaçırdınız.</span><span class="sxs-lookup"><span data-stu-id="34e3d-224">You probably missed the [migrations step](#migration).</span></span>

* <span data-ttu-id="34e3d-225">**Oluştur** sayfasını test edin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-225">Test the **Create** page.</span></span> <span data-ttu-id="34e3d-226">Veri girin ve gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="34e3d-226">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="34e3d-227">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="34e3d-227">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="34e3d-228">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-228">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="34e3d-229">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="34e3d-229">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="34e3d-230">**Düzenleme**, **Ayrıntılar**ve **silme** sayfalarını test edin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-230">Test the **Edit**, **Details**, and **Delete** pages.</span></span>

## <a name="dependency-injection-in-the-controller"></a><span data-ttu-id="34e3d-231">Denetleyiciye bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="34e3d-231">Dependency injection in the controller</span></span>

<span data-ttu-id="34e3d-232">*Controllers/MoviesController. cs* dosyasını açın ve oluşturucuyu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-232">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller (or use the old one as I did in the 3.0 upgrade) because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="34e3d-233">Oluşturucu, veritabanı bağlamını (`MvcMovieContext`) denetleyiciye eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-233">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="34e3d-234">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="34e3d-234">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="34e3d-235">Türü kesin belirlenmiş modeller ve @model anahtar sözcüğü</span><span class="sxs-lookup"><span data-stu-id="34e3d-235">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="34e3d-236">Bu öğreticide daha önce, bir denetleyicinin `ViewData` sözlüğü kullanarak bir görünüme nasıl veri veya nesne geçirekullanabileceğinizi gördünüz.</span><span class="sxs-lookup"><span data-stu-id="34e3d-236">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="34e3d-237">Sözlük `ViewData` , bir görünüme bilgi geçirmek için uygun, geç bağlanan bir yol sağlayan dinamik bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-237">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="34e3d-238">MVC Ayrıca, kesin olarak belirlenmiş model nesnelerini bir görünüme geçirmeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-238">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="34e3d-239">Bu kesin türü belirtilmiş yaklaşım derleme zamanı kodu denetimini sunar.</span><span class="sxs-lookup"><span data-stu-id="34e3d-239">This strongly typed approach enables compile time code checking.</span></span> <span data-ttu-id="34e3d-240">Yapı iskelesi mekanizması bu yaklaşımı (yani, türü kesin belirlenmiş bir model geçirerek) `MoviesController` sınıf ve görünümlerle kullandı.</span><span class="sxs-lookup"><span data-stu-id="34e3d-240">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views.</span></span>

<span data-ttu-id="34e3d-241">`Details` *Controllers/MoviesController. cs* dosyasındaki oluşturulan yöntemi inceleyin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-241">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="34e3d-242">`id` Parametre genellikle rota verileri olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-242">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="34e3d-243">Örneğin `https://localhost:5001/movies/details/1` :</span><span class="sxs-lookup"><span data-stu-id="34e3d-243">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="34e3d-244">`movies` Denetleyicinin denetleyicisi (ilk URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="34e3d-244">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="34e3d-245">Eylem `details` (ikinci URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="34e3d-245">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="34e3d-246">Kimliği 1 ' e (son URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="34e3d-246">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="34e3d-247">Ayrıca, `id` aşağıdaki gibi bir sorgu dizesi ile de geçiş yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="34e3d-247">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="34e3d-248">Parametre `id` , bir kimlik değeri sağlanmadığından [null yapılabilir](/dotnet/csharp/programming-guide/nullable-types/index) bir`int?`tür () olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-248">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="34e3d-249">Bir [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) , rota verileriyle veya `FirstOrDefaultAsync` sorgu dizesi değeriyle eşleşen film varlıklarını seçmek için öğesine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-249">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="34e3d-250">Bir film bulunursa, `Movie` `Details` görünüme bir model örneği geçirilir:</span><span class="sxs-lookup"><span data-stu-id="34e3d-250">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="34e3d-251">*Görünümler/filmler/ayrıntılar. cshtml* dosyasının içeriğini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-251">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="34e3d-252">Görünüm dosyasının en üstündeki ifade,görünümünbeklediğinesnetürünübelirtir.`@model`</span><span class="sxs-lookup"><span data-stu-id="34e3d-252">The `@model` statement at the top of the view file specifies the type of object that the view expects.</span></span> <span data-ttu-id="34e3d-253">Film denetleyicisi oluşturulduğunda, aşağıdaki `@model` ifade eklenmiştir:</span><span class="sxs-lookup"><span data-stu-id="34e3d-253">When the movie controller was created, the following `@model` statement was included:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="34e3d-254">Bu `@model` yönerge, denetleyicinin görünüme geçirildiği filme erişimine izin verir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-254">This `@model` directive allows access to the movie that the controller passed to the view.</span></span> <span data-ttu-id="34e3d-255">`Model` Nesne kesin olarak belirlenmiş.</span><span class="sxs-lookup"><span data-stu-id="34e3d-255">The `Model` object is strongly typed.</span></span> <span data-ttu-id="34e3d-256">Örneğin, *details. cshtml* görünümünde, kod her film alanını `DisplayNameFor` `DisplayFor` türü kesin belirlenmiş `Model` nesne ile HTML yardımcılarının içine geçirir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-256">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="34e3d-257">Ve yöntemleri ve görünümleri de bir `Movie` model nesnesi iletir. `Edit` `Create`</span><span class="sxs-lookup"><span data-stu-id="34e3d-257">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="34e3d-258">*Index. cshtml* görünümünü ve `Index` filmler denetleyicisindeki yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-258">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="34e3d-259">Yöntemi çağırdığında kodun nasıl bir `List` nesne oluşturduğunu fark edin. `View`</span><span class="sxs-lookup"><span data-stu-id="34e3d-259">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="34e3d-260">Kod bu `Movies` listeyi `Index` eylem yönteminden görünüme geçirir:</span><span class="sxs-lookup"><span data-stu-id="34e3d-260">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="34e3d-261">Film denetleyicisi oluşturulduğunda, yapı iskelesi *Index. cshtml* dosyasının en üstüne `@model` aşağıdaki ifadeyi içeriyordu:</span><span class="sxs-lookup"><span data-stu-id="34e3d-261">When the movies controller was created, scaffolding included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="34e3d-262">Yönergesi, kesin olarak belirlenmiş bir `Model` nesne kullanarak denetleyicinin görünüme geçirildiği film listesine erişmenizi sağlar. `@model`</span><span class="sxs-lookup"><span data-stu-id="34e3d-262">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="34e3d-263">Örneğin, *Index. cshtml* görünümünde, kod kesin türü belirtilmiş `foreach` `Model` nesne üzerinde bir deyimle birlikte filmlerde döngü yapılır:</span><span class="sxs-lookup"><span data-stu-id="34e3d-263">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="34e3d-264">Nesne türü kesin belirlenmiş olduğundan (bir `IEnumerable<Movie>` nesne olarak), döngüdeki her öğe olarak `Movie`yazılır. `Model`</span><span class="sxs-lookup"><span data-stu-id="34e3d-264">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="34e3d-265">Diğer avantajların yanı sıra, kodu derleme zaman denetimini alacağınız anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-265">Among other benefits, this means that you get compile time checking of the code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="34e3d-266">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="34e3d-266">Additional resources</span></span>

* [<span data-ttu-id="34e3d-267">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="34e3d-267">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="34e3d-268">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="34e3d-268">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="34e3d-269">[Daha önce](adding-view.md)
> [SQL ile çalışan](working-with-sql.md) bir görünüm ekleme</span><span class="sxs-lookup"><span data-stu-id="34e3d-269">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="34e3d-270">Veri modeli sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="34e3d-270">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e3d-271">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-271">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="34e3d-272">**Sınıf**Ekle > > *modeller* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="34e3d-272">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="34e3d-273">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="34e3d-273">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="34e3d-274">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-274">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="34e3d-275">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="34e3d-275">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="34e3d-276">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="34e3d-276">Scaffold the movie model</span></span>

<span data-ttu-id="34e3d-277">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="34e3d-277">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="34e3d-278">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="34e3d-278">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e3d-279">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-279">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="34e3d-280">**Çözüm Gezgini**, *denetleyiciler* klasörüne sağ tıklayıp **yeni > yapı iskelesi > öğesi ekleyin**.</span><span class="sxs-lookup"><span data-stu-id="34e3d-280">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Yukarıdaki adımın görünümü](adding-model/_static/add_controller21.png)

<span data-ttu-id="34e3d-282">**Yapı Ekle** iletişim kutusunda, **Entity Framework > Ekle ' yi kullanarak views ile MVC denetleyicisi '** ni seçin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-282">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Yapı Iskelesi Ekle iletişim kutusu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="34e3d-284">**Denetleyici Ekle** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="34e3d-284">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="34e3d-285">**Model Sınıfı:** *Film (MvcMovie. modeller)*</span><span class="sxs-lookup"><span data-stu-id="34e3d-285">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="34e3d-286">**Veri bağlamı sınıfı:** Simgeyi seçin ve varsayılan **mvcmovie. modeller. mvcmoviecontext öğesini ekleyin** **+**</span><span class="sxs-lookup"><span data-stu-id="34e3d-286">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Veri bağlamı Ekle](adding-model/_static/dc.png)

* <span data-ttu-id="34e3d-288">**Görünümler:** Her seçeneğin varsayılan kısmını işaretli tut</span><span class="sxs-lookup"><span data-stu-id="34e3d-288">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="34e3d-289">**Denetleyici adı:** Varsayılan *MoviesController* tut</span><span class="sxs-lookup"><span data-stu-id="34e3d-289">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="34e3d-290">**Ekle** 'yi seçin</span><span class="sxs-lookup"><span data-stu-id="34e3d-290">Select **Add**</span></span>

![Denetleyici Ekle iletişim kutusu](adding-model/_static/add_controller2.png)

<span data-ttu-id="34e3d-292">Visual Studio şunları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="34e3d-292">Visual Studio creates:</span></span>

* <span data-ttu-id="34e3d-293">Entity Framework Core [veritabanı bağlam sınıfı](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext. cs*)</span><span class="sxs-lookup"><span data-stu-id="34e3d-293">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="34e3d-294">Bir filmler denetleyicisi (*denetleyiciler/MoviesController. cs*)</span><span class="sxs-lookup"><span data-stu-id="34e3d-294">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="34e3d-295">Razor oluşturma, silme, ayrıntılar, düzenleme ve dizin sayfaları için dosyaları görüntüleme (*Görünümler/filmler/\*. cshtml*)</span><span class="sxs-lookup"><span data-stu-id="34e3d-295">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="34e3d-296">Veritabanı bağlamı ve [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemlerinin ve görünümlerinin otomatik olarak oluşturulması, *Yapı iskelesi*olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-296">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="34e3d-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="34e3d-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="34e3d-298">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="34e3d-298">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="34e3d-299">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-299">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="34e3d-300">Linux 'ta, scafkatlama aracı yolunu dışarı aktarın:</span><span class="sxs-lookup"><span data-stu-id="34e3d-300">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="34e3d-301">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="34e3d-301">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="34e3d-302">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-302">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="34e3d-303">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="34e3d-303">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="34e3d-304">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-304">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="34e3d-305">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="34e3d-305">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="34e3d-306">Uygulamayı çalıştırır ve **MVC filmi** bağlantısına tıklarsanız aşağıdakine benzer bir hata alırsınız:</span><span class="sxs-lookup"><span data-stu-id="34e3d-306">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e3d-307">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-307">Visual Studio</span></span>](#tab/visual-studio)

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="34e3d-308">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-308">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

``` error
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)

```

---

<span data-ttu-id="34e3d-309">Veritabanını oluşturmanız ve bunu yapmak için EF Core [geçişleri](xref:data/ef-mvc/migrations) özelliğini kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-309">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="34e3d-310">Geçişler veri modelinize uyan bir veritabanı oluşturmanıza ve veri modeliniz değiştiğinde veritabanı şemasını güncelleştirmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-310">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="34e3d-311">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="34e3d-311">Initial migration</span></span>

<span data-ttu-id="34e3d-312">Bu bölümde, aşağıdaki görevler tamamlanır:</span><span class="sxs-lookup"><span data-stu-id="34e3d-312">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="34e3d-313">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-313">Add an initial migration.</span></span>
* <span data-ttu-id="34e3d-314">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-314">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e3d-315">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-315">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="34e3d-316">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** (PMC) öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-316">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![PMC menüsü](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="34e3d-318">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-318">In the PMC, enter the following commands:</span></span>

   ```console
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="34e3d-319">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="34e3d-319">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="34e3d-320">Veritabanı şeması, `MvcMovieContext` sınıfında belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-320">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span> <span data-ttu-id="34e3d-321">`Initial` Bağımsız değişkeni geçiş adıdır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-321">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="34e3d-322">Herhangi bir ad kullanılabilir, ancak kurala göre, geçişi açıklayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-322">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="34e3d-323">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="34e3d-323">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="34e3d-324">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="34e3d-324">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="34e3d-325">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-325">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="34e3d-326">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="34e3d-326">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="34e3d-327">Veritabanı şeması `MvcMovieContext` sınıfında belirtilen modeli temel alır ( *Data/mvcmoviecontext. cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="34e3d-327">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="34e3d-328">`InitialCreate` Bağımsız değişkeni geçiş adıdır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-328">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="34e3d-329">Herhangi bir ad kullanılabilir, ancak kurala göre, geçişi açıklayan bir ad seçilidir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-329">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="34e3d-330">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="34e3d-330">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="34e3d-331">ASP.NET Core, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="34e3d-331">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="34e3d-332">Hizmetler (EF Core DB bağlamı gibi) uygulama başlatma sırasında dı ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-332">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="34e3d-333">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-333">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="34e3d-334">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-334">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e3d-335">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-335">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="34e3d-336">Scafkatlama aracı otomatik olarak bir DB bağlamı oluşturup dı kapsayıcısına kaydetti.</span><span class="sxs-lookup"><span data-stu-id="34e3d-336">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="34e3d-337">Aşağıdaki `Startup.ConfigureServices` yöntemi inceleyin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-337">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="34e3d-338">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="34e3d-338">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

<span data-ttu-id="34e3d-339">`MvcMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="34e3d-339">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="34e3d-340">Veri bağlamı (`MvcMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="34e3d-340">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="34e3d-341">Veri bağlamı, veri modeline hangi varlıkların ekleneceğini belirtir:</span><span class="sxs-lookup"><span data-stu-id="34e3d-341">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Data/MvcMovieContext.cs)]

<span data-ttu-id="34e3d-342">Önceki kod, varlık kümesi için bir [dbset\<filmi >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="34e3d-342">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="34e3d-343">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-343">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="34e3d-344">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-344">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="34e3d-345">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="34e3d-345">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="34e3d-346">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="34e3d-346">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="34e3d-347">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e3d-347">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="34e3d-348">Bir DB bağlamı oluşturdunuz ve bunu DI kapsayıcısı ile kaydettiniz.</span><span class="sxs-lookup"><span data-stu-id="34e3d-348">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="34e3d-349">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="34e3d-349">Test the app</span></span>

* <span data-ttu-id="34e3d-350">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="34e3d-350">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="34e3d-351">Aşağıdakine benzer bir veritabanı özel durumu alırsanız:</span><span class="sxs-lookup"><span data-stu-id="34e3d-351">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="34e3d-352">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="34e3d-352">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="34e3d-353">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="34e3d-353">Test the **Create** link.</span></span> <span data-ttu-id="34e3d-354">Veri girin ve gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="34e3d-354">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="34e3d-355">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="34e3d-355">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="34e3d-356">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-356">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="34e3d-357">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="34e3d-357">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="34e3d-358">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="34e3d-358">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="34e3d-359">`Startup` Sınıfı inceleyin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-359">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="34e3d-360">Önceki vurgulanan kod, [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına eklenen film veritabanı bağlamını gösterir:</span><span class="sxs-lookup"><span data-stu-id="34e3d-360">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="34e3d-361">`services.AddDbContext<MvcMovieContext>(options =>`kullanılacak veritabanını ve bağlantı dizesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-361">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="34e3d-362">`=>`bir [lambda operatörü](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="34e3d-362">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="34e3d-363">*Controllers/MoviesController. cs* dosyasını açın ve oluşturucuyu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-363">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="34e3d-364">Oluşturucu, veritabanı bağlamını (`MvcMovieContext`) denetleyiciye eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-364">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="34e3d-365">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="34e3d-365">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="34e3d-366">Türü kesin belirlenmiş modeller ve @model anahtar sözcüğü</span><span class="sxs-lookup"><span data-stu-id="34e3d-366">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="34e3d-367">Bu öğreticide daha önce, bir denetleyicinin `ViewData` sözlüğü kullanarak bir görünüme nasıl veri veya nesne geçirekullanabileceğinizi gördünüz.</span><span class="sxs-lookup"><span data-stu-id="34e3d-367">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="34e3d-368">Sözlük `ViewData` , bir görünüme bilgi geçirmek için uygun, geç bağlanan bir yol sağlayan dinamik bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-368">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="34e3d-369">MVC Ayrıca, kesin olarak belirlenmiş model nesnelerini bir görünüme geçirmeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-369">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="34e3d-370">Bu kesin türü belirtilmiş yaklaşım, kodunuzun daha iyi derleme zaman denetimini sunar.</span><span class="sxs-lookup"><span data-stu-id="34e3d-370">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="34e3d-371">Yapı iskelesi mekanizması, bu yaklaşımı (yani, türü kesin belirlenmiş bir model geçirerek), yöntem ve `MoviesController` görünümleri oluştururken sınıfı ve görünümleri ile kullandı.</span><span class="sxs-lookup"><span data-stu-id="34e3d-371">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="34e3d-372">`Details` *Controllers/MoviesController. cs* dosyasındaki oluşturulan yöntemi inceleyin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-372">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="34e3d-373">`id` Parametre genellikle rota verileri olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-373">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="34e3d-374">Örneğin `https://localhost:5001/movies/details/1` :</span><span class="sxs-lookup"><span data-stu-id="34e3d-374">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="34e3d-375">`movies` Denetleyicinin denetleyicisi (ilk URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="34e3d-375">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="34e3d-376">Eylem `details` (ikinci URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="34e3d-376">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="34e3d-377">Kimliği 1 ' e (son URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="34e3d-377">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="34e3d-378">Ayrıca, `id` aşağıdaki gibi bir sorgu dizesi ile de geçiş yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="34e3d-378">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="34e3d-379">Parametre `id` , bir kimlik değeri sağlanmadığından [null yapılabilir](/dotnet/csharp/programming-guide/nullable-types/index) bir`int?`tür () olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="34e3d-379">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="34e3d-380">Bir [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) , rota verileriyle veya `FirstOrDefaultAsync` sorgu dizesi değeriyle eşleşen film varlıklarını seçmek için öğesine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-380">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="34e3d-381">Bir film bulunursa, `Movie` `Details` görünüme bir model örneği geçirilir:</span><span class="sxs-lookup"><span data-stu-id="34e3d-381">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="34e3d-382">*Görünümler/filmler/ayrıntılar. cshtml* dosyasının içeriğini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-382">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="34e3d-383">Görünüm dosyasının üst `@model` kısmına bir ifade ekleyerek, görünümün beklediği nesne türünü belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="34e3d-383">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="34e3d-384">Film denetleyicisini oluştururken, *Ayrıntılar. cshtml* dosyasının en `@model` üstüne aşağıdaki ifade otomatik olarak eklenmiştir:</span><span class="sxs-lookup"><span data-stu-id="34e3d-384">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="34e3d-385">Bu `@model` yönerge, kesin olarak belirlenmiş bir `Model` nesne kullanarak denetleyicinin görünüme geçirildiği filme erişmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="34e3d-385">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="34e3d-386">Örneğin, *details. cshtml* görünümünde, kod her film alanını `DisplayNameFor` `DisplayFor` türü kesin belirlenmiş `Model` nesne ile HTML yardımcılarının içine geçirir.</span><span class="sxs-lookup"><span data-stu-id="34e3d-386">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="34e3d-387">Ve yöntemleri ve görünümleri de bir `Movie` model nesnesi iletir. `Edit` `Create`</span><span class="sxs-lookup"><span data-stu-id="34e3d-387">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="34e3d-388">*Index. cshtml* görünümünü ve `Index` filmler denetleyicisindeki yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="34e3d-388">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="34e3d-389">Yöntemi çağırdığında kodun nasıl bir `List` nesne oluşturduğunu fark edin. `View`</span><span class="sxs-lookup"><span data-stu-id="34e3d-389">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="34e3d-390">Kod bu `Movies` listeyi `Index` eylem yönteminden görünüme geçirir:</span><span class="sxs-lookup"><span data-stu-id="34e3d-390">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="34e3d-391">Film denetleyicisini oluştururken, yapı iskelesi *Index. cshtml* dosyasının en üstüne aşağıdaki `@model` ifadeyi otomatik olarak dahil edin:</span><span class="sxs-lookup"><span data-stu-id="34e3d-391">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="34e3d-392">Yönergesi, kesin olarak belirlenmiş bir `Model` nesne kullanarak denetleyicinin görünüme geçirildiği film listesine erişmenizi sağlar. `@model`</span><span class="sxs-lookup"><span data-stu-id="34e3d-392">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="34e3d-393">Örneğin, *Index. cshtml* görünümünde, kod kesin türü belirtilmiş `foreach` `Model` nesne üzerinde bir deyimle birlikte filmlerde döngü yapılır:</span><span class="sxs-lookup"><span data-stu-id="34e3d-393">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="34e3d-394">Nesne türü kesin belirlenmiş olduğundan (bir `IEnumerable<Movie>` nesne olarak), döngüdeki her öğe olarak `Movie`yazılır. `Model`</span><span class="sxs-lookup"><span data-stu-id="34e3d-394">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="34e3d-395">Diğer avantajların yanı sıra, kodun derleme zaman denetimini alacağınız anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="34e3d-395">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="34e3d-396">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="34e3d-396">Additional resources</span></span>

* [<span data-ttu-id="34e3d-397">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="34e3d-397">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="34e3d-398">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="34e3d-398">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="34e3d-399">[Daha önce](adding-view.md)
> [SQL ile çalışan](working-with-sql.md) bir görünüm ekleme</span><span class="sxs-lookup"><span data-stu-id="34e3d-399">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end
