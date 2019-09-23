<span data-ttu-id="fc971-101">Aşağıdaki .NET Core CLI komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="fc971-101">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="fc971-102">Yukarıdaki komutlar şunları ekler:</span><span class="sxs-lookup"><span data-stu-id="fc971-102">The preceding commands add:</span></span>

* <span data-ttu-id="fc971-103">[ASPNET-CodeGenerator scafkatlama aracı](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span><span class="sxs-lookup"><span data-stu-id="fc971-103">The [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>
* <span data-ttu-id="fc971-104">.NET Core CLI için Entity Framework Core araçları.</span><span class="sxs-lookup"><span data-stu-id="fc971-104">The Entity Framework Core Tools for the .NET Core CLI.</span></span>
* <span data-ttu-id="fc971-105">EF Core paketini bir bağımlılık olarak yükleyecek EF Core SQLite sağlayıcı.</span><span class="sxs-lookup"><span data-stu-id="fc971-105">The EF Core SQLite provider, which installs the EF Core package as a dependency.</span></span>
* <span data-ttu-id="fc971-106">Yapı iskelesi için gereken paketler: `Microsoft.VisualStudio.Web.CodeGeneration.Design` ve `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="fc971-106">Packages needed for scaffolding: `Microsoft.VisualStudio.Web.CodeGeneration.Design` and `Microsoft.EntityFrameworkCore.SqlServer`.</span></span>
