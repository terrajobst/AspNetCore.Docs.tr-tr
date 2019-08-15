::: moniker range=">= aspnetcore-3.0"

<a name="dc"></a>

<span data-ttu-id="5decb-101">Bir *veri* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5decb-101">Create a *Data* folder.</span></span>

<span data-ttu-id="5decb-102">Aşağıdaki `MvcMovieContext` sınıfı *veri* klasörüne ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5decb-102">Add the following `MvcMovieContext` class to the *Data* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="5decb-103">Önceki kod, varlık kümesi `DbSet` için bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5decb-103">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="5decb-104">Entity Framework terimlerinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir ve bir varlık tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="5decb-104">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="5decb-105">Veritabanı bağlantı dizesi Ekle</span><span class="sxs-lookup"><span data-stu-id="5decb-105">Add a database connection string</span></span>

<span data-ttu-id="5decb-106">*AppSettings. JSON* dosyasına bir bağlantı dizesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5decb-106">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="5decb-107">NuGet paketleri ve EF araçları ekleme</span><span class="sxs-lookup"><span data-stu-id="5decb-107">Add NuGet packages and EF tools</span></span>

<span data-ttu-id="5decb-108">Aşağıdaki .NET Core CLI komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="5decb-108">Run the following .NET Core CLI commands:</span></span>

```console
dotnet tool install --global dotnet-ef --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

<span data-ttu-id="5decb-109">Yukarıdaki komutlar, .NET CLı için Entity Framework Core araçları ve projeye birkaç paket ekler.</span><span class="sxs-lookup"><span data-stu-id="5decb-109">The preceding commands add Entity Framework Core Tools for the .NET CLI and several packages to the project.</span></span> <span data-ttu-id="5decb-110">Paket `Microsoft.VisualStudio.Web.CodeGeneration.Design` , yapı iskelesi için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="5decb-110">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="5decb-111">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="5decb-111">Register the database context</span></span>

<span data-ttu-id="5decb-112">Aşağıdaki `using` deyimlerini *Startup.cs*üst kısmına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5decb-112">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="5decb-113">Veritabanı bağlamını içindeki `Startup.ConfigureServices` [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="5decb-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

<span data-ttu-id="5decb-114">Projeyi derleyici hatalarına yönelik bir denetim olarak derleyin.</span><span class="sxs-lookup"><span data-stu-id="5decb-114">Build the project as a check for compiler errors.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5decb-115">Aşağıdaki `MvcMovieContext` sınıfının *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="5decb-115">Add the following `MvcMovieContext` class to the *Models* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="5decb-116">Önceki kod, varlık kümesi `DbSet` için bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5decb-116">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="5decb-117">Entity Framework terimlerinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir ve bir varlık tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="5decb-117">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="5decb-118">Veritabanı bağlantı dizesi Ekle</span><span class="sxs-lookup"><span data-stu-id="5decb-118">Add a database connection string</span></span>

<span data-ttu-id="5decb-119">*AppSettings. JSON* dosyasına bir bağlantı dizesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5decb-119">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="5decb-120">Gerekli NuGet paketlerini ekleyin</span><span class="sxs-lookup"><span data-stu-id="5decb-120">Add required NuGet packages</span></span>

<span data-ttu-id="5decb-121">SQLite ve CodeGeneration eklemek için aşağıdaki .NET Core CLI komutunu çalıştırın. projeye tasarım:</span><span class="sxs-lookup"><span data-stu-id="5decb-121">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

<span data-ttu-id="5decb-122">Paket `Microsoft.VisualStudio.Web.CodeGeneration.Design` , yapı iskelesi için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="5decb-122">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="5decb-123">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="5decb-123">Register the database context</span></span>

<span data-ttu-id="5decb-124">Aşağıdaki `using` deyimlerini *Startup.cs*üst kısmına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5decb-124">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="5decb-125">Veritabanı bağlamını içindeki `Startup.ConfigureServices` [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="5decb-125">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="5decb-126">Hataları denetlemek için projeyi bir denetim olarak derleyin.</span><span class="sxs-lookup"><span data-stu-id="5decb-126">Build the project as a check for errors.</span></span>
::: moniker-end