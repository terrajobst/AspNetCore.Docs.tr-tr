<a name="cli"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="312fb-101">İlk geçiş gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="312fb-101">Perform initial migration</span></span>

<span data-ttu-id="312fb-102">Komut satırından aşağıdaki .NET Core CLI komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="312fb-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="312fb-103">`dotnet ef migrations add InitialCreate` Komutu ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="312fb-103">The `dotnet ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="312fb-104">Belirtilen model şeması dayalı `DbContext` (içinde *Models/MvcMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="312fb-104">The schema is based on the model specified in the `DbContext` (In the *Models/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="312fb-105">`Initial` Bağımsız değişkeni geçişler adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="312fb-105">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="312fb-106">Herhangi bir ad kullanabilirsiniz, ancak kurala göre geçiş açıklayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="312fb-106">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="312fb-107">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="312fb-107">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="312fb-108">`dotnet ef database update` Komutu çalıştırır `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosyası bir veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="312fb-108">The `dotnet ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
