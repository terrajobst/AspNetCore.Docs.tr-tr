<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="87e5a-101">Veritabanı bağlamı sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="87e5a-101">Add a database context class</span></span>

<span data-ttu-id="87e5a-102">RazorPagesMovie projesinde, *veri*adlı yeni bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="87e5a-102">In the RazorPagesMovie project, create a new folder called *Data*.</span></span> <span data-ttu-id="87e5a-103">Aşağıdaki `RazorPagesMovieContext` sınıfı *veri* klasörüne ekleyin:</span><span class="sxs-lookup"><span data-stu-id="87e5a-103">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="87e5a-104">Önceki kod, varlık kümesi `DbSet` için bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="87e5a-104">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="87e5a-105">Entity Framework terimlerinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir ve bir varlık tablodaki bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="87e5a-105">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="87e5a-106">Veritabanı bağlantı dizesi Ekle</span><span class="sxs-lookup"><span data-stu-id="87e5a-106">Add a database connection string</span></span>

<span data-ttu-id="87e5a-107">Aşağıdaki Vurgulanan kodda gösterildiği gibi *appSettings. JSON* dosyasına bir bağlantı dizesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="87e5a-107">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="87e5a-108">NuGet paketleri ve EF araçları ekleme</span><span class="sxs-lookup"><span data-stu-id="87e5a-108">Add NuGet packages and EF tools</span></span>

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="87e5a-109">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="87e5a-109">Register the database context</span></span>

<span data-ttu-id="87e5a-110">Aşağıdaki `using` deyimlerini *Startup.cs*üst kısmına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="87e5a-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="87e5a-111">Veritabanı bağlamını içindeki `Startup.ConfigureServices` [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="87e5a-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="87e5a-112">Gerekli NuGet paketlerini ekleyin</span><span class="sxs-lookup"><span data-stu-id="87e5a-112">Add required NuGet packages</span></span>

<span data-ttu-id="87e5a-113">SQLite ve CodeGeneration eklemek için aşağıdaki .NET Core CLI komutunu çalıştırın. projeye tasarım:</span><span class="sxs-lookup"><span data-stu-id="87e5a-113">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
```

<span data-ttu-id="87e5a-114">Paket `Microsoft.VisualStudio.Web.CodeGeneration.Design` , yapı iskelesi için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="87e5a-114">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="87e5a-115">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="87e5a-115">Register the database context</span></span>

<span data-ttu-id="87e5a-116">Aşağıdaki `using` deyimlerini *Startup.cs*üst kısmına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="87e5a-116">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="87e5a-117">Veritabanı bağlamını içindeki `Startup.ConfigureServices` [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="87e5a-117">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="87e5a-118">Hataları denetlemek için projeyi bir denetim olarak derleyin.</span><span class="sxs-lookup"><span data-stu-id="87e5a-118">Build the project as a check for errors.</span></span>
::: moniker-end
