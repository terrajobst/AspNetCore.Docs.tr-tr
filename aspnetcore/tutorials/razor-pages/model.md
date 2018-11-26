---
title: Bir ASP.NET Core Razor sayfaları uygulama için model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında filmler yönetmek için sınıflar ekleme keşfedin.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: c4b23f75da298e4ee804f649219c2ce466b6d6ea
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299449"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="17bd4-103">Bir ASP.NET Core Razor sayfaları uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="17bd4-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="17bd4-104">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="17bd4-104">Add a data model</span></span>

<span data-ttu-id="17bd4-105">Çözüm Gezgini'nde sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="17bd4-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="17bd4-106">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="17bd4-106">Name the folder *Models*.</span></span>

<span data-ttu-id="17bd4-107">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="17bd4-107">Right click the *Models* folder.</span></span> <span data-ttu-id="17bd4-108">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="17bd4-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="17bd4-109">Sınıf adı **film** ve içeriklerini `Movie` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="17bd4-109">Name the class **Movie** and replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="17bd4-110">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="17bd4-110">Scaffold the movie model</span></span>

<span data-ttu-id="17bd4-111">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="17bd4-111">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="17bd4-112">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="17bd4-112">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="17bd4-113">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="17bd4-113">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="17bd4-114">İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları* klasör > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="17bd4-114">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="17bd4-115">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="17bd4-115">Name the folder *Movies*</span></span>

<span data-ttu-id="17bd4-116">İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları/filmler* klasör > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="17bd4-116">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="17bd4-118">İçinde **İskele Ekle** iletişim kutusunda **Entity Framework (CRUD) kullanarak Razor sayfaları** > **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="17bd4-118">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="17bd4-120">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="17bd4-120">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="17bd4-121">İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="17bd4-121">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="17bd4-122">İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan adı kabul **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="17bd4-122">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="17bd4-123">Seçin **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="17bd4-123">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

<span data-ttu-id="17bd4-125">İskele işlem oluşturur ve aşağıdaki dosyaları güncelleştirir:</span><span class="sxs-lookup"><span data-stu-id="17bd4-125">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="17bd4-126">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="17bd4-126">Files created</span></span>

* <span data-ttu-id="17bd4-127">*Sayfa/filmler*: oluşturma, silme, Ayrıntılar, düzenleme, dizin.</span><span class="sxs-lookup"><span data-stu-id="17bd4-127">*Pages/Movies*: Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="17bd4-128">Bu sayfalar, sonraki öğreticide açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="17bd4-128">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="17bd4-129">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="17bd4-129">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="17bd4-130">Dosya güncelleştirildi</span><span class="sxs-lookup"><span data-stu-id="17bd4-130">File updated</span></span>

* <span data-ttu-id="17bd4-131">*Startup.cs*: Bu dosyada yapılan değişiklikler sonraki bölümde ayrıntılı.</span><span class="sxs-lookup"><span data-stu-id="17bd4-131">*Startup.cs*: Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="17bd4-132">*appSettings.JSON*: yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklenir.</span><span class="sxs-lookup"><span data-stu-id="17bd4-132">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="17bd4-133">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="17bd4-133">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="17bd4-134">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="17bd4-134">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="17bd4-135">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="17bd4-135">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="17bd4-136">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="17bd4-136">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="17bd4-137">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="17bd4-137">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="17bd4-138">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="17bd4-138">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="17bd4-139">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="17bd4-139">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="17bd4-140">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="17bd4-140">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="17bd4-141">Verilen veri modeli için EF Core işlevselliği koordine eden ana DB bağlamı sınıfının sınıftır.</span><span class="sxs-lookup"><span data-stu-id="17bd4-141">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="17bd4-142">Veri bağlamı türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="17bd4-142">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="17bd4-143">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="17bd4-143">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="17bd4-144">Bu projede adlı sınıfı `RazorPagesMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="17bd4-144">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="17bd4-145">Yukarıdaki kod oluşturur bir [olan DB\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="17bd4-145">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="17bd4-146">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="17bd4-146">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="17bd4-147">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="17bd4-147">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="17bd4-148">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="17bd4-148">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="17bd4-149">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="17bd4-149">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="17bd4-150">İlk geçiş gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="17bd4-150">Perform initial migration</span></span>

<span data-ttu-id="17bd4-151">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanın:</span><span class="sxs-lookup"><span data-stu-id="17bd4-151">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="17bd4-152">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="17bd4-152">Add an initial migration.</span></span>
* <span data-ttu-id="17bd4-153">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="17bd4-153">Update the database with the initial migration.</span></span>

<span data-ttu-id="17bd4-154">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="17bd4-154">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="17bd4-156">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="17bd4-156">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="17bd4-157">Alternatif olarak, proje klasöründen aşağıdaki .NET Core CLI komutları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="17bd4-157">Alternatively, the following .NET Core CLI commands can be used from the project folder:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="17bd4-158">Bir sonraki öğreticide düzeltme aşağıdaki uyarı iletisini yoksay:</span><span class="sxs-lookup"><span data-stu-id="17bd4-158">Ignore the following warning message, which you fix in a later tutorial:</span></span>

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.
```

<span data-ttu-id="17bd4-159">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="17bd4-159">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="17bd4-160">Belirtilen model şeması dayanır `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="17bd4-160">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="17bd4-161">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="17bd4-161">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="17bd4-162">Herhangi bir adı kullanabilirsiniz, ancak bu kurala göre geçiş tanımlayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="17bd4-162">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="17bd4-163">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="17bd4-163">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="17bd4-164">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="17bd4-164">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="17bd4-165">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="17bd4-165">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="17bd4-166">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="17bd4-166">You missed the [migrations step](#pmc).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="17bd4-167">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="17bd4-167">Add a data model</span></span>

<span data-ttu-id="17bd4-168">Çözüm Gezgini'nde sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="17bd4-168">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="17bd4-169">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="17bd4-169">Name the folder *Models*.</span></span>

<span data-ttu-id="17bd4-170">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="17bd4-170">Right click the *Models* folder.</span></span> <span data-ttu-id="17bd4-171">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="17bd4-171">Select **Add** > **Class**.</span></span> <span data-ttu-id="17bd4-172">Sınıf adı **film** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="17bd4-172">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="17bd4-173">Bir veritabanı bağlantı dizesi Ekle</span><span class="sxs-lookup"><span data-stu-id="17bd4-173">Add a database connection string</span></span>

<span data-ttu-id="17bd4-174">Bir bağlantı dizesi Ekle *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="17bd4-174">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="17bd4-175">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="17bd4-175">Register the database context</span></span>

<span data-ttu-id="17bd4-176">Veritabanı bağlamı ile kaydetme [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında [başlangıç sınıfının Createservicereplicalisteners() yöntemi](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="17bd4-176">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="17bd4-177">Herhangi bir hata yoksa doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="17bd4-177">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="17bd4-178">Yapı iskelesi araçları ekleyin ve ilk geçiş gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="17bd4-178">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="17bd4-179">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanın:</span><span class="sxs-lookup"><span data-stu-id="17bd4-179">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="17bd4-180">Visual Studio web kodu oluşturma paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="17bd4-180">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="17bd4-181">Bu paket, yapı iskelesi altyapısı çalıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="17bd4-181">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="17bd4-182">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="17bd4-182">Add an initial migration.</span></span>
* <span data-ttu-id="17bd4-183">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="17bd4-183">Update the database with the initial migration.</span></span>

<span data-ttu-id="17bd4-184">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="17bd4-184">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="17bd4-186">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="17bd4-186">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="17bd4-187">Alternatif olarak, aşağıdaki .NET Core CLI komutları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="17bd4-187">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="17bd4-188">Aşağıdaki ileti yoksay:</span><span class="sxs-lookup"><span data-stu-id="17bd4-188">Ignore the following message:</span></span>

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'
```

<span data-ttu-id="17bd4-189">Sonraki öğreticide bunu düzeltelim.</span><span class="sxs-lookup"><span data-stu-id="17bd4-189">You fix that in the next tutorial.</span></span>

<span data-ttu-id="17bd4-190">`Install-Package` Komut yapı iskelesi altyapısı çalıştırmak için gerekli araçları yükler.</span><span class="sxs-lookup"><span data-stu-id="17bd4-190">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="17bd4-191">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="17bd4-191">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="17bd4-192">Belirtilen model şeması dayanır `DbContext` (içinde *Models/MovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="17bd4-192">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="17bd4-193">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="17bd4-193">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="17bd4-194">Herhangi bir adı kullanabilirsiniz, ancak bu kurala göre geçiş tanımlayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="17bd4-194">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="17bd4-195">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="17bd4-195">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="17bd4-196">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="17bd4-196">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="17bd4-197">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="17bd4-197">Test the app</span></span>

* <span data-ttu-id="17bd4-198">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="17bd4-198">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="17bd4-199">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="17bd4-199">Test the **Create** link.</span></span>

  ![sayfası oluşturma](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="17bd4-201">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="17bd4-201">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="17bd4-202">SQL özel durum alırsanız geçişlerini çalıştırarak ve veritabanı güncelleştirme doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="17bd4-202">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="17bd4-203">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="17bd4-203">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="17bd4-204">[Önceki: Başlama](xref:tutorials/razor-pages/razor-pages-start)
> [sonraki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="17bd4-204">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
