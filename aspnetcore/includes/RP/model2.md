<a name="dc"></a>
### 

<span data-ttu-id="a9358-101">Aşağıdaki `RazorPagesMovieContext` sınıfının *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="a9358-101">Add the following `RazorPagesMovieContext` class to the *Models* folder:</span></span>  

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="a9358-102">Yukarıdaki kod oluşturur bir `DbSet` varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="a9358-102">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="a9358-103">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir ve bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a9358-103">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="a9358-104">Bir veritabanı bağlantı dizesi Ekle</span><span class="sxs-lookup"><span data-stu-id="a9358-104">Add a database connection string</span></span>

<span data-ttu-id="a9358-105">Bir bağlantı dizesi Ekle *appsettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="a9358-105">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="a9358-106">Gerekli NuGet paketleri Ekle</span><span class="sxs-lookup"><span data-stu-id="a9358-106">Add required NuGet packages</span></span>

<span data-ttu-id="a9358-107">SQLite ve CodeGeneration.Design projeye eklemek için aşağıdaki .NET Core CLI komutunu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a9358-107">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

<span data-ttu-id="a9358-108">`Microsoft.VisualStudio.Web.CodeGeneration.Design` Paket iskele oluşturma için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a9358-108">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="a9358-109">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="a9358-109">Register the database context</span></span>

<span data-ttu-id="a9358-110">Aşağıdaki `using` deyimleri en üstündeki *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9358-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="a9358-111">Veritabanı bağlamı ile kaydetme [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a9358-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="a9358-112">Hatalar için bir onay olarak, projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="a9358-112">Build the project as a check for errors.</span></span>
