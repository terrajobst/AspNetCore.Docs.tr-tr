---
title: ASP.NET Core MVC uygulamasına model ekleme
author: rick-anderson
description: Basit bir ASP.NET Core uygulamasına model ekleyin.
ms.author: riande
ms.date: 8/15/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 2fac37e7069fb2a464d4de1da8912197f7adf8a8
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73761096"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="a026e-103">ASP.NET Core MVC uygulamasına model ekleme</span><span class="sxs-lookup"><span data-stu-id="a026e-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="a026e-104">[Rick Anderson](https://twitter.com/RickAndMSFT) ve [Tom Dykstra](https://github.com/tdykstra) tarafından</span><span class="sxs-lookup"><span data-stu-id="a026e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="a026e-105">Bu bölümde, bir veritabanında film yönetmeye yönelik sınıflar eklersiniz.</span><span class="sxs-lookup"><span data-stu-id="a026e-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="a026e-106">Bu sınıflar, **d**VC uygulamasının "**d**odel" parçası olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a026e-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="a026e-107">Bu sınıfları bir veritabanıyla çalışmak için [Entity Framework Core](/ef/core) (EF Core) ile birlikte kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="a026e-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="a026e-108">EF Core, yazmanız gereken veri erişim kodunu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="a026e-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="a026e-109">Oluşturduğunuz model sınıfları, EF Core hiçbir bağımlılığı olmadığından, POCO sınıfları olarak bilinir ( **P**Lain **O**).</span><span class="sxs-lookup"><span data-stu-id="a026e-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="a026e-110">Yalnızca veritabanında depolanacak verilerin özelliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a026e-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="a026e-111">Bu öğreticide, önce model sınıflarını yazdığınızda EF Core veritabanını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a026e-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="a026e-112">Burada kapsanmayan alternatif bir yaklaşım var olan bir veritabanından model sınıfları oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="a026e-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="a026e-113">Bu yaklaşım hakkında daha fazla bilgi için bkz. [ASP.NET Core-var olan veritabanı](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="a026e-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="a026e-114">Veri modeli sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="a026e-114">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a026e-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a026e-116"> > **sınıf** **eklemek** > *modeller* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a026e-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="a026e-117">Dosyayı *Movie.cs*olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="a026e-117">Name the file *Movie.cs*.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a026e-118">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-118">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="a026e-119">*Modeller* klasörüne *Movie.cs* adlı bir dosya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a026e-119">Add a file named *Movie.cs* to the *Models* folder.</span></span>

---

<span data-ttu-id="a026e-120">*Movie.cs* dosyasını aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="a026e-120">Update the *Movie.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/Movie.cs)]

<span data-ttu-id="a026e-121">`Movie` sınıfı, birincil anahtar için veritabanı için gerekli olan bir `Id` alanı içerir.</span><span class="sxs-lookup"><span data-stu-id="a026e-121">The `Movie` class contains an `Id` field, which is required by the database for the primary key.</span></span>

<span data-ttu-id="a026e-122">`ReleaseDate` [veri türü özniteliği,](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) verilerin türünü belirtir (`Date`).</span><span class="sxs-lookup"><span data-stu-id="a026e-122">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute on `ReleaseDate` specifies the type of the data (`Date`).</span></span> <span data-ttu-id="a026e-123">Bu öznitelikle:</span><span class="sxs-lookup"><span data-stu-id="a026e-123">With this attribute:</span></span>

  * <span data-ttu-id="a026e-124">Kullanıcının Tarih alanına saat bilgilerini girmesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a026e-124">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="a026e-125">Zaman bilgisi değil yalnızca tarih görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="a026e-125">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="a026e-126">[Veri açıklamaları](/dotnet/api/system.componentmodel.dataannotations) sonraki bir öğreticide ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="a026e-126">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="a026e-127">NuGet paketleri Ekle</span><span class="sxs-lookup"><span data-stu-id="a026e-127">Add NuGet packages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a026e-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-128">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a026e-129">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** (PMC) öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="a026e-129">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

![PMC menüsü](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="a026e-131">PMC 'de şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a026e-131">In the PMC, run the following command:</span></span>

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="a026e-132">Yukarıdaki komut, EF Core SQL Server sağlayıcısını ekler.</span><span class="sxs-lookup"><span data-stu-id="a026e-132">The preceding command adds the EF Core SQL Server provider.</span></span> <span data-ttu-id="a026e-133">Sağlayıcı paketi, EF Core paketini bir bağımlılık olarak yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="a026e-133">The provider package installs the EF Core package as a dependency.</span></span> <span data-ttu-id="a026e-134">Ek paketler, öğreticinin sonraki bölümlerinde bulunan yapı iskelesi adımında otomatik olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="a026e-134">Additional packages are installed automatically in the scaffolding step later in the tutorial.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a026e-135">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-135">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

---

<a name="dc"></a>

## <a name="create-a-database-context-class"></a><span data-ttu-id="a026e-136">Veritabanı bağlamı sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="a026e-136">Create a database context class</span></span>

<span data-ttu-id="a026e-137">`Movie` modeli için EF Core işlevselliği (oluşturma, okuma, güncelleştirme, silme) koordine etmek için bir veritabanı bağlamı sınıfı gerekir.</span><span class="sxs-lookup"><span data-stu-id="a026e-137">A database context class is needed to coordinate EF Core functionality (Create, Read, Update, Delete) for the `Movie` model.</span></span> <span data-ttu-id="a026e-138">Veritabanı bağlamı [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) öğesinden türetilir ve veri modeline dahil edilecek varlıkları belirtir.</span><span class="sxs-lookup"><span data-stu-id="a026e-138">The database context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and specifies the entities to include in the data model.</span></span>

<span data-ttu-id="a026e-139">Bir *veri* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a026e-139">Create a *Data* folder.</span></span>

<span data-ttu-id="a026e-140">Aşağıdaki kodla bir *Data/MvcMovieContext. cs* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a026e-140">Add a *Data/MvcMovieContext.cs* file with the following code:</span></span> 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="a026e-141">Önceki kod, varlık kümesi için bir [Dbset\<filmi >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a026e-141">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="a026e-142">Entity Framework terminolojisinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a026e-142">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="a026e-143">Bir varlık, tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a026e-143">An entity corresponds to a row in the table.</span></span>

<a name="reg"></a>

## <a name="register-the-database-context"></a><span data-ttu-id="a026e-144">Veritabanı bağlamını kaydetme</span><span class="sxs-lookup"><span data-stu-id="a026e-144">Register the database context</span></span>

<span data-ttu-id="a026e-145">ASP.NET Core, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="a026e-145">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a026e-146">Hizmetlerin (EF Core DB bağlamı gibi) uygulama başlatma sırasında DI ile kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a026e-146">Services (such as the EF Core DB context) must be registered with DI during application startup.</span></span> <span data-ttu-id="a026e-147">Bu hizmetleri gerektiren bileşenler (örneğin Razor Pages), bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="a026e-147">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="a026e-148">Bir DB bağlam örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a026e-148">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span> <span data-ttu-id="a026e-149">Bu bölümde, veritabanı bağlamını dı kapsayıcısına kaydedersiniz.</span><span class="sxs-lookup"><span data-stu-id="a026e-149">In this section, you register the database context with the DI container.</span></span>

<span data-ttu-id="a026e-150">Aşağıdaki `using` deyimlerini *Startup.cs*üst kısmına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a026e-150">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="a026e-151">Aşağıdaki Vurgulanan kodu `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a026e-151">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a026e-152">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-152">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a026e-153">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-153">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

---

<span data-ttu-id="a026e-154">Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a026e-154">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="a026e-155">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="a026e-155">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="cs"></a>

## <a name="add-a-database-connection-string"></a><span data-ttu-id="a026e-156">Veritabanı bağlantı dizesi Ekle</span><span class="sxs-lookup"><span data-stu-id="a026e-156">Add a database connection string</span></span>

<span data-ttu-id="a026e-157">*AppSettings. JSON* dosyasına bir bağlantı dizesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a026e-157">Add a connection string to the *appsettings.json* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a026e-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-158">Visual Studio</span></span>](#tab/visual-studio)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a026e-159">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-159">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

---

<span data-ttu-id="a026e-160">Projeyi derleyici hatalarına yönelik bir denetim olarak derleyin.</span><span class="sxs-lookup"><span data-stu-id="a026e-160">Build the project as a check for compiler errors.</span></span>

## <a name="scaffold-movie-pages"></a><span data-ttu-id="a026e-161">Yapı iskelesi film sayfaları</span><span class="sxs-lookup"><span data-stu-id="a026e-161">Scaffold movie pages</span></span>

<span data-ttu-id="a026e-162">Film modeli için oluşturma, okuma, güncelleştirme ve silme (CRUD) sayfaları üretmek için scafkatlama aracını kullanın.</span><span class="sxs-lookup"><span data-stu-id="a026e-162">Use the scaffolding tool to produce Create, Read, Update, and Delete (CRUD) pages for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a026e-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-163">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a026e-164">**Çözüm Gezgini**, *denetleyiciler* klasörüne sağ tıklayıp **yeni > yapı iskelesi > öğesi ekleyin**.</span><span class="sxs-lookup"><span data-stu-id="a026e-164">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Yukarıdaki adımın görünümü](adding-model/_static/add_controller21.png)

<span data-ttu-id="a026e-166">**Yapı Ekle** iletişim kutusunda, **Entity Framework > Ekle ' yi kullanarak views ile MVC denetleyicisi '** ni seçin.</span><span class="sxs-lookup"><span data-stu-id="a026e-166">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Yapı Iskelesi Ekle iletişim kutusu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="a026e-168">**Denetleyici Ekle** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="a026e-168">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="a026e-169">**Model sınıfı:** *Film (mvcmovie. modeller)*</span><span class="sxs-lookup"><span data-stu-id="a026e-169">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="a026e-170">**Veri bağlamı sınıfı:** *mvcmoviecontext (mvcmovie. Data)*</span><span class="sxs-lookup"><span data-stu-id="a026e-170">**Data context class:** *MvcMovieContext (MvcMovie.Data)*</span></span>

![Veri bağlamı Ekle](adding-model/_static/dc3.png)

* <span data-ttu-id="a026e-172">**Görünümler:** Her seçeneğin varsayılan kısmını işaretli tut</span><span class="sxs-lookup"><span data-stu-id="a026e-172">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="a026e-173">**Denetleyici adı:** Varsayılan *MoviesController* tut</span><span class="sxs-lookup"><span data-stu-id="a026e-173">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="a026e-174">**Ekle** 'yi seçin</span><span class="sxs-lookup"><span data-stu-id="a026e-174">Select **Add**</span></span>

<span data-ttu-id="a026e-175">Visual Studio şunları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a026e-175">Visual Studio creates:</span></span>

* <span data-ttu-id="a026e-176">Bir filmler denetleyicisi (*denetleyiciler/MoviesController. cs*)</span><span class="sxs-lookup"><span data-stu-id="a026e-176">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="a026e-177">Razor oluşturma, silme, ayrıntılar, düzenleme ve dizin sayfaları için dosyaları görüntüleme (*Görünümler/filmler/\*. cshtml*)</span><span class="sxs-lookup"><span data-stu-id="a026e-177">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="a026e-178">Bu dosyaların otomatik olarak oluşturulması, *Yapı iskelesi*olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="a026e-178">The automatic creation of these files is known as *scaffolding*.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a026e-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a026e-179">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="a026e-180">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="a026e-180">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="a026e-181">Linux 'ta, scafkatlama aracı yolunu dışarı aktarın:</span><span class="sxs-lookup"><span data-stu-id="a026e-181">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="a026e-182">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a026e-182">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a026e-183">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-183">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a026e-184">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="a026e-184">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="a026e-185">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a026e-185">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of tabs                  -->

<span data-ttu-id="a026e-186">Veritabanı mevcut olmadığından, scafkatmış sayfaları henüz kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="a026e-186">You can't use the scaffolded pages yet because the database doesn't exist.</span></span> <span data-ttu-id="a026e-187">Uygulamayı çalıştırır ve **film uygulaması** bağlantısına tıklarsanız, bir *veritabanı* açılamıyor veya *böyle bir tablo yok: film* hata iletisi.</span><span class="sxs-lookup"><span data-stu-id="a026e-187">If you run the app and click on the **Movie App** link, you get a *Cannot open database* or *no such table: Movie* error message.</span></span>

<a name="migration"></a>

## <a name="initial-migration"></a><span data-ttu-id="a026e-188">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="a026e-188">Initial migration</span></span>

<span data-ttu-id="a026e-189">Veritabanını oluşturmak için EF Core [geçişleri](xref:data/ef-mvc/migrations) özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a026e-189">Use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to create the database.</span></span> <span data-ttu-id="a026e-190">Geçişler, veri modelinizle eşleşecek bir veritabanı oluşturmanıza ve güncelleştirmenize olanak sağlayan bir araç kümesidir.</span><span class="sxs-lookup"><span data-stu-id="a026e-190">Migrations is a set of tools that let you create and update a database to match your data model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a026e-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-191">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a026e-192">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** (PMC) öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="a026e-192">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

<span data-ttu-id="a026e-193">PMC 'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="a026e-193">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

* <span data-ttu-id="a026e-194">`Add-Migration InitialCreate`: bir *geçişler/{timestamp} _ınitialcreate. cs* geçiş dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a026e-194">`Add-Migration InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="a026e-195">`InitialCreate` bağımsız değişkeni geçiş adıdır.</span><span class="sxs-lookup"><span data-stu-id="a026e-195">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="a026e-196">Herhangi bir ad kullanılabilir, ancak kurala göre, geçişi açıklayan bir ad seçilidir.</span><span class="sxs-lookup"><span data-stu-id="a026e-196">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="a026e-197">Bu ilk geçiş olduğundan, oluşturulan sınıf veritabanı şemasını oluşturmak için kod içerir.</span><span class="sxs-lookup"><span data-stu-id="a026e-197">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="a026e-198">Veritabanı şeması, `MvcMovieContext` sınıfında belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="a026e-198">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span>

* <span data-ttu-id="a026e-199">`Update-Database`: veritabanını, önceki komutun oluşturulduğu en son geçişe güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="a026e-199">`Update-Database`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="a026e-200">Bu komut, veritabanını oluşturan *geçişler/{Time-damga} _ınitialcreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="a026e-200">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

  <span data-ttu-id="a026e-201">Database Update komutu aşağıdaki uyarıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="a026e-201">The database update command generates the following warning:</span></span> 

  > <span data-ttu-id="a026e-202">' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="a026e-202">No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="a026e-203">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="a026e-203">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="a026e-204">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açıkça belirtin.</span><span class="sxs-lookup"><span data-stu-id="a026e-204">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.</span></span>

  <span data-ttu-id="a026e-205">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="a026e-205">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

[!INCLUDE [more information on the PMC tools for EF Core](~/includes/ef-pmc.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a026e-206">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-206">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="a026e-207">Aşağıdaki .NET Core CLI komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a026e-207">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* <span data-ttu-id="a026e-208">`ef migrations add InitialCreate`: bir *geçişler/{timestamp} _ınitialcreate. cs* geçiş dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a026e-208">`ef migrations add InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="a026e-209">`InitialCreate` bağımsız değişkeni geçiş adıdır.</span><span class="sxs-lookup"><span data-stu-id="a026e-209">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="a026e-210">Herhangi bir ad kullanılabilir, ancak kurala göre, geçişi açıklayan bir ad seçilidir.</span><span class="sxs-lookup"><span data-stu-id="a026e-210">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="a026e-211">Bu ilk geçiş olduğundan, oluşturulan sınıf veritabanı şemasını oluşturmak için kod içerir.</span><span class="sxs-lookup"><span data-stu-id="a026e-211">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="a026e-212">Veritabanı şeması, `MvcMovieContext` sınıfında belirtilen modeli temel alır ( *Data/MvcMovieContext. cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="a026e-212">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span>

* <span data-ttu-id="a026e-213">`ef database update`: veritabanını, önceki komutun oluşturulduğu en son geçişe güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="a026e-213">`ef database update`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="a026e-214">Bu komut, veritabanını oluşturan *geçişler/{Time-damga} _ınitialcreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="a026e-214">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [ more information on the CLI tools for EF Core](~/includes/ef-cli.md)]

---

### <a name="the-initialcreate-class"></a><span data-ttu-id="a026e-215">Initialcreate sınıfı</span><span class="sxs-lookup"><span data-stu-id="a026e-215">The InitialCreate class</span></span>

<span data-ttu-id="a026e-216">*Geçişleri/{timestamp} _ınitialcreate. cs* geçiş dosyasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a026e-216">Examine the *Migrations/{timestamp}_InitialCreate.cs* migration file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Migrations/20190805165915_InitialCreate.cs?name=snippet)]

 <span data-ttu-id="a026e-217">`Up` yöntemi, film tablosunu oluşturur ve `Id` birincil anahtar olarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a026e-217">The `Up` method creates the Movie table and configures `Id` as the primary key.</span></span> <span data-ttu-id="a026e-218">`Down` yöntemi, `Up` geçişi tarafından yapılan şema değişikliklerini geri alır.</span><span class="sxs-lookup"><span data-stu-id="a026e-218">The `Down` method reverts the schema changes made by the `Up` migration.</span></span>

<a name="test"></a>

## <a name="test-the-app"></a><span data-ttu-id="a026e-219">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="a026e-219">Test the app</span></span>

* <span data-ttu-id="a026e-220">Uygulamayı çalıştırın ve **film uygulaması** bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a026e-220">Run the app and click the **Movie App** link.</span></span>

  <span data-ttu-id="a026e-221">Aşağıdakilerden birine benzer bir özel durum alırsanız:</span><span class="sxs-lookup"><span data-stu-id="a026e-221">If you get an exception similar to one of the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a026e-222">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-222">Visual Studio</span></span>](#tab/visual-studio)

  ```console
  SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a026e-223">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-223">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

  ```console
  SqliteException: SQLite Error 1: 'no such table: Movie'.
  ```

---
  <span data-ttu-id="a026e-224">Muhtemelen [geçişler adımını](#migration)kaçırdınız.</span><span class="sxs-lookup"><span data-stu-id="a026e-224">You probably missed the [migrations step](#migration).</span></span>

* <span data-ttu-id="a026e-225">**Oluştur** sayfasını test edin.</span><span class="sxs-lookup"><span data-stu-id="a026e-225">Test the **Create** page.</span></span> <span data-ttu-id="a026e-226">Veri girin ve gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a026e-226">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="a026e-227">`Price` alanına ondalık virgül giremeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a026e-227">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="a026e-228">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanın Genelleştirilmiş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a026e-228">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="a026e-229">Genelleştirme yönergeleri için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)bakın.</span><span class="sxs-lookup"><span data-stu-id="a026e-229">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="a026e-230">**Düzenleme**, **Ayrıntılar**ve **silme** sayfalarını test edin.</span><span class="sxs-lookup"><span data-stu-id="a026e-230">Test the **Edit**, **Details**, and **Delete** pages.</span></span>

## <a name="dependency-injection-in-the-controller"></a><span data-ttu-id="a026e-231">Denetleyiciye bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="a026e-231">Dependency injection in the controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a026e-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-232">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a026e-233">*Controllers/MoviesController. cs* dosyasını açın ve oluşturucuyu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a026e-233">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller (or use the old one as I did in the 3.0 upgrade) because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="a026e-234">Oluşturucu, veritabanı bağlamını (`MvcMovieContext`) denetleyiciye eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="a026e-234">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="a026e-235">Veritabanı bağlamı, denetleyicideki [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) yöntemlerinde her birinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a026e-235">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a026e-236">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-236">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="a026e-237">Oluşturucu, veritabanı bağlamını (`MvcMovieContext`) denetleyiciye eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="a026e-237">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="a026e-238">Veritabanı bağlamı, denetleyicideki [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) yöntemlerinde her birinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a026e-238">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---
<!-- end of tabs --->

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="a026e-239">Türü kesin belirlenmiş modeller ve @model anahtar sözcüğü</span><span class="sxs-lookup"><span data-stu-id="a026e-239">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="a026e-240">Bu öğreticide daha önce, bir denetleyicinin `ViewData` sözlüğünü kullanarak bir görünüme nasıl veri veya nesne geçirekullanabileceğinizi gördünüz.</span><span class="sxs-lookup"><span data-stu-id="a026e-240">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="a026e-241">`ViewData` sözlüğü bir görünüme bilgi geçirmek için uygun, geç bağlanan bir yol sağlayan dinamik bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="a026e-241">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="a026e-242">MVC Ayrıca, kesin olarak belirlenmiş model nesnelerini bir görünüme geçirmeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="a026e-242">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="a026e-243">Bu kesin türü belirtilmiş yaklaşım derleme zamanı kodu denetimini sunar.</span><span class="sxs-lookup"><span data-stu-id="a026e-243">This strongly typed approach enables compile time code checking.</span></span> <span data-ttu-id="a026e-244">Yapı iskelesi mekanizması, `MoviesController` sınıfı ve görünümleriyle bu yaklaşımı (türü kesin belirlenmiş bir modeli geçirerek) kullandı.</span><span class="sxs-lookup"><span data-stu-id="a026e-244">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views.</span></span>

<span data-ttu-id="a026e-245">*Controllers/MoviesController. cs* dosyasında oluşturulan `Details` yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a026e-245">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="a026e-246">`id` parametresi genellikle rota verileri olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a026e-246">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="a026e-247">Örneğin `https://localhost:5001/movies/details/1` kümeler:</span><span class="sxs-lookup"><span data-stu-id="a026e-247">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="a026e-248">`movies` denetleyicisine denetleyici (ilk URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="a026e-248">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="a026e-249">`details` eylemi (ikinci URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="a026e-249">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="a026e-250">Kimliği 1 ' e (son URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="a026e-250">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="a026e-251">Aşağıdaki gibi bir sorgu dizesiyle `id` de geçirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a026e-251">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="a026e-252">KIMLIK değeri sağlanmazsa `id` parametresi [null yapılabilir bir tür](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="a026e-252">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="a026e-253">Bir [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) , rota verileriyle veya sorgu dizesi değeriyle eşleşen film varlıklarını seçmek üzere `FirstOrDefaultAsync` ' A geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a026e-253">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="a026e-254">Bir film bulunursa, `Movie` modelinin bir örneği `Details` görünümüne geçirilir:</span><span class="sxs-lookup"><span data-stu-id="a026e-254">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="a026e-255">*Görünümler/filmler/ayrıntılar. cshtml* dosyasının içeriğini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a026e-255">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="a026e-256">Görünüm dosyasının en üstündeki `@model` ifade, görünümün beklediği nesne türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="a026e-256">The `@model` statement at the top of the view file specifies the type of object that the view expects.</span></span> <span data-ttu-id="a026e-257">Film denetleyicisi oluşturulduğunda, aşağıdaki `@model` deyimleri eklenmiştir:</span><span class="sxs-lookup"><span data-stu-id="a026e-257">When the movie controller was created, the following `@model` statement was included:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="a026e-258">Bu `@model` yönergesi, denetleyicinin görünüme geçirildiği filme erişimine izin verir.</span><span class="sxs-lookup"><span data-stu-id="a026e-258">This `@model` directive allows access to the movie that the controller passed to the view.</span></span> <span data-ttu-id="a026e-259">`Model` nesne kesin olarak belirlenmiş.</span><span class="sxs-lookup"><span data-stu-id="a026e-259">The `Model` object is strongly typed.</span></span> <span data-ttu-id="a026e-260">Örneğin, *details. cshtml* görünümünde, kod her bir film alanını `DisplayNameFor` `DisplayFor` ve HTML yardımcılarını türü kesin belirlenmiş `Model` nesnesiyle geçirir.</span><span class="sxs-lookup"><span data-stu-id="a026e-260">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="a026e-261">`Create` ve `Edit` yöntemleri ve görünümleri bir `Movie` model nesnesi de iletir.</span><span class="sxs-lookup"><span data-stu-id="a026e-261">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="a026e-262">*Dizin. cshtml* görünümünü ve film denetleyicisindeki `Index` yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="a026e-262">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="a026e-263">`List`, `View` yöntemini çağırdığında kodun bir nesne nasıl oluşturduğunu fark edin.</span><span class="sxs-lookup"><span data-stu-id="a026e-263">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="a026e-264">Kod, `Index` eylem yönteminden bu `Movies` listesini görünüme geçirir:</span><span class="sxs-lookup"><span data-stu-id="a026e-264">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="a026e-265">Film denetleyicisi oluşturulduğunda, yapı iskelesi *Index. cshtml* dosyasının en üstüne aşağıdaki `@model` ifadesini içeriyordu:</span><span class="sxs-lookup"><span data-stu-id="a026e-265">When the movies controller was created, scaffolding included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="a026e-266">`@model` yönergesi, kesin olarak belirlenmiş bir `Model` nesnesi kullanarak, denetleyicinin görünüme geçirildiği film listesine erişmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a026e-266">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="a026e-267">Örneğin, *Index. cshtml* görünümünde, kod kesin türü belirtilmiş `Model` nesnesi üzerinde `foreach` bir ifadesiyle filmlerle döngü yapılır:</span><span class="sxs-lookup"><span data-stu-id="a026e-267">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="a026e-268">`Model` nesne kesin olarak yazıldığı için (bir `IEnumerable<Movie>` nesnesi olarak), döngüdeki her öğe `Movie`olarak yazılır.</span><span class="sxs-lookup"><span data-stu-id="a026e-268">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="a026e-269">Diğer avantajların yanı sıra, kodu derleme zaman denetimini alacağınız anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="a026e-269">Among other benefits, this means that you get compile time checking of the code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a026e-270">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a026e-270">Additional resources</span></span>

* [<span data-ttu-id="a026e-271">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="a026e-271">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="a026e-272">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="a026e-272">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="a026e-273">Daha [önce bir görünüm ekleme](adding-view.md)
> [daha sonra SQL ile çalışma](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="a026e-273">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="a026e-274">Veri modeli sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="a026e-274">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a026e-275">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-275">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a026e-276"> > **sınıf** **eklemek** > *modeller* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a026e-276">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="a026e-277">Sınıf **filmi**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="a026e-277">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a026e-278">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-278">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a026e-279">*Movie.cs*adlı *modeller* klasörüne bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a026e-279">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="a026e-280">Film modelini dolandırın</span><span class="sxs-lookup"><span data-stu-id="a026e-280">Scaffold the movie model</span></span>

<span data-ttu-id="a026e-281">Bu bölümde, film modeli scafkatdır.</span><span class="sxs-lookup"><span data-stu-id="a026e-281">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="a026e-282">Diğer bir deyişle, scafkatlama aracı film modeli için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri için sayfalar üretir.</span><span class="sxs-lookup"><span data-stu-id="a026e-282">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a026e-283">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-283">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a026e-284">**Çözüm Gezgini**, *denetleyiciler* klasörüne sağ tıklayıp **yeni > yapı iskelesi > öğesi ekleyin**.</span><span class="sxs-lookup"><span data-stu-id="a026e-284">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Yukarıdaki adımın görünümü](adding-model/_static/add_controller21.png)

<span data-ttu-id="a026e-286">**Yapı Ekle** iletişim kutusunda, **Entity Framework > Ekle ' yi kullanarak views ile MVC denetleyicisi '** ni seçin.</span><span class="sxs-lookup"><span data-stu-id="a026e-286">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Yapı Iskelesi Ekle iletişim kutusu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="a026e-288">**Denetleyici Ekle** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="a026e-288">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="a026e-289">**Model sınıfı:** *Film (mvcmovie. modeller)*</span><span class="sxs-lookup"><span data-stu-id="a026e-289">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="a026e-290">**Veri bağlamı sınıfı:** **+** simgesini seçin ve varsayılan **Mvcmovie. modeller. MvcMovieContext** öğesini ekleyin</span><span class="sxs-lookup"><span data-stu-id="a026e-290">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Veri bağlamı Ekle](adding-model/_static/dc.png)

* <span data-ttu-id="a026e-292">**Görünümler:** Her seçeneğin varsayılan kısmını işaretli tut</span><span class="sxs-lookup"><span data-stu-id="a026e-292">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="a026e-293">**Denetleyici adı:** Varsayılan *MoviesController* tut</span><span class="sxs-lookup"><span data-stu-id="a026e-293">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="a026e-294">**Ekle** 'yi seçin</span><span class="sxs-lookup"><span data-stu-id="a026e-294">Select **Add**</span></span>

![Denetleyici Ekle iletişim kutusu](adding-model/_static/add_controller2.png)

<span data-ttu-id="a026e-296">Visual Studio şunları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a026e-296">Visual Studio creates:</span></span>

* <span data-ttu-id="a026e-297">Entity Framework Core [veritabanı bağlam sınıfı](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext. cs*)</span><span class="sxs-lookup"><span data-stu-id="a026e-297">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="a026e-298">Bir filmler denetleyicisi (*denetleyiciler/MoviesController. cs*)</span><span class="sxs-lookup"><span data-stu-id="a026e-298">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="a026e-299">Razor oluşturma, silme, ayrıntılar, düzenleme ve dizin sayfaları için dosyaları görüntüleme (*Görünümler/filmler/\*. cshtml*)</span><span class="sxs-lookup"><span data-stu-id="a026e-299">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="a026e-300">Veritabanı bağlamı ve [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemlerinin ve görünümlerinin otomatik olarak oluşturulması, *Yapı iskelesi*olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="a026e-300">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a026e-301">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a026e-301">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="a026e-302">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="a026e-302">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="a026e-303">Scafkatlama aracını yükler:</span><span class="sxs-lookup"><span data-stu-id="a026e-303">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="a026e-304">Linux 'ta, scafkatlama aracı yolunu dışarı aktarın:</span><span class="sxs-lookup"><span data-stu-id="a026e-304">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="a026e-305">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a026e-305">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a026e-306">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-306">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a026e-307">Proje dizininde bir komut penceresi açın ( *program.cs*, *Startup.cs*ve *. csproj* dosyalarını içeren dizin).</span><span class="sxs-lookup"><span data-stu-id="a026e-307">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="a026e-308">Scafkatlama aracını yükler:</span><span class="sxs-lookup"><span data-stu-id="a026e-308">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="a026e-309">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a026e-309">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="a026e-310">Uygulamayı çalıştırır ve **MVC filmi** bağlantısına tıklarsanız aşağıdakine benzer bir hata alırsınız:</span><span class="sxs-lookup"><span data-stu-id="a026e-310">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a026e-311">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-311">Visual Studio</span></span>](#tab/visual-studio)

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a026e-312">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-312">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

``` error
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)

```

---

<span data-ttu-id="a026e-313">Veritabanını oluşturmanız ve bunu yapmak için EF Core [geçişleri](xref:data/ef-mvc/migrations) özelliğini kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a026e-313">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="a026e-314">Geçişler veri modelinize uyan bir veritabanı oluşturmanıza ve veri modeliniz değiştiğinde veritabanı şemasını güncelleştirmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="a026e-314">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="a026e-315">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="a026e-315">Initial migration</span></span>

<span data-ttu-id="a026e-316">Bu bölümde, aşağıdaki görevler tamamlanır:</span><span class="sxs-lookup"><span data-stu-id="a026e-316">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="a026e-317">İlk geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a026e-317">Add an initial migration.</span></span>
* <span data-ttu-id="a026e-318">Veritabanını ilk geçişle güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="a026e-318">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a026e-319">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-319">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a026e-320">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** (PMC) öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="a026e-320">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![PMC menüsü](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="a026e-322">PMC 'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="a026e-322">In the PMC, enter the following commands:</span></span>

   ```PMC
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="a026e-323">`Add-Migration` komutu, ilk veritabanı şemasını oluşturmak için kod üretir.</span><span class="sxs-lookup"><span data-stu-id="a026e-323">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="a026e-324">Veritabanı şeması, `MvcMovieContext` sınıfında belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="a026e-324">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span> <span data-ttu-id="a026e-325">`Initial` bağımsız değişkeni geçiş adıdır.</span><span class="sxs-lookup"><span data-stu-id="a026e-325">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="a026e-326">Herhangi bir ad kullanılabilir, ancak kurala göre, geçişi açıklayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a026e-326">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="a026e-327">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="a026e-327">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="a026e-328">`Update-Database` komutu, veritabanını oluşturan *geçişler/{Time-damga} _ınitialcreate. cs* dosyasındaki `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="a026e-328">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a026e-329">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-329">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="a026e-330">`ef migrations add InitialCreate` komutu, ilk veritabanı şemasını oluşturmak için kod üretir.</span><span class="sxs-lookup"><span data-stu-id="a026e-330">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="a026e-331">Veritabanı şeması, `MvcMovieContext` sınıfında belirtilen modeli temel alır ( *Data/MvcMovieContext. cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="a026e-331">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="a026e-332">`InitialCreate` bağımsız değişkeni geçiş adıdır.</span><span class="sxs-lookup"><span data-stu-id="a026e-332">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="a026e-333">Herhangi bir ad kullanılabilir, ancak kurala göre, geçişi açıklayan bir ad seçilidir.</span><span class="sxs-lookup"><span data-stu-id="a026e-333">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="a026e-334">Bağımlılık ekleme ile kaydedilen bağlamı inceleyin</span><span class="sxs-lookup"><span data-stu-id="a026e-334">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="a026e-335">ASP.NET Core, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="a026e-335">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a026e-336">Hizmetler (EF Core DB bağlamı gibi) uygulama başlatma sırasında dı ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a026e-336">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="a026e-337">Bu hizmetleri gerektiren bileşenler (örneğin Razor Pages), bu hizmetleri Oluşturucu parametreleri aracılığıyla sağlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="a026e-337">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="a026e-338">Bir DB bağlam örneğini alan Oluşturucu kodu öğreticide daha sonra gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a026e-338">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a026e-339">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-339">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a026e-340">Scafkatlama aracı otomatik olarak bir DB bağlamı oluşturup dı kapsayıcısına kaydetti.</span><span class="sxs-lookup"><span data-stu-id="a026e-340">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="a026e-341">Aşağıdaki `Startup.ConfigureServices` yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="a026e-341">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a026e-342">Vurgulanan satır, scaffolder tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="a026e-342">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

<span data-ttu-id="a026e-343">`MvcMovieContext`, `Movie` modeli için EF Core işlevselliğini (oluşturma, okuma, güncelleştirme, silme, vb.) koordine eder.</span><span class="sxs-lookup"><span data-stu-id="a026e-343">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="a026e-344">Veri bağlamı (`MvcMovieContext`) [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)öğesinden türetilir.</span><span class="sxs-lookup"><span data-stu-id="a026e-344">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="a026e-345">Veri bağlamı, veri modeline hangi varlıkların ekleneceğini belirtir:</span><span class="sxs-lookup"><span data-stu-id="a026e-345">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Data/MvcMovieContext.cs)]

<span data-ttu-id="a026e-346">Önceki kod, varlık kümesi için bir [Dbset\<filmi >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a026e-346">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="a026e-347">Entity Framework terminolojisinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a026e-347">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="a026e-348">Bir varlık, tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a026e-348">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="a026e-349">Bağlantı dizesinin adı, [Dbcontextoptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesinde bir yöntem çağırarak bağlama geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a026e-349">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="a026e-350">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) , *appSettings. JSON* dosyasından bağlantı dizesini okur.</span><span class="sxs-lookup"><span data-stu-id="a026e-350">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a026e-351">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a026e-351">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="a026e-352">Bir DB bağlamı oluşturdunuz ve bunu DI kapsayıcısı ile kaydettiniz.</span><span class="sxs-lookup"><span data-stu-id="a026e-352">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="a026e-353">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="a026e-353">Test the app</span></span>

* <span data-ttu-id="a026e-354">Uygulamayı çalıştırın ve tarayıcıdaki URL 'ye `/Movies` ekleyin (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="a026e-354">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="a026e-355">Aşağıdakine benzer bir veritabanı özel durumu alırsanız:</span><span class="sxs-lookup"><span data-stu-id="a026e-355">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="a026e-356">[Geçişler adımını](#pmc)kaçırdınız.</span><span class="sxs-lookup"><span data-stu-id="a026e-356">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="a026e-357">**Oluştur** bağlantısını test edin.</span><span class="sxs-lookup"><span data-stu-id="a026e-357">Test the **Create** link.</span></span> <span data-ttu-id="a026e-358">Veri girin ve gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a026e-358">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="a026e-359">`Price` alanına ondalık virgül giremeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a026e-359">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="a026e-360">Ondalık bir nokta ve ABD Ingilizcesi olmayan tarih biçimleri için virgül (",") kullanan Ingilizce olmayan yerel ayarlarda [jQuery doğrulamasını](https://jqueryvalidation.org/) desteklemek için, uygulamanın Genelleştirilmiş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a026e-360">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="a026e-361">Genelleştirme yönergeleri için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)bakın.</span><span class="sxs-lookup"><span data-stu-id="a026e-361">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="a026e-362">**Düzenleme**, **Ayrıntılar**ve **silme** bağlantılarını test edin.</span><span class="sxs-lookup"><span data-stu-id="a026e-362">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="a026e-363">`Startup` sınıfını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a026e-363">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="a026e-364">Önceki vurgulanan kod, [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına eklenen film veritabanı bağlamını gösterir:</span><span class="sxs-lookup"><span data-stu-id="a026e-364">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="a026e-365">`services.AddDbContext<MvcMovieContext>(options =>` kullanılacak veritabanını ve bağlantı dizesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a026e-365">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="a026e-366">`=>` [lambda operatörü](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="a026e-366">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="a026e-367">*Controllers/MoviesController. cs* dosyasını açın ve oluşturucuyu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a026e-367">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="a026e-368">Oluşturucu, veritabanı bağlamını (`MvcMovieContext`) denetleyiciye eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="a026e-368">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="a026e-369">Veritabanı bağlamı, denetleyicideki [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) yöntemlerinde her birinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a026e-369">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="a026e-370">Türü kesin belirlenmiş modeller ve @model anahtar sözcüğü</span><span class="sxs-lookup"><span data-stu-id="a026e-370">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="a026e-371">Bu öğreticide daha önce, bir denetleyicinin `ViewData` sözlüğünü kullanarak bir görünüme nasıl veri veya nesne geçirekullanabileceğinizi gördünüz.</span><span class="sxs-lookup"><span data-stu-id="a026e-371">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="a026e-372">`ViewData` sözlüğü bir görünüme bilgi geçirmek için uygun, geç bağlanan bir yol sağlayan dinamik bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="a026e-372">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="a026e-373">MVC Ayrıca, kesin olarak belirlenmiş model nesnelerini bir görünüme geçirmeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="a026e-373">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="a026e-374">Bu kesin türü belirtilmiş yaklaşım, kodunuzun daha iyi derleme zaman denetimini sunar.</span><span class="sxs-lookup"><span data-stu-id="a026e-374">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="a026e-375">Yapı iskelesi mekanizması, yöntem ve görünümleri oluştururken `MoviesController` sınıfı ve görünümleriyle bu yaklaşımı (türü kesin belirlenmiş bir model geçirme) kullanır.</span><span class="sxs-lookup"><span data-stu-id="a026e-375">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="a026e-376">*Controllers/MoviesController. cs* dosyasında oluşturulan `Details` yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a026e-376">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="a026e-377">`id` parametresi genellikle rota verileri olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a026e-377">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="a026e-378">Örneğin `https://localhost:5001/movies/details/1` kümeler:</span><span class="sxs-lookup"><span data-stu-id="a026e-378">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="a026e-379">`movies` denetleyicisine denetleyici (ilk URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="a026e-379">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="a026e-380">`details` eylemi (ikinci URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="a026e-380">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="a026e-381">Kimliği 1 ' e (son URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="a026e-381">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="a026e-382">Aşağıdaki gibi bir sorgu dizesiyle `id` de geçirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a026e-382">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="a026e-383">KIMLIK değeri sağlanmazsa `id` parametresi [null yapılabilir bir tür](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="a026e-383">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="a026e-384">Bir [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) , rota verileriyle veya sorgu dizesi değeriyle eşleşen film varlıklarını seçmek üzere `FirstOrDefaultAsync` ' A geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a026e-384">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="a026e-385">Bir film bulunursa, `Movie` modelinin bir örneği `Details` görünümüne geçirilir:</span><span class="sxs-lookup"><span data-stu-id="a026e-385">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="a026e-386">*Görünümler/filmler/ayrıntılar. cshtml* dosyasının içeriğini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="a026e-386">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="a026e-387">Görünüm dosyasının üst kısmına bir `@model` ifadesini ekleyerek, görünümün beklediği nesne türünü belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a026e-387">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="a026e-388">Film denetleyicisini oluştururken, *Ayrıntılar. cshtml* dosyasının en üstüne aşağıdaki `@model` deyimleri otomatik olarak eklenmiştir:</span><span class="sxs-lookup"><span data-stu-id="a026e-388">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="a026e-389">Bu `@model` yönergesi, kesin olarak belirlenmiş bir `Model` nesnesi kullanarak denetleyicinin görünüme geçirildiği filme erişmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a026e-389">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="a026e-390">Örneğin, *details. cshtml* görünümünde, kod her bir film alanını `DisplayNameFor` `DisplayFor` ve HTML yardımcılarını türü kesin belirlenmiş `Model` nesnesiyle geçirir.</span><span class="sxs-lookup"><span data-stu-id="a026e-390">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="a026e-391">`Create` ve `Edit` yöntemleri ve görünümleri bir `Movie` model nesnesi de iletir.</span><span class="sxs-lookup"><span data-stu-id="a026e-391">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="a026e-392">*Dizin. cshtml* görünümünü ve film denetleyicisindeki `Index` yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="a026e-392">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="a026e-393">`List`, `View` yöntemini çağırdığında kodun bir nesne nasıl oluşturduğunu fark edin.</span><span class="sxs-lookup"><span data-stu-id="a026e-393">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="a026e-394">Kod, `Index` eylem yönteminden bu `Movies` listesini görünüme geçirir:</span><span class="sxs-lookup"><span data-stu-id="a026e-394">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="a026e-395">Film denetleyicisini oluştururken, yapı iskelesi *Index. cshtml* dosyasının en üstüne aşağıdaki `@model` ifadesini otomatik olarak dahil edin:</span><span class="sxs-lookup"><span data-stu-id="a026e-395">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="a026e-396">`@model` yönergesi, kesin olarak belirlenmiş bir `Model` nesnesi kullanarak, denetleyicinin görünüme geçirildiği film listesine erişmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a026e-396">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="a026e-397">Örneğin, *Index. cshtml* görünümünde, kod kesin türü belirtilmiş `Model` nesnesi üzerinde `foreach` bir ifadesiyle filmlerle döngü yapılır:</span><span class="sxs-lookup"><span data-stu-id="a026e-397">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="a026e-398">`Model` nesne kesin olarak yazıldığı için (bir `IEnumerable<Movie>` nesnesi olarak), döngüdeki her öğe `Movie`olarak yazılır.</span><span class="sxs-lookup"><span data-stu-id="a026e-398">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="a026e-399">Diğer avantajların yanı sıra, kodun derleme zaman denetimini alacağınız anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="a026e-399">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a026e-400">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a026e-400">Additional resources</span></span>

* [<span data-ttu-id="a026e-401">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="a026e-401">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="a026e-402">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="a026e-402">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="a026e-403">Daha [önce bir görünüm ekleme](adding-view.md)
> [daha sonra SQL ile çalışma](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="a026e-403">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end
