<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="8c8c0-101">Yapı iskelesi araçları ekleyin ve ilk geçiş gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="8c8c0-101">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="8c8c0-102">Aşağıdaki satırları ekleyin *RazorPagesMovie.csproj* dosyasını kapatmadan önce yalnızca `</Project>` etiketi:</span><span class="sxs-lookup"><span data-stu-id="8c8c0-102">Add the following lines to the *RazorPagesMovie.csproj* file, just before the closing `</Project>` tag:</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.1.0-preview1-final"/>
</ItemGroup>
```  
<span data-ttu-id="8c8c0-103">Komut satırından aşağıdaki .NET Core CLI komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="8c8c0-103">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="8c8c0-104">`DotNetCliToolReference` Öğesi ve `add package` yapı iskelesi altyapısı çalıştırmak için gerekli araçları yükleme komutu.</span><span class="sxs-lookup"><span data-stu-id="8c8c0-104">The `DotNetCliToolReference` element and the `add package` command install the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="8c8c0-105">`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8c8c0-105">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="8c8c0-106">Belirtilen model şeması dayanır `DbContext` (içinde *Models/MovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="8c8c0-106">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="8c8c0-107">`InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8c8c0-107">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="8c8c0-108">Herhangi bir adı kullanabilirsiniz, ancak bu kurala göre geçiş tanımlayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="8c8c0-108">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="8c8c0-109">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8c8c0-109">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="8c8c0-110">`ef database update` Komutu çalıştırmaları `Up` yöntemi *geçişleri /\<zaman damgası > _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8c8c0-110">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
