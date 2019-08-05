<a name="dc"></a>

### <a name="add-a-database-context-class"></a>Veritabanı bağlamı sınıfı ekleme

RazorPagesMovie projesinde, *veri*adlı yeni bir klasör oluşturun. Aşağıdaki `RazorPagesMovieContext` sınıfı *veri* klasörüne ekleyin:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

Önceki kod, varlık kümesi `DbSet` için bir özellik oluşturur. Entity Framework terimlerinde, genellikle bir varlık kümesi bir veritabanı tablosuna karşılık gelir ve bir varlık tablodaki bir satıra karşılık gelir.

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a>Veritabanı bağlantı dizesi Ekle

Aşağıdaki Vurgulanan kodda gösterildiği gibi *appSettings. JSON* dosyasına bir bağlantı dizesi ekleyin:

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a>NuGet paketleri ve EF araçları ekleme

RazorPagesMovie projesi için bir Terminal açın.  Tasarım/yerleşim çubuğunda Proje adına sağ tıklayın ve terminalde **aç > araçlar** ' a gidin. Termial içinde aşağıdaki .NET Core CLI komutlarını çalıştırın:

```console
dotnet tool install --global dotnet-ef --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

Yukarıdaki komutlar, .NET CLı için Entity Framework Core araçları ve projeye birkaç paket ekler. Paket `Microsoft.VisualStudio.Web.CodeGeneration.Design` , yapı iskelesi için gereklidir.

<a name="reg"></a>

### <a name="register-the-database-context"></a>Veritabanı bağlamı Kaydet

Aşağıdaki `using` deyimlerini *Startup.cs*üst kısmına ekleyin:

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Veritabanı bağlamını içindeki `Startup.ConfigureServices` [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydedin.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a>Gerekli NuGet paketlerini ekleyin

SQLite ve CodeGeneration eklemek için aşağıdaki .NET Core CLI komutunu çalıştırın. projeye tasarım:

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

Paket `Microsoft.VisualStudio.Web.CodeGeneration.Design` , yapı iskelesi için gereklidir.

<a name="reg"></a>

### <a name="register-the-database-context"></a>Veritabanı bağlamı Kaydet

Aşağıdaki `using` deyimlerini *Startup.cs*üst kısmına ekleyin:

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Veritabanı bağlamını içindeki `Startup.ConfigureServices` [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydedin.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

Hataları denetlemek için projeyi bir denetim olarak derleyin.
::: moniker-end
