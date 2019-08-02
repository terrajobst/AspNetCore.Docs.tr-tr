<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="6018a-101">Veritabanı bağlamı sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="6018a-101">Add a database context class</span></span>

<span data-ttu-id="6018a-102">RazorPagesMovie projesinde, *veri*adlı yeni bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6018a-102">In the RazorPagesMovie project, create a new folder called *Data*.</span></span> <span data-ttu-id="6018a-103">Aşağıdaki `RazorPagesMovieContext` sınıfı *veri* klasörüne ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6018a-103">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="6018a-104">Önceki kod, varlık kümesi `DbSet` için bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6018a-104">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="6018a-105">Entity Framework terimlerinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir ve bir varlık tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6018a-105">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="6018a-106">Veritabanı bağlantı dizesi Ekle</span><span class="sxs-lookup"><span data-stu-id="6018a-106">Add a database connection string</span></span>

<span data-ttu-id="6018a-107">Aşağıdaki Vurgulanan kodda gösterildiği gibi *appSettings. JSON* dosyasına bir bağlantı dizesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6018a-107">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="6018a-108">Gerekli NuGet paketlerini ekleyin</span><span class="sxs-lookup"><span data-stu-id="6018a-108">Add required NuGet packages</span></span>

<span data-ttu-id="6018a-109">RazorPagesMovie projesi için bir Terminal açın.</span><span class="sxs-lookup"><span data-stu-id="6018a-109">Open a terminal for the RazorPagesMovie project.</span></span>  <span data-ttu-id="6018a-110">Tasarım/yerleşim çubuğunda Proje adına sağ tıklayın ve terminalde **aç > araçlar** ' a gidin.</span><span class="sxs-lookup"><span data-stu-id="6018a-110">Right click the project name in the design/layout bar and go to **Tools > Open** in Terminal.</span></span> <span data-ttu-id="6018a-111">Termial içinde aşağıdaki .NET Core CLI komutlarını çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6018a-111">Run the following .NET Core CLI commands in the Termial.</span></span> <span data-ttu-id="6018a-112">Bu komutlar, SQLite, Entity Framework Core ve CodeGeneration. Design öğesine, projeye tasarım ekler:</span><span class="sxs-lookup"><span data-stu-id="6018a-112">The commands add SQLite, Entity Framework Core, and  CodeGeneration.Design to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

<span data-ttu-id="6018a-113">Paket `Microsoft.VisualStudio.Web.CodeGeneration.Design` , yapı iskelesi için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6018a-113">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="6018a-114">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="6018a-114">Register the database context</span></span>

<span data-ttu-id="6018a-115">Aşağıdaki `using` deyimlerini *Startup.cs*üst kısmına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6018a-115">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="6018a-116">Veritabanı bağlamını içindeki `Startup.ConfigureServices` [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="6018a-116">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="6018a-117">Gerekli NuGet paketlerini ekleyin</span><span class="sxs-lookup"><span data-stu-id="6018a-117">Add required NuGet packages</span></span>

<span data-ttu-id="6018a-118">SQLite ve CodeGeneration eklemek için aşağıdaki .NET Core CLI komutunu çalıştırın. projeye tasarım:</span><span class="sxs-lookup"><span data-stu-id="6018a-118">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

<span data-ttu-id="6018a-119">Paket `Microsoft.VisualStudio.Web.CodeGeneration.Design` , yapı iskelesi için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6018a-119">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="6018a-120">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="6018a-120">Register the database context</span></span>

<span data-ttu-id="6018a-121">Aşağıdaki `using` deyimlerini *Startup.cs*üst kısmına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6018a-121">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="6018a-122">Veritabanı bağlamını içindeki `Startup.ConfigureServices` [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="6018a-122">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="6018a-123">Hataları denetlemek için projeyi bir denetim olarak derleyin.</span><span class="sxs-lookup"><span data-stu-id="6018a-123">Build the project as a check for errors.</span></span>
::: moniker-end
