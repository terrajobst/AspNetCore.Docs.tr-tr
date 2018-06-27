---
title: ASP.NET Core bir Razor sayfalarının uygulama için model ekleme
author: rick-anderson
description: Entity Framework Çekirdek (EF çekirdek) kullanarak bir veritabanındaki filmler yönetmek için sınıfları ekleme bulur.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: ed8faf8b3049adc7bcc7953d63ad805b0a836bd9
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961181"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="1eed2-103">ASP.NET Core bir Razor sayfalarının uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="1eed2-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="1eed2-104">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="1eed2-104">Add a data model</span></span>

<span data-ttu-id="1eed2-105">Çözüm Gezgini'nde sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="1eed2-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="1eed2-106">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="1eed2-106">Name the folder *Models*.</span></span>

<span data-ttu-id="1eed2-107">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="1eed2-107">Right click the *Models* folder.</span></span> <span data-ttu-id="1eed2-108">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="1eed2-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="1eed2-109">Sınıf adını **film** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="1eed2-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="1eed2-110">Değiştir `Movie` aşağıdaki kodla sınıfı:</span><span class="sxs-lookup"><span data-stu-id="1eed2-110">Replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="1eed2-111">İskele film modeli</span><span class="sxs-lookup"><span data-stu-id="1eed2-111">Scaffold the movie model</span></span>

<span data-ttu-id="1eed2-112">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="1eed2-112">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="1eed2-113">Diğer bir deyişle, yapı iskelesi aracı film modeli için oluşturma, okuma, güncelleştirme ve Sil (CRUD) işlemleri için sayfaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1eed2-113">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="1eed2-114">Oluşturma bir *sayfaları/filmler* klasörü:</span><span class="sxs-lookup"><span data-stu-id="1eed2-114">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="1eed2-115">İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları* klasörü > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="1eed2-115">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="1eed2-116">Klasör adı *filmler*</span><span class="sxs-lookup"><span data-stu-id="1eed2-116">Name the folder *Movies*</span></span>

<span data-ttu-id="1eed2-117">İçinde **Çözüm Gezgini**, sağ tıklayın *sayfaları/filmler* klasörü > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="1eed2-117">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/sca.png)

<span data-ttu-id="1eed2-119">İçinde **İskele Ekle** iletişim kutusunda **Razor Entity Framework (CRUD) kullanarak sayfaları** > **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="1eed2-119">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/add_scaffold.png)

<span data-ttu-id="1eed2-121">Tamamlamak **Razor Entity Framework (CRUD) kullanarak Sayfa Ekle** iletişim:</span><span class="sxs-lookup"><span data-stu-id="1eed2-121">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="1eed2-122">İçinde **Model sınıfı** seçin, açılan **film (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="1eed2-122">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="1eed2-123">İçinde **veri bağlamı sınıfı** satır, select **+** (artı) oturum açın ve oluşturulan adı kabul **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="1eed2-123">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="1eed2-124">İçinde **veri bağlamı sınıfı** seçin, açılan **RazorPagesMovie.Models.RazorPagesMovieContext**</span><span class="sxs-lookup"><span data-stu-id="1eed2-124">In the **Data context class** drop down, select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="1eed2-125">Seçin **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="1eed2-125">Select **Add**.</span></span>

![Önceki yönergeleri görüntüden.](model/_static/arp.png)

<span data-ttu-id="1eed2-127">İskele işlemi oluşturulan ve aşağıdaki dosyaları değişti:</span><span class="sxs-lookup"><span data-stu-id="1eed2-127">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="1eed2-128">Oluşturulan dosyalar</span><span class="sxs-lookup"><span data-stu-id="1eed2-128">Files created</span></span>

* <span data-ttu-id="1eed2-129">*Sayfa/filmler* oluşturma, silme, ayrıntı, düzenleme, dizin.</span><span class="sxs-lookup"><span data-stu-id="1eed2-129">*Pages/Movies* Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="1eed2-130">Bu sayfa, sonraki öğreticide açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="1eed2-130">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="1eed2-131">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="1eed2-131">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="files-updates"></a><span data-ttu-id="1eed2-132">Güncelleştirme dosyaları</span><span class="sxs-lookup"><span data-stu-id="1eed2-132">Files updates</span></span>

* <span data-ttu-id="1eed2-133">*Haline*: Bu dosyada yapılan değişiklikler ayrıntılı bir sonraki bölüm.</span><span class="sxs-lookup"><span data-stu-id="1eed2-133">*Startup.cs*: Changes to this file in are detailed the next section.</span></span>
* <span data-ttu-id="1eed2-134">*appSettings.JSON*: yerel bir veritabanına bağlanmak için kullanılan bağlantı dizesi eklendi.</span><span class="sxs-lookup"><span data-stu-id="1eed2-134">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="1eed2-135">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="1eed2-135">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="1eed2-136">ASP.NET Core ile oluşturulan [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1eed2-136">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="1eed2-137">Hizmetleri (örneğin, EF çekirdek DB bağlamı), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="1eed2-137">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="1eed2-138">Bu Hizmetleri (örneğin, Razor sayfalarının) gerektiren bileşenler bu hizmetlere Oluşturucu parametreleri yoluyla sağlanır.</span><span class="sxs-lookup"><span data-stu-id="1eed2-138">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="1eed2-139">Bir DB bağlamı örneği alır Oluşturucusu kodu daha sonra öğreticide gösterilir.</span><span class="sxs-lookup"><span data-stu-id="1eed2-139">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="1eed2-140">Yapı iskelesi Aracı'nı otomatik olarak bir veritabanı bağlamını oluşturulur ve bağımlılık ekleme kapsayıcısını ile kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="1eed2-140">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="1eed2-141">İncelemek `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1eed2-141">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="1eed2-142">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="1eed2-142">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="1eed2-143">Verilen veri modeli için EF temel işlevleri koordinatları ana sınıfı DB bağlamı sınıftır.</span><span class="sxs-lookup"><span data-stu-id="1eed2-143">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="1eed2-144">Veri bağlamı türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="1eed2-144">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="1eed2-145">Veri bağlamı hangi varlıkların veri modelinde dahil edildiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="1eed2-145">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="1eed2-146">Bu projede adlı sınıfı `RazorPagesMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="1eed2-146">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="1eed2-147">Önceki kod oluşturur bir [DbSet\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği için varlık kümesi.</span><span class="sxs-lookup"><span data-stu-id="1eed2-147">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="1eed2-148">Entity Framework terminolojisinde bir varlık kümesine genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="1eed2-148">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="1eed2-149">Bir varlık tablosunda bir satırı karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="1eed2-149">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="1eed2-150">Bağlantı dizesinin adını bağlamına üzerinde bir yöntemini çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesnesi.</span><span class="sxs-lookup"><span data-stu-id="1eed2-150">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="1eed2-151">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="1eed2-151">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="1eed2-152">İlk geçiş gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="1eed2-152">Perform initial migration</span></span>

<span data-ttu-id="1eed2-153">Bu bölümde, Paket Yöneticisi Konsolu (PMC) için kullanın:</span><span class="sxs-lookup"><span data-stu-id="1eed2-153">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="1eed2-154">İlk geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1eed2-154">Add an initial migration.</span></span>
* <span data-ttu-id="1eed2-155">Veritabanı ile ilk geçiş güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="1eed2-155">Update the database with the initial migration.</span></span>

<span data-ttu-id="1eed2-156">Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="1eed2-156">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="1eed2-158">PMC aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="1eed2-158">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="1eed2-159">Alternatif olarak, aşağıdaki .NET Core CLI komutları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="1eed2-159">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="1eed2-160">Aşağıdaki uyarı iletisini yoksaymak, sonraki öğreticide düzeltin:</span><span class="sxs-lookup"><span data-stu-id="1eed2-160">Ignore the following warning message, you fix that in the next tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="1eed2-161">`Add-Migration` Komutu ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1eed2-161">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="1eed2-162">Belirtilen model şeması dayalı `RazorPagesMovieContext` (içinde *Data/RazorPagesMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="1eed2-162">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="1eed2-163">`Initial` Bağımsız değişkeni geçişler adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1eed2-163">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="1eed2-164">Herhangi bir ad kullanabilirsiniz, ancak kurala göre geçiş açıklayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="1eed2-164">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="1eed2-165">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1eed2-165">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="1eed2-166">`Update-Database` Komutu çalıştırır `Up` yönteminde *geçişleri / {zaman damgası} _InitialCreate.cs* dosyası bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1eed2-166">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="1eed2-167">Hatayı alırsanız:</span><span class="sxs-lookup"><span data-stu-id="1eed2-167">If you get the error:</span></span>

<span data-ttu-id="1eed2-168">SqlException: "GUID RazorPagesMovieContext" oturum açma tarafından istenen veritabanı açılamıyor.</span><span class="sxs-lookup"><span data-stu-id="1eed2-168">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="1eed2-169">Oturum açma başarısız.</span><span class="sxs-lookup"><span data-stu-id="1eed2-169">The login failed.</span></span>
<span data-ttu-id="1eed2-170">Oturum açma kullanıcısı 'User name' için başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="1eed2-170">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="1eed2-171">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="1eed2-171">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="1eed2-172">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="1eed2-172">Add a data model</span></span>

<span data-ttu-id="1eed2-173">Çözüm Gezgini'nde sağ **RazorPagesMovie** Proje > **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="1eed2-173">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="1eed2-174">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="1eed2-174">Name the folder *Models*.</span></span>

<span data-ttu-id="1eed2-175">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="1eed2-175">Right click the *Models* folder.</span></span> <span data-ttu-id="1eed2-176">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="1eed2-176">Select **Add** > **Class**.</span></span> <span data-ttu-id="1eed2-177">Sınıf adını **film** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="1eed2-177">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="1eed2-178">Bir veritabanı bağlantı dizesi Ekle</span><span class="sxs-lookup"><span data-stu-id="1eed2-178">Add a database connection string</span></span>

<span data-ttu-id="1eed2-179">Bir bağlantı dizesi eklemek *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="1eed2-179">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="1eed2-180">Veritabanı bağlamı kaydetme</span><span class="sxs-lookup"><span data-stu-id="1eed2-180">Register the database context</span></span>

<span data-ttu-id="1eed2-181">Veritabanı bağlamı ile kayıt [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında [başlangıç sınıfının ConfigureServices yöntemi](xref:fundamentals/startup#the-startup-class) (*haline*):</span><span class="sxs-lookup"><span data-stu-id="1eed2-181">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="1eed2-182">Herhangi bir hata yoksa doğrulamak için projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1eed2-182">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="1eed2-183">İskele araç ekleyin ve başlangıç geçiş gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="1eed2-183">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="1eed2-184">Bu bölümde, Paket Yöneticisi Konsolu (PMC) için kullanın:</span><span class="sxs-lookup"><span data-stu-id="1eed2-184">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="1eed2-185">Visual Studio web kod oluşturma paketi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1eed2-185">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="1eed2-186">Bu paket, yapı iskelesi altyapısı çalıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="1eed2-186">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="1eed2-187">İlk geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1eed2-187">Add an initial migration.</span></span>
* <span data-ttu-id="1eed2-188">Veritabanı ile ilk geçiş güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="1eed2-188">Update the database with the initial migration.</span></span>

<span data-ttu-id="1eed2-189">Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="1eed2-189">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="1eed2-191">PMC aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="1eed2-191">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="1eed2-192">Alternatif olarak, aşağıdaki .NET Core CLI komutları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="1eed2-192">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="1eed2-193">Aşağıdaki iletiyi göz ardı edin:</span><span class="sxs-lookup"><span data-stu-id="1eed2-193">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="1eed2-194">Sonraki öğreticide düzeltin.</span><span class="sxs-lookup"><span data-stu-id="1eed2-194">You fix that in the next tutorial.</span></span>

<span data-ttu-id="1eed2-195">`Install-Package` Komutu yapı iskelesi altyapısı çalıştırmak için gerekli araçları yükler.</span><span class="sxs-lookup"><span data-stu-id="1eed2-195">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="1eed2-196">`Add-Migration` Komutu ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1eed2-196">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="1eed2-197">Belirtilen model şeması dayalı `DbContext` (içinde *Models/MovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="1eed2-197">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="1eed2-198">`Initial` Bağımsız değişkeni geçişler adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1eed2-198">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="1eed2-199">Herhangi bir ad kullanabilirsiniz, ancak kurala göre geçiş açıklayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="1eed2-199">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="1eed2-200">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1eed2-200">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="1eed2-201">`Update-Database` Komutu çalıştırır `Up` yönteminde *geçişleri / {zaman damgası} _InitialCreate.cs* dosyası bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1eed2-201">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="1eed2-202">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="1eed2-202">Test the app</span></span>

* <span data-ttu-id="1eed2-203">Uygulamayı çalıştırın ve append `/Movies` URL tarayıcıda (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="1eed2-203">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="1eed2-204">Test **oluşturma** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="1eed2-204">Test the **Create** link.</span></span>

  ![Sayfa oluşturma](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="1eed2-206">Test **Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="1eed2-206">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="1eed2-207">Bir SQL özel durumu alırsanız, geçişler çalıştırın ve veritabanı güncelleştirilmiş doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="1eed2-207">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="1eed2-208">Sonraki öğretici yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="1eed2-208">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1eed2-209">[Önceki: Başlama](xref:tutorials/razor-pages/razor-pages-start)
> [sonraki: iskele kurulmuş Razor sayfaları](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="1eed2-209">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
