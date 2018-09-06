---
title: Bir ASP.NET Core Razor sayfaları uygulama için model ekleme
author: rick-anderson
description: Entity Framework Core (EF Core) kullanarak bir veritabanında filmler yönetmek için sınıflar ekleme keşfedin.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: fb3a287725fa68ff9feb9935d7e6c5c2b8316517
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893126"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="a2fd0-103">Bir ASP.NET Core Razor sayfaları uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="a2fd0-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="a2fd0-104">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="a2fd0-104">Add a data model</span></span>

<span data-ttu-id="a2fd0-105">Çözüm Gezgini'nde sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="a2fd0-106">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-106">Name the folder *Models*.</span></span>

<span data-ttu-id="a2fd0-107">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-107">Right click the *Models* folder.</span></span> <span data-ttu-id="a2fd0-108">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="a2fd0-109">Sınıf adı **film** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="a2fd0-110">Öğesinin içeriğini değiştirin `Movie` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-110">Replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="a2fd0-111">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="a2fd0-111">Scaffold the movie model</span></span>

<span data-ttu-id="a2fd0-112">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-112">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="a2fd0-113">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-113">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="a2fd0-114">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-114">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="a2fd0-115">İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları* klasör > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-115">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="a2fd0-116">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="a2fd0-116">Name the folder *Movies*</span></span>

<span data-ttu-id="a2fd0-117">İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları/filmler* klasör > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-117">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="a2fd0-119">İçinde **İskele Ekle** iletişim kutusunda **Entity Framework (CRUD) kullanarak Razor sayfaları** > **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-119">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="a2fd0-121">Tamamlamak **ekleme Razor sayfaları (CRUD) Entity Framework kullanarak** iletişim:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-121">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="a2fd0-122">İçinde **Model sınıfı** seçin, açılan menü **film (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-122">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="a2fd0-123">İçinde **veri bağlamı sınıfının** satır, select **+** (artı) oturum açın ve oluşturulan adı kabul **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-123">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="a2fd0-124">İçinde **veri bağlamı sınıfının** seçin, açılan menü **RazorPagesMovie.Models.RazorPagesMovieContext**</span><span class="sxs-lookup"><span data-stu-id="a2fd0-124">In the **Data context class** drop down, select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="a2fd0-125">Seçin **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-125">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

<span data-ttu-id="a2fd0-127">İskele işlem oluşturulur ve aşağıdaki dosya değişti:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-127">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="a2fd0-128">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="a2fd0-128">Files created</span></span>

* <span data-ttu-id="a2fd0-129">*Sayfa/filmler* oluşturma, silme, Ayrıntılar, düzenleme, dizin.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-129">*Pages/Movies* Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="a2fd0-130">Bu sayfalar, sonraki öğreticide açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-130">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="a2fd0-131">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="a2fd0-131">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="files-updates"></a><span data-ttu-id="a2fd0-132">Güncelleştirme dosyaları</span><span class="sxs-lookup"><span data-stu-id="a2fd0-132">Files updates</span></span>

* <span data-ttu-id="a2fd0-133">*Startup.cs*: Bu dosyada yapılan değişiklikler sonraki bölümde ayrıntılı.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-133">*Startup.cs*: Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="a2fd0-134">*appSettings.JSON*: yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklenir.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-134">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="a2fd0-135">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="a2fd0-135">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="a2fd0-136">ASP.NET Core ile oluşturulmuş [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a2fd0-136">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a2fd0-137">Hizmetler (örneğin, EF Core DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-137">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="a2fd0-138">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-138">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="a2fd0-139">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-139">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="a2fd0-140">Yapı iskelesi aracı otomatik olarak oluşturulmuş bir veritabanı bağlamını ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-140">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="a2fd0-141">İnceleme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-141">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a2fd0-142">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-142">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="a2fd0-143">Verilen veri modeli için EF Core işlevselliği koordine eden ana DB bağlamı sınıfının sınıftır.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-143">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="a2fd0-144">Veri bağlamı türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="a2fd0-144">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="a2fd0-145">Veri bağlamı, hangi varlıkları veri modelinde yer alan belirtir.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-145">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="a2fd0-146">Bu projede adlı sınıfı `RazorPagesMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-146">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="a2fd0-147">Yukarıdaki kod oluşturur bir [olan DB\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-147">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="a2fd0-148">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-148">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="a2fd0-149">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-149">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="a2fd0-150">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-150">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="a2fd0-151">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-151">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="a2fd0-152">İlk geçiş gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="a2fd0-152">Perform initial migration</span></span>

<span data-ttu-id="a2fd0-153">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanın:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-153">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="a2fd0-154">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-154">Add an initial migration.</span></span>
* <span data-ttu-id="a2fd0-155">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-155">Update the database with the initial migration.</span></span>

<span data-ttu-id="a2fd0-156">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-156">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="a2fd0-158">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-158">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="a2fd0-159">Alternatif olarak, proje klasöründen aşağıdaki .NET Core CLI komutları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-159">Alternatively, the following .NET Core CLI commands can be used from the project folder:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="a2fd0-160">Aşağıdaki uyarı iletisini yoksay, düzeltme, bir sonraki öğreticide:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-160">Ignore the following warning message, you fix that in a a later tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="a2fd0-161">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-161">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="a2fd0-162">Belirtilen model şeması dayanır `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="a2fd0-162">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="a2fd0-163">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-163">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="a2fd0-164">Herhangi bir adı kullanabilirsiniz, ancak bu kurala göre geçiş tanımlayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-164">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="a2fd0-165">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-165">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="a2fd0-166">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-166">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="a2fd0-167">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-167">If you get the error:</span></span>

<span data-ttu-id="a2fd0-168">Hata: "GUID RazorPagesMovieContext" oturum açma tarafından istenen veritabanı açılamıyor.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-168">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="a2fd0-169">Oturum açma başarısız.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-169">The login failed.</span></span>
<span data-ttu-id="a2fd0-170">Oturum açma kullanıcı 'User-name' için başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-170">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="a2fd0-171">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="a2fd0-171">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="a2fd0-172">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="a2fd0-172">Add a data model</span></span>

<span data-ttu-id="a2fd0-173">Çözüm Gezgini'nde sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-173">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="a2fd0-174">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-174">Name the folder *Models*.</span></span>

<span data-ttu-id="a2fd0-175">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-175">Right click the *Models* folder.</span></span> <span data-ttu-id="a2fd0-176">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-176">Select **Add** > **Class**.</span></span> <span data-ttu-id="a2fd0-177">Sınıf adı **film** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-177">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="a2fd0-178">Bir veritabanı bağlantı dizesi Ekle</span><span class="sxs-lookup"><span data-stu-id="a2fd0-178">Add a database connection string</span></span>

<span data-ttu-id="a2fd0-179">Bir bağlantı dizesi Ekle *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-179">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="a2fd0-180">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="a2fd0-180">Register the database context</span></span>

<span data-ttu-id="a2fd0-181">Veritabanı bağlamı ile kaydetme [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında [başlangıç sınıfının Createservicereplicalisteners() yöntemi](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="a2fd0-181">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="a2fd0-182">Herhangi bir hata yoksa doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-182">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="a2fd0-183">Yapı iskelesi araçları ekleyin ve ilk geçiş gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="a2fd0-183">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="a2fd0-184">Bu bölümde, Paket Yöneticisi Konsolu (PMC'yi) için kullanın:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-184">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="a2fd0-185">Visual Studio web kodu oluşturma paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-185">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="a2fd0-186">Bu paket, yapı iskelesi altyapısı çalıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-186">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="a2fd0-187">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-187">Add an initial migration.</span></span>
* <span data-ttu-id="a2fd0-188">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-188">Update the database with the initial migration.</span></span>

<span data-ttu-id="a2fd0-189">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-189">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="a2fd0-191">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-191">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="a2fd0-192">Alternatif olarak, aşağıdaki .NET Core CLI komutları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-192">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="a2fd0-193">Aşağıdaki ileti yoksay:</span><span class="sxs-lookup"><span data-stu-id="a2fd0-193">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="a2fd0-194">Sonraki öğreticide bunu düzeltelim.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-194">You fix that in the next tutorial.</span></span>

<span data-ttu-id="a2fd0-195">`Install-Package` Komut yapı iskelesi altyapısı çalıştırmak için gerekli araçları yükler.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-195">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="a2fd0-196">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-196">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="a2fd0-197">Belirtilen model şeması dayanır `DbContext` (içinde *Models/MovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="a2fd0-197">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="a2fd0-198">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-198">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="a2fd0-199">Herhangi bir adı kullanabilirsiniz, ancak bu kurala göre geçiş tanımlayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-199">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="a2fd0-200">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-200">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="a2fd0-201">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-201">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="a2fd0-202">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="a2fd0-202">Test the app</span></span>

* <span data-ttu-id="a2fd0-203">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="a2fd0-203">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="a2fd0-204">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-204">Test the **Create** link.</span></span>

  ![sayfası oluşturma](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="a2fd0-206">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-206">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="a2fd0-207">SQL özel durum alırsanız geçişlerini çalıştırarak ve veritabanı güncelleştirme doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-207">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="a2fd0-208">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="a2fd0-208">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a2fd0-209">[Önceki: Başlama](xref:tutorials/razor-pages/razor-pages-start)
> [sonraki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="a2fd0-209">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
