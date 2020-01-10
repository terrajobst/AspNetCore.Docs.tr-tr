---
title: ASP.NET Core MVC uygulamasına model ekleme
author: rick-anderson
description: Basit bir ASP.NET Core uygulamasına model ekleyin.
ms.author: riande
ms.date: 8/15/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 5d4251a2577111324aa2cfb715c41e3ecad5a9d1
ms.sourcegitcommit: da2fb2d78ce70accdba903ccbfdcfffdd0112123
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75722809"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="6cf2e-103">ASP.NET Core MVC uygulamasına model ekleme</span><span class="sxs-lookup"><span data-stu-id="6cf2e-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="6cf2e-104">[Rick Anderson](https://twitter.com/RickAndMSFT) ve [Tom Dykstra](https://github.com/tdykstra) tarafından</span><span class="sxs-lookup"><span data-stu-id="6cf2e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="6cf2e-105">Bu bölümde, bir veritabanında film yönetmeye yönelik sınıflar eklersiniz.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="6cf2e-106">Bu sınıflar, **d**VC uygulamasının "**d**odel" parçası olacaktır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="6cf2e-107">Bu sınıfları bir veritabanıyla çalışmak için [Entity Framework Core](/ef/core) (EF Core) ile birlikte kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="6cf2e-108">EF Core, yazmanız gereken veri erişim kodunu kolaylaştıran bir nesne ilişkisel eşleme (ORM) çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="6cf2e-109">Oluşturduğunuz model sınıfları, EF Core hiçbir bağımlılığı olmadığından, POCO sınıfları olarak bilinir ( **P**Lain **O**).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="6cf2e-110">Yalnızca veritabanında depolanacak verilerin özelliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="6cf2e-111">Bu öğreticide, önce model sınıflarını yazdığınızda EF Core veritabanını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="6cf2e-112">Burada kapsanmayan alternatif bir yaklaşım var olan bir veritabanından model sınıfları oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="6cf2e-113">Bu yaklaşım hakkında daha fazla bilgi için bkz. [ASP.NET Core-var olan veritabanı](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="6cf2e-114">Veri modeli sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="6cf2e-114">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cf2e-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cf2e-116"> > **sınıf** **eklemek** > *modeller* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="6cf2e-117">Dosyayı *Movie.cs*olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-117">Name the file *Movie.cs*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6cf2e-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6cf2e-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6cf2e-119">*Modeller* klasörüne *Movie.cs* adlı bir dosya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-119">Add a file named *Movie.cs* to the *Models* folder.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6cf2e-120">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6cf2e-121">*Modeller* klasörüne sağ tıklayın > **yeni > Class** > **boş sınıf** **ekleyin** .</span><span class="sxs-lookup"><span data-stu-id="6cf2e-121">Right-click the *Models* folder > **Add** > **New Class** > **Empty Class**.</span></span> <span data-ttu-id="6cf2e-122">Dosyayı *Movie.cs*olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-122">Name the file *Movie.cs*.</span></span>

---

<span data-ttu-id="6cf2e-123">*Movie.cs* dosyasını aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-123">Update the *Movie.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/Movie.cs)]

<span data-ttu-id="6cf2e-124">`Movie` sınıfı, birincil anahtar için veritabanı için gerekli olan bir `Id` alanı içerir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-124">The `Movie` class contains an `Id` field, which is required by the database for the primary key.</span></span>

<span data-ttu-id="6cf2e-125">`ReleaseDate` [veri türü özniteliği,](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) verilerin türünü belirtir (`Date`).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-125">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute on `ReleaseDate` specifies the type of the data (`Date`).</span></span> <span data-ttu-id="6cf2e-126">Bu öznitelikle:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-126">With this attribute:</span></span>

  * <span data-ttu-id="6cf2e-127">Kullanıcının Tarih alanına saat bilgilerini girmesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-127">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="6cf2e-128">Zaman bilgisi değil yalnızca tarih görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-128">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="6cf2e-129">[Veri açıklamaları](/dotnet/api/system.componentmodel.dataannotations) sonraki bir öğreticide ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-129">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="6cf2e-130">NuGet paketleri Ekle</span><span class="sxs-lookup"><span data-stu-id="6cf2e-130">Add NuGet packages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cf2e-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-131">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cf2e-132">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** (PMC) öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-132">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

![PMC menüsü](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="6cf2e-134">PMC 'de şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-134">In the PMC, run the following command:</span></span>

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="6cf2e-135">Yukarıdaki komut, EF Core SQL Server sağlayıcısını ekler.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-135">The preceding command adds the EF Core SQL Server provider.</span></span> <span data-ttu-id="6cf2e-136">Sağlayıcı paketi, EF Core paketini bir bağımlılık olarak yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-136">The provider package installs the EF Core package as a dependency.</span></span> <span data-ttu-id="6cf2e-137">Ek paketler, öğreticinin sonraki bölümlerinde bulunan yapı iskelesi adımında otomatik olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-137">Additional packages are installed automatically in the scaffolding step later in the tutorial.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6cf2e-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6cf2e-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6cf2e-139">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6cf2e-140">**Proje** menüsünde, **NuGet Paketlerini Yönet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-140">From the **Project** menu, select **Manage Nuget Packages**.</span></span>

<span data-ttu-id="6cf2e-141">Sağ üst köşedeki **arama** alanına `Microsoft.EntityFrameworkCore.SQLite` girin ve aramak için **dönüş** tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-141">In the **Search** field in the upper right, enter `Microsoft.EntityFrameworkCore.SQLite` and press the **Return** key to search.</span></span> <span data-ttu-id="6cf2e-142">Eşleşen NuGet paketini seçin ve **paket Ekle** düğmesine basın.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-142">Select the matching NuGet package and press the **Add Package** button.</span></span>

![Entity Framework Core NuGet paketi Ekle](~/tutorials/first-mvc-app-mac/adding-model/_static/add-nuget-packages.png)

<span data-ttu-id="6cf2e-144">Proje **Seç** iletişim kutusu, `MvcMovie` projesi seçili olacak şekilde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-144">The **Select Projects** dialog will be displayed, with the `MvcMovie` project selected.</span></span> <span data-ttu-id="6cf2e-145">**Tamam** düğmesine basın.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-145">Press the **Ok** button.</span></span>

<span data-ttu-id="6cf2e-146">Bir **Lisans kabul** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-146">A **License Acceptance** dialog will be displayed.</span></span> <span data-ttu-id="6cf2e-147">Lisansları istenen şekilde gözden geçirin ve ardından **kabul et** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-147">Review the licenses as desired, then click the **Accept** button.</span></span>

<span data-ttu-id="6cf2e-148">Aşağıdaki NuGet paketlerini yüklemek için yukarıdaki adımları yineleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-148">Repeat the above steps to install the following NuGet packages:</span></span>
 * `Microsoft.VisualStudio.Web.CodeGeneration.Design`
 * `Microsoft.EntityFrameworkCore.SqlServer`
 * `Microsoft.EntityFrameworkCore.Design`

---

<a name="dc"></a>

## <a name="create-a-database-context-class"></a><span data-ttu-id="6cf2e-149">Veritabanı bağlamı sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="6cf2e-149">Create a database context class</span></span>

<span data-ttu-id="6cf2e-150">`Movie` modeli için EF Core işlevselliği (oluşturma, okuma, güncelleştirme, silme) koordine etmek için bir veritabanı bağlamı sınıfı gerekir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-150">A database context class is needed to coordinate EF Core functionality (Create, Read, Update, Delete) for the `Movie` model.</span></span> <span data-ttu-id="6cf2e-151">Veritabanı bağlamı [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) öğesinden türetilir ve veri modeline dahil edilecek varlıkları belirtir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-151">The database context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and specifies the entities to include in the data model.</span></span>

<span data-ttu-id="6cf2e-152">Bir *veri* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-152">Create a *Data* folder.</span></span>

<span data-ttu-id="6cf2e-153">Aşağıdaki kodla bir *Data/MvcMovieContext. cs* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-153">Add a *Data/MvcMovieContext.cs* file with the following code:</span></span> 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="6cf2e-154">Önceki kod, varlık kümesi için bir [Dbset\<filmi >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-154">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="6cf2e-155">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-155">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="6cf2e-156">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-156">An entity corresponds to a row in the table.</span></span>

<a name="reg"></a>

## <a name="register-the-database-context"></a><span data-ttu-id="6cf2e-157">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="6cf2e-157">Register the database context</span></span>

<span data-ttu-id="6cf2e-158">ASP.NET Core, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-158">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6cf2e-159">Hizmetlerin (EF Core DB bağlamı gibi) uygulama başlatma sırasında DI ile kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-159">Services (such as the EF Core DB context) must be registered with DI during application startup.</span></span> <span data-ttu-id="6cf2e-160">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-160">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="6cf2e-161">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-161">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span> <span data-ttu-id="6cf2e-162">Bu bölümde, veritabanı bağlamını dı kapsayıcısına kaydedersiniz.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-162">In this section, you register the database context with the DI container.</span></span>

<span data-ttu-id="6cf2e-163">Aşağıdaki `using` deyimlerini *Startup.cs*üst kısmına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-163">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="6cf2e-164">Aşağıdaki Vurgulanan kodu `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-164">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cf2e-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-165">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6cf2e-166">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

---

<span data-ttu-id="6cf2e-167">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-167">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="6cf2e-168">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-168">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="cs"></a>

## <a name="add-a-database-connection-string"></a><span data-ttu-id="6cf2e-169">Veritabanı bağlantı dizesi Ekle</span><span class="sxs-lookup"><span data-stu-id="6cf2e-169">Add a database connection string</span></span>

<span data-ttu-id="6cf2e-170">*AppSettings. JSON* dosyasına bir bağlantı dizesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-170">Add a connection string to the *appsettings.json* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cf2e-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-171">Visual Studio</span></span>](#tab/visual-studio)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6cf2e-172">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-172">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

---

<span data-ttu-id="6cf2e-173">Projeyi derleyici hatalarına yönelik bir denetim olarak derleyin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-173">Build the project as a check for compiler errors.</span></span>

## <a name="scaffold-movie-pages"></a><span data-ttu-id="6cf2e-174">Yapı iskelesi film sayfaları</span><span class="sxs-lookup"><span data-stu-id="6cf2e-174">Scaffold movie pages</span></span>

<span data-ttu-id="6cf2e-175">Film modeli için oluşturma, okuma, güncelleştirme ve silme (CRUD) sayfaları üretmek için scafkatlama aracını kullanın.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-175">Use the scaffolding tool to produce Create, Read, Update, and Delete (CRUD) pages for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cf2e-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cf2e-177">**Çözüm Gezgini**, *denetleyiciler* klasörüne sağ tıklayıp **yeni > yapı iskelesi > öğesi ekleyin**.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-177">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Yukarıdaki adımın görünümü](adding-model/_static/add_controller21.png)

<span data-ttu-id="6cf2e-179">**Yapı Ekle** iletişim kutusunda, **Entity Framework > Ekle ' yi kullanarak views ile MVC denetleyicisi '** ni seçin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-179">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Yapı Iskelesi Ekle iletişim kutusu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="6cf2e-181">**Denetleyici Ekle** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-181">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="6cf2e-182">**Model sınıfı:** *Film (mvcmovie. modeller)*</span><span class="sxs-lookup"><span data-stu-id="6cf2e-182">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="6cf2e-183">**Veri bağlamı sınıfı:** *mvcmoviecontext (mvcmovie. Data)*</span><span class="sxs-lookup"><span data-stu-id="6cf2e-183">**Data context class:** *MvcMovieContext (MvcMovie.Data)*</span></span>

![Veri bağlamı Ekle](adding-model/_static/dc3.png)

* <span data-ttu-id="6cf2e-185">**Görünümler:** Her seçeneğin varsayılan kısmını işaretli tut</span><span class="sxs-lookup"><span data-stu-id="6cf2e-185">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="6cf2e-186">**Denetleyici adı:** Varsayılan *MoviesController* tut</span><span class="sxs-lookup"><span data-stu-id="6cf2e-186">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="6cf2e-187">**Ekle**’yi seçin</span><span class="sxs-lookup"><span data-stu-id="6cf2e-187">Select **Add**</span></span>

<span data-ttu-id="6cf2e-188">Visual Studio şunları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-188">Visual Studio creates:</span></span>

* <span data-ttu-id="6cf2e-189">Bir filmler denetleyicisi (*denetleyiciler/MoviesController. cs*)</span><span class="sxs-lookup"><span data-stu-id="6cf2e-189">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="6cf2e-190">Razor oluşturma, silme, ayrıntılar, düzenleme ve dizin sayfaları için dosyaları görüntüleme (*Görünümler/filmler/\*. cshtml*)</span><span class="sxs-lookup"><span data-stu-id="6cf2e-190">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="6cf2e-191">Bu dosyaların otomatik olarak oluşturulması, *Yapı iskelesi*olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-191">The automatic creation of these files is known as *scaffolding*.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6cf2e-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6cf2e-192">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="6cf2e-193">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-193">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="6cf2e-194">Linux 'ta, scafkatlama aracı yolunu dışarı aktarın:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-194">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="6cf2e-195">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-195">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6cf2e-196">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6cf2e-197">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-197">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="6cf2e-198">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-198">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of tabs                  -->

<span data-ttu-id="6cf2e-199">Veritabanı mevcut olmadığından, scafkatmış sayfaları henüz kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-199">You can't use the scaffolded pages yet because the database doesn't exist.</span></span> <span data-ttu-id="6cf2e-200">Uygulamayı çalıştırır ve **film uygulaması** bağlantısına tıklarsanız, bir *veritabanı* açılamıyor veya *böyle bir tablo yok: film* hata iletisi.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-200">If you run the app and click on the **Movie App** link, you get a *Cannot open database* or *no such table: Movie* error message.</span></span>

<a name="migration"></a>

## <a name="initial-migration"></a><span data-ttu-id="6cf2e-201">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="6cf2e-201">Initial migration</span></span>

<span data-ttu-id="6cf2e-202">Veritabanını oluşturmak için EF Core [geçişleri](xref:data/ef-mvc/migrations) özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-202">Use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to create the database.</span></span> <span data-ttu-id="6cf2e-203">Geçişler, veri modelinizle eşleşecek bir veritabanı oluşturmanıza ve güncelleştirmenize olanak sağlayan bir araç kümesidir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-203">Migrations is a set of tools that let you create and update a database to match your data model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cf2e-204">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-204">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cf2e-205">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** (PMC) öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-205">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

<span data-ttu-id="6cf2e-206">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-206">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

* <span data-ttu-id="6cf2e-207">`Add-Migration InitialCreate`: bir *geçişler/{timestamp} _InitialCreate. cs* geçiş dosyası oluşturuyor.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-207">`Add-Migration InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="6cf2e-208">`InitialCreate` bağımsız değişkeni geçiş adıdır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-208">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="6cf2e-209">Herhangi bir ad kullanılabilir, ancak kurala göre, geçişi açıklayan bir ad seçilidir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-209">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="6cf2e-210">Bu ilk geçiş olduğundan, oluşturulan sınıf veritabanı şemasını oluşturmak için kod içerir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-210">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="6cf2e-211">Veritabanı şeması, `MvcMovieContext` sınıfında belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-211">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span>

* <span data-ttu-id="6cf2e-212">`Update-Database`: veritabanını, önceki komutun oluşturulduğu en son geçişe güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-212">`Update-Database`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="6cf2e-213">Bu komut, veritabanını oluşturan *geçişler/{Time-damga} _InitialCreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-213">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

  <span data-ttu-id="6cf2e-214">Database Update komutu aşağıdaki uyarıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-214">The database update command generates the following warning:</span></span> 

  > <span data-ttu-id="6cf2e-215">' Movie ' varlık türündeki ' Price ' ondalık sütunu için tür belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-215">No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="6cf2e-216">Bu, varsayılan duyarlık ve ölçeğe uygun olmadıkları takdirde değerlerin sessizce kesilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-216">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="6cf2e-217">' Hasccolumntype () ' kullanarak tüm değerleri barındırabilecek SQL Server sütun türünü açıkça belirtin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-217">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.</span></span>

  <span data-ttu-id="6cf2e-218">Bu uyarıyı yoksayabilirsiniz, daha sonraki bir öğreticide düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-218">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

[!INCLUDE [more information on the PMC tools for EF Core](~/includes/ef-pmc.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6cf2e-219">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-219">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="6cf2e-220">Aşağıdaki .NET Core CLI komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-220">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* <span data-ttu-id="6cf2e-221">`ef migrations add InitialCreate`: bir *geçişler/{timestamp} _InitialCreate. cs* geçiş dosyası oluşturuyor.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-221">`ef migrations add InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="6cf2e-222">`InitialCreate` bağımsız değişkeni geçiş adıdır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-222">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="6cf2e-223">Herhangi bir ad kullanılabilir, ancak kurala göre, geçişi açıklayan bir ad seçilidir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-223">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="6cf2e-224">Bu ilk geçiş olduğundan, oluşturulan sınıf veritabanı şemasını oluşturmak için kod içerir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-224">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="6cf2e-225">Veritabanı şeması, `MvcMovieContext` sınıfında belirtilen modeli temel alır ( *Data/MvcMovieContext. cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-225">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span>

* <span data-ttu-id="6cf2e-226">`ef database update`: veritabanını, önceki komutun oluşturulduğu en son geçişe güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-226">`ef database update`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="6cf2e-227">Bu komut, veritabanını oluşturan *geçişler/{Time-damga} _InitialCreate. cs* dosyasında `Up` yöntemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-227">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [ more information on the CLI tools for EF Core](~/includes/ef-cli.md)]

---

### <a name="the-initialcreate-class"></a><span data-ttu-id="6cf2e-228">Initialcreate sınıfı</span><span class="sxs-lookup"><span data-stu-id="6cf2e-228">The InitialCreate class</span></span>

<span data-ttu-id="6cf2e-229">*Geçişleri/{timestamp} _InitialCreate. cs* geçiş dosyasını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-229">Examine the *Migrations/{timestamp}_InitialCreate.cs* migration file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Migrations/20190805165915_InitialCreate.cs?name=snippet)]

 <span data-ttu-id="6cf2e-230">`Up` yöntemi, film tablosunu oluşturur ve `Id` birincil anahtar olarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-230">The `Up` method creates the Movie table and configures `Id` as the primary key.</span></span> <span data-ttu-id="6cf2e-231">`Down` yöntemi, `Up` geçişi tarafından yapılan şema değişikliklerini geri alır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-231">The `Down` method reverts the schema changes made by the `Up` migration.</span></span>

<a name="test"></a>

## <a name="test-the-app"></a><span data-ttu-id="6cf2e-232">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="6cf2e-232">Test the app</span></span>

* <span data-ttu-id="6cf2e-233">Uygulamayı çalıştırın ve **film uygulaması** bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-233">Run the app and click the **Movie App** link.</span></span>

  <span data-ttu-id="6cf2e-234">Aşağıdakilerden birine benzer bir özel durum alırsanız:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-234">If you get an exception similar to one of the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cf2e-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-235">Visual Studio</span></span>](#tab/visual-studio)

  ```console
  SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6cf2e-236">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-236">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

  ```console
  SqliteException: SQLite Error 1: 'no such table: Movie'.
  ```

---
  <span data-ttu-id="6cf2e-237">Muhtemelen [geçişler adımını](#migration)kaçırdınız.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-237">You probably missed the [migrations step](#migration).</span></span>

* <span data-ttu-id="6cf2e-238">**Oluştur** sayfasını test edin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-238">Test the **Create** page.</span></span> <span data-ttu-id="6cf2e-239">Veri girin ve gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-239">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="6cf2e-240">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-240">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="6cf2e-241">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-241">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="6cf2e-242">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-242">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="6cf2e-243">**Düzenleme**, **Ayrıntılar**ve **silme** sayfalarını test edin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-243">Test the **Edit**, **Details**, and **Delete** pages.</span></span>

## <a name="dependency-injection-in-the-controller"></a><span data-ttu-id="6cf2e-244">Denetleyiciye bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="6cf2e-244">Dependency injection in the controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cf2e-245">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-245">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cf2e-246">*Controllers/MoviesController. cs* dosyasını açın ve oluşturucuyu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-246">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller (or use the old one as I did in the 3.0 upgrade) because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="6cf2e-247">Oluşturucu, veritabanı bağlamını (`MvcMovieContext`) denetleyiciye eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-247">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="6cf2e-248">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-248">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6cf2e-249">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-249">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="6cf2e-250">Oluşturucu, veritabanı bağlamını (`MvcMovieContext`) denetleyiciye eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-250">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="6cf2e-251">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-251">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---
<!-- end of tabs --->

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="6cf2e-252">Türü kesin belirlenmiş modeller ve @model anahtar sözcüğü</span><span class="sxs-lookup"><span data-stu-id="6cf2e-252">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="6cf2e-253">Bu öğreticide daha önce, bir denetleyicinin `ViewData` sözlüğünü kullanarak bir görünüme nasıl veri veya nesne geçirekullanabileceğinizi gördünüz.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-253">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="6cf2e-254">`ViewData` sözlüğü bir görünüme bilgi geçirmek için uygun, geç bağlanan bir yol sağlayan dinamik bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-254">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="6cf2e-255">MVC Ayrıca, kesin olarak belirlenmiş model nesnelerini bir görünüme geçirmeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-255">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="6cf2e-256">Bu kesin türü belirtilmiş yaklaşım derleme zamanı kodu denetimini sunar.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-256">This strongly typed approach enables compile time code checking.</span></span> <span data-ttu-id="6cf2e-257">Yapı iskelesi mekanizması, `MoviesController` sınıfı ve görünümleriyle bu yaklaşımı (türü kesin belirlenmiş bir modeli geçirerek) kullandı.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-257">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views.</span></span>

<span data-ttu-id="6cf2e-258">*Controllers/MoviesController. cs* dosyasında oluşturulan `Details` yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-258">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="6cf2e-259">`id` parametresi genellikle rota verileri olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-259">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="6cf2e-260">Örneğin `https://localhost:5001/movies/details/1` kümeler:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-260">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="6cf2e-261">`movies` denetleyicisine denetleyici (ilk URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-261">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="6cf2e-262">`details` eylemi (ikinci URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-262">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="6cf2e-263">Kimliği 1 ' e (son URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-263">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="6cf2e-264">Aşağıdaki gibi bir sorgu dizesiyle `id` de geçirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-264">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="6cf2e-265">KIMLIK değeri sağlanmazsa `id` parametresi [null yapılabilir bir tür](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-265">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="6cf2e-266">Bir [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) , rota verileriyle veya sorgu dizesi değeriyle eşleşen film varlıklarını seçmek üzere `FirstOrDefaultAsync` ' A geçirilir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-266">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="6cf2e-267">Bir film bulunursa, `Movie` modelinin bir örneği `Details` görünümüne geçirilir:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-267">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="6cf2e-268">*Görünümler/filmler/ayrıntılar. cshtml* dosyasının içeriğini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-268">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="6cf2e-269">Görünüm dosyasının en üstündeki `@model` ifade, görünümün beklediği nesne türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-269">The `@model` statement at the top of the view file specifies the type of object that the view expects.</span></span> <span data-ttu-id="6cf2e-270">Film denetleyicisi oluşturulduğunda, aşağıdaki `@model` deyimleri eklenmiştir:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-270">When the movie controller was created, the following `@model` statement was included:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="6cf2e-271">Bu `@model` yönergesi, denetleyicinin görünüme geçirildiği filme erişimine izin verir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-271">This `@model` directive allows access to the movie that the controller passed to the view.</span></span> <span data-ttu-id="6cf2e-272">`Model` nesne kesin olarak belirlenmiş.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-272">The `Model` object is strongly typed.</span></span> <span data-ttu-id="6cf2e-273">Örneğin, *details. cshtml* görünümünde, kod her bir film alanını `DisplayNameFor` `DisplayFor` ve HTML yardımcılarını türü kesin belirlenmiş `Model` nesnesiyle geçirir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-273">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="6cf2e-274">`Create` ve `Edit` yöntemleri ve görünümleri bir `Movie` model nesnesi de iletir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-274">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="6cf2e-275">*Dizin. cshtml* görünümünü ve film denetleyicisindeki `Index` yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-275">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="6cf2e-276">`List`, `View` yöntemini çağırdığında kodun bir nesne nasıl oluşturduğunu fark edin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-276">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="6cf2e-277">Kod, `Index` eylem yönteminden bu `Movies` listesini görünüme geçirir:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-277">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="6cf2e-278">Film denetleyicisi oluşturulduğunda, yapı iskelesi *Index. cshtml* dosyasının en üstüne aşağıdaki `@model` ifadesini içeriyordu:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-278">When the movies controller was created, scaffolding included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="6cf2e-279">`@model` yönergesi, kesin olarak belirlenmiş bir `Model` nesnesi kullanarak, denetleyicinin görünüme geçirildiği film listesine erişmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-279">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="6cf2e-280">Örneğin, *Index. cshtml* görünümünde, kod kesin türü belirtilmiş `Model` nesnesi üzerinde `foreach` bir ifadesiyle filmlerle döngü yapılır:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-280">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="6cf2e-281">`Model` nesne kesin olarak yazıldığı için (bir `IEnumerable<Movie>` nesnesi olarak), döngüdeki her öğe `Movie`olarak yazılır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-281">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="6cf2e-282">Diğer avantajların yanı sıra, kodu derleme zaman denetimini alacağınız anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-282">Among other benefits, this means that you get compile time checking of the code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6cf2e-283">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6cf2e-283">Additional resources</span></span>

* [<span data-ttu-id="6cf2e-284">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="6cf2e-284">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="6cf2e-285">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="6cf2e-285">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="6cf2e-286">Daha [önce bir görünüm ekleme](adding-view.md)
> [daha sonra SQL ile çalışma](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="6cf2e-286">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="6cf2e-287">Veri modeli sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="6cf2e-287">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cf2e-288">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-288">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cf2e-289"> > **sınıf** **eklemek** > *modeller* klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-289">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="6cf2e-290">Sınıf adı **film**.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-290">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6cf2e-291">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-291">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="6cf2e-292">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-292">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="6cf2e-293">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="6cf2e-293">Scaffold the movie model</span></span>

<span data-ttu-id="6cf2e-294">Bu bölümde, film modeli iskele kurulmuş.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-294">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="6cf2e-295">Diğer bir deyişle, yapı iskelesi aracı sayfaları için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerine yönelik film modeli oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-295">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cf2e-296">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-296">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cf2e-297">**Çözüm Gezgini**, *denetleyiciler* klasörüne sağ tıklayıp **yeni > yapı iskelesi > öğesi ekleyin**.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-297">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![Yukarıdaki adımın görünümü](adding-model/_static/add_controller21.png)

<span data-ttu-id="6cf2e-299">**Yapı Ekle** iletişim kutusunda, **Entity Framework > Ekle ' yi kullanarak views ile MVC denetleyicisi '** ni seçin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-299">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Yapı Iskelesi Ekle iletişim kutusu](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="6cf2e-301">**Denetleyici Ekle** iletişim kutusunu doldurun:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-301">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="6cf2e-302">**Model sınıfı:** *Film (mvcmovie. modeller)*</span><span class="sxs-lookup"><span data-stu-id="6cf2e-302">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="6cf2e-303">**Veri bağlamı sınıfı:** **+** simgesini seçin ve varsayılan **Mvcmovie. modeller. MvcMovieContext** öğesini ekleyin</span><span class="sxs-lookup"><span data-stu-id="6cf2e-303">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Veri bağlamı Ekle](adding-model/_static/dc.png)

* <span data-ttu-id="6cf2e-305">**Görünümler:** Her seçeneğin varsayılan kısmını işaretli tut</span><span class="sxs-lookup"><span data-stu-id="6cf2e-305">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="6cf2e-306">**Denetleyici adı:** Varsayılan *MoviesController* tut</span><span class="sxs-lookup"><span data-stu-id="6cf2e-306">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="6cf2e-307">**Ekle**’yi seçin</span><span class="sxs-lookup"><span data-stu-id="6cf2e-307">Select **Add**</span></span>

![Denetleyici Ekle iletişim kutusu](adding-model/_static/add_controller2.png)

<span data-ttu-id="6cf2e-309">Visual Studio şunları oluşturur:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-309">Visual Studio creates:</span></span>

* <span data-ttu-id="6cf2e-310">Entity Framework Core [veritabanı bağlam sınıfı](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext. cs*)</span><span class="sxs-lookup"><span data-stu-id="6cf2e-310">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="6cf2e-311">Bir filmler denetleyicisi (*denetleyiciler/MoviesController. cs*)</span><span class="sxs-lookup"><span data-stu-id="6cf2e-311">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="6cf2e-312">Razor oluşturma, silme, ayrıntılar, düzenleme ve dizin sayfaları için dosyaları görüntüleme (*Görünümler/filmler/\*. cshtml*)</span><span class="sxs-lookup"><span data-stu-id="6cf2e-312">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="6cf2e-313">Veritabanı bağlamı ve [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemlerinin ve görünümlerinin otomatik olarak oluşturulması, *Yapı iskelesi*olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-313">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6cf2e-314">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6cf2e-314">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="6cf2e-315">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-315">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6cf2e-316">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-316">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="6cf2e-317">Linux 'ta, scafkatlama aracı yolunu dışarı aktarın:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-317">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="6cf2e-318">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-318">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6cf2e-319">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-319">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6cf2e-320">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-320">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6cf2e-321">Yapı iskelesi Aracı'nı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-321">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="6cf2e-322">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-322">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="6cf2e-323">Uygulamayı çalıştırır ve **MVC filmi** bağlantısına tıklarsanız aşağıdakine benzer bir hata alırsınız:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-323">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cf2e-324">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-324">Visual Studio</span></span>](#tab/visual-studio)

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6cf2e-325">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-325">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

``` error
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)

```

---

<span data-ttu-id="6cf2e-326">Veritabanını oluşturmanız ve bunu yapmak için EF Core [geçişleri](xref:data/ef-mvc/migrations) özelliğini kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-326">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="6cf2e-327">Geçişler veri modelinize uyan bir veritabanı oluşturmanıza ve veri modeliniz değiştiğinde veritabanı şemasını güncelleştirmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-327">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="6cf2e-328">İlk geçiş</span><span class="sxs-lookup"><span data-stu-id="6cf2e-328">Initial migration</span></span>

<span data-ttu-id="6cf2e-329">Bu bölümde, aşağıdaki görevler tamamlanır:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-329">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="6cf2e-330">Bir başlangıç geçiş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-330">Add an initial migration.</span></span>
* <span data-ttu-id="6cf2e-331">Veritabanı, ilk geçiş ile güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-331">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cf2e-332">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-332">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="6cf2e-333">**Araçlar** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** (PMC) öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-333">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![PMC menüsü](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="6cf2e-335">PMC'de aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-335">In the PMC, enter the following commands:</span></span>

   ```PMC
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="6cf2e-336">`Add-Migration` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-336">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="6cf2e-337">Veritabanı şeması, `MvcMovieContext` sınıfında belirtilen modeli temel alır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-337">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span> <span data-ttu-id="6cf2e-338">`Initial` bağımsız değişkeni geçiş adıdır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-338">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="6cf2e-339">Herhangi bir ad kullanılabilir, ancak kurala göre, geçişi açıklayan bir ad kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-339">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="6cf2e-340">Daha fazla bilgi için bkz. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-340">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="6cf2e-341">`Update-Database` Komutu çalıştırmaları `Up` yöntemi *geçişleri / {zaman damgası} _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-341">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6cf2e-342">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-342">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="6cf2e-343">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-343">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="6cf2e-344">Veritabanı şeması, `MvcMovieContext` sınıfında belirtilen modeli temel alır ( *Data/MvcMovieContext. cs* dosyasında).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-344">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="6cf2e-345">`InitialCreate` bağımsız değişkeni geçiş adıdır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-345">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="6cf2e-346">Herhangi bir ad kullanılabilir, ancak kurala göre, geçişi açıklayan bir ad seçilidir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-346">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="6cf2e-347">Bağımlılık ekleme ile kayıtlı bağlamını İnceleme</span><span class="sxs-lookup"><span data-stu-id="6cf2e-347">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="6cf2e-348">ASP.NET Core, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)ile oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-348">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6cf2e-349">Hizmetler (EF Core DB bağlamı gibi) uygulama başlatma sırasında dı ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-349">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="6cf2e-350">Bu hizmetler (örneğin, Razor sayfaları) gerektiren bileşenler bu hizmetler Oluşturucu parametresi üzerinden sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-350">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="6cf2e-351">Bir DB bağlamı örneği alır Oluşturucu kodu öğreticinin ilerleyen bölümlerinde gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-351">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6cf2e-352">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-352">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6cf2e-353">Scafkatlama aracı otomatik olarak bir DB bağlamı oluşturup dı kapsayıcısına kaydetti.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-353">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="6cf2e-354">Aşağıdaki `Startup.ConfigureServices` yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-354">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6cf2e-355">Vurgulanan satırı iskele kurucu tarafından eklendi:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-355">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

<span data-ttu-id="6cf2e-356">`MvcMovieContext` EF Core işlevleri (oluşturma, okuma, güncelleştirme, silme, vb.) için koordinatları `Movie` modeli.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-356">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="6cf2e-357">Veri bağlamı (`MvcMovieContext`) türetilir [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-357">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="6cf2e-358">Veri bağlamı, veri modeline hangi varlıkların ekleneceğini belirtir:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-358">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Data/MvcMovieContext.cs)]

<span data-ttu-id="6cf2e-359">Önceki kod, varlık kümesi için bir [Dbset\<filmi >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-359">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="6cf2e-360">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-360">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="6cf2e-361">Bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-361">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="6cf2e-362">Bağlantı dizesi adı için bağlam üzerinde bir yöntemi çağırarak geçirilen bir [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) nesne.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-362">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="6cf2e-363">Yerel geliştirme için [ASP.NET Core yapılandırma sistemi](xref:fundamentals/configuration/index) bağlantı dizesinden okur *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-363">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6cf2e-364">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cf2e-364">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="6cf2e-365">Bir DB bağlamı oluşturdunuz ve bunu DI kapsayıcısı ile kaydettiniz.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-365">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="6cf2e-366">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="6cf2e-366">Test the app</span></span>

* <span data-ttu-id="6cf2e-367">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-367">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="6cf2e-368">Aşağıdakine benzer bir veritabanı özel durumu alırsanız:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-368">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="6cf2e-369">Eksik [geçişler adım](#pmc).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-369">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="6cf2e-370">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-370">Test the **Create** link.</span></span> <span data-ttu-id="6cf2e-371">Veri girin ve gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-371">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="6cf2e-372">Ondalık virgül kullanımı girmeniz mümkün olmayabilir `Price` alan.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-372">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="6cf2e-373">Desteklemek için [jQuery doğrulama](https://jqueryvalidation.org/) virgül İngilizce olmayan yerel (",") bir ondalık noktasının ve ABD İngilizce olmayan tarih biçimleri, uygulamayı Eğer gerekir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-373">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="6cf2e-374">Genelleştirme hakkında yönergeler için bkz. [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-374">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="6cf2e-375">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-375">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="6cf2e-376">`Startup` sınıfını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-376">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="6cf2e-377">Önceki vurgulanan kod, [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına eklenen film veritabanı bağlamını gösterir:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-377">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="6cf2e-378">`services.AddDbContext<MvcMovieContext>(options =>` kullanılacak veritabanını ve bağlantı dizesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-378">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="6cf2e-379">`=>` [lambda operatörü](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="6cf2e-379">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="6cf2e-380">*Controllers/MoviesController. cs* dosyasını açın ve oluşturucuyu inceleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-380">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="6cf2e-381">Oluşturucu, veritabanı bağlamını (`MvcMovieContext`) denetleyiciye eklemek için [bağımlılık ekleme](xref:fundamentals/dependency-injection) işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-381">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="6cf2e-382">Her bir veritabanı bağlamı kullanılan [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) denetleyici yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-382">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="6cf2e-383">Türü kesin belirlenmiş modeller ve @model anahtar sözcüğü</span><span class="sxs-lookup"><span data-stu-id="6cf2e-383">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="6cf2e-384">Bu öğreticide daha önce, bir denetleyicinin `ViewData` sözlüğünü kullanarak bir görünüme nasıl veri veya nesne geçirekullanabileceğinizi gördünüz.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-384">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="6cf2e-385">`ViewData` sözlüğü bir görünüme bilgi geçirmek için uygun, geç bağlanan bir yol sağlayan dinamik bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-385">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="6cf2e-386">MVC Ayrıca, kesin olarak belirlenmiş model nesnelerini bir görünüme geçirmeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-386">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="6cf2e-387">Bu kesin türü belirtilmiş yaklaşım, kodunuzun daha iyi derleme zaman denetimini sunar.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-387">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="6cf2e-388">Yapı iskelesi mekanizması, yöntem ve görünümleri oluştururken `MoviesController` sınıfı ve görünümleriyle bu yaklaşımı (türü kesin belirlenmiş bir model geçirme) kullanır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-388">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="6cf2e-389">*Controllers/MoviesController. cs* dosyasında oluşturulan `Details` yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-389">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="6cf2e-390">`id` parametresi genellikle rota verileri olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-390">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="6cf2e-391">Örneğin `https://localhost:5001/movies/details/1` kümeler:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-391">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="6cf2e-392">`movies` denetleyicisine denetleyici (ilk URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-392">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="6cf2e-393">`details` eylemi (ikinci URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-393">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="6cf2e-394">Kimliği 1 ' e (son URL segmenti).</span><span class="sxs-lookup"><span data-stu-id="6cf2e-394">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="6cf2e-395">Aşağıdaki gibi bir sorgu dizesiyle `id` de geçirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-395">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="6cf2e-396">KIMLIK değeri sağlanmazsa `id` parametresi [null yapılabilir bir tür](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-396">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="6cf2e-397">Bir [lambda ifadesi](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) , rota verileriyle veya sorgu dizesi değeriyle eşleşen film varlıklarını seçmek üzere `FirstOrDefaultAsync` ' A geçirilir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-397">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="6cf2e-398">Bir film bulunursa, `Movie` modelinin bir örneği `Details` görünümüne geçirilir:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-398">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="6cf2e-399">*Görünümler/filmler/ayrıntılar. cshtml* dosyasının içeriğini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-399">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="6cf2e-400">Görünüm dosyasının üst kısmına bir `@model` ifadesini ekleyerek, görünümün beklediği nesne türünü belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-400">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="6cf2e-401">Film denetleyicisini oluştururken, *Ayrıntılar. cshtml* dosyasının en üstüne aşağıdaki `@model` deyimleri otomatik olarak eklenmiştir:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-401">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="6cf2e-402">Bu `@model` yönergesi, kesin olarak belirlenmiş bir `Model` nesnesi kullanarak denetleyicinin görünüme geçirildiği filme erişmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-402">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="6cf2e-403">Örneğin, *details. cshtml* görünümünde, kod her bir film alanını `DisplayNameFor` `DisplayFor` ve HTML yardımcılarını türü kesin belirlenmiş `Model` nesnesiyle geçirir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-403">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="6cf2e-404">`Create` ve `Edit` yöntemleri ve görünümleri bir `Movie` model nesnesi de iletir.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-404">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="6cf2e-405">*Dizin. cshtml* görünümünü ve film denetleyicisindeki `Index` yöntemini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-405">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="6cf2e-406">`List`, `View` yöntemini çağırdığında kodun bir nesne nasıl oluşturduğunu fark edin.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-406">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="6cf2e-407">Kod, `Index` eylem yönteminden bu `Movies` listesini görünüme geçirir:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-407">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="6cf2e-408">Film denetleyicisini oluştururken, yapı iskelesi *Index. cshtml* dosyasının en üstüne aşağıdaki `@model` ifadesini otomatik olarak dahil edin:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-408">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="6cf2e-409">`@model` yönergesi, kesin olarak belirlenmiş bir `Model` nesnesi kullanarak, denetleyicinin görünüme geçirildiği film listesine erişmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-409">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="6cf2e-410">Örneğin, *Index. cshtml* görünümünde, kod kesin türü belirtilmiş `Model` nesnesi üzerinde `foreach` bir ifadesiyle filmlerle döngü yapılır:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-410">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="6cf2e-411">`Model` nesne kesin olarak yazıldığı için (bir `IEnumerable<Movie>` nesnesi olarak), döngüdeki her öğe `Movie`olarak yazılır.</span><span class="sxs-lookup"><span data-stu-id="6cf2e-411">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="6cf2e-412">Diğer avantajların yanı sıra, kodun derleme zaman denetimini alacağınız anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="6cf2e-412">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6cf2e-413">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6cf2e-413">Additional resources</span></span>

* [<span data-ttu-id="6cf2e-414">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="6cf2e-414">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="6cf2e-415">Genelleştirme ve yerelleştirme</span><span class="sxs-lookup"><span data-stu-id="6cf2e-415">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="6cf2e-416">Daha önce bir [veritabanı Ile çalışmaya](working-with-sql.md)
> [bir görünüm ekleme](adding-view.md)</span><span class="sxs-lookup"><span data-stu-id="6cf2e-416">[Previous Adding a View](adding-view.md)
[Next Working with a database](working-with-sql.md)</span></span>

::: moniker-end
