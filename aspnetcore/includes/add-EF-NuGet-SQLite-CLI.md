Aşağıdaki .NET Core CLI komutları çalıştırın:

```dotnetcli
dotnet tool install --global dotnet-ef
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Yukarıdaki komutlar şunları ekler:

* [ASPNET-CodeGenerator scafkatlama aracı](xref:fundamentals/tools/dotnet-aspnet-codegenerator).
* .NET Core CLI için Entity Framework Core araçları.
* EF Core paketini bir bağımlılık olarak yükleyecek EF Core SQLite sağlayıcı.
* Yapı iskelesi için gereken paketler: `Microsoft.VisualStudio.Web.CodeGeneration.Design` ve `Microsoft.EntityFrameworkCore.SqlServer`.
