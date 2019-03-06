<a name="dc"></a>
### 

Aşağıdaki `RazorPagesMovieContext` sınıfının *modelleri* klasörü:  

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

Yukarıdaki kod oluşturur bir `DbSet` varlık kümesi özelliği. Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir ve bir varlık tablosunda bir satıra karşılık gelir.

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a>Bir veritabanı bağlantı dizesi Ekle

Bir bağlantı dizesi Ekle *appsettings.json* dosyası:

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a>Gerekli NuGet paketleri Ekle

SQLite ve CodeGeneration.Design projeye eklemek için aşağıdaki .NET Core CLI komutunu çalıştırın:

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

`Microsoft.VisualStudio.Web.CodeGeneration.Design` Paket iskele oluşturma için gereklidir.

<a name="reg"></a>

### <a name="register-the-database-context"></a>Veritabanı bağlamı Kaydet

Aşağıdaki `using` deyimleri en üstündeki *Startup.cs*:

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Veritabanı bağlamı ile kaydetme [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında `Startup.ConfigureServices`.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

Hatalar için bir onay olarak, projeyi derleyin.
