<span data-ttu-id="cd1ec-101">Oluşturulan kimlik veritabanı kod gerektirir [Entity Framework Çekirdek geçişler](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="cd1ec-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="cd1ec-102">Bir geçiş oluşturun ve veritabanı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="cd1ec-102">Create a migration and update the database.</span></span> <span data-ttu-id="cd1ec-103">Örneğin, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cd1ec-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cd1ec-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd1ec-104">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="cd1ec-105">Visual Studio'da **Paket Yöneticisi Konsolu**:</span><span class="sxs-lookup"><span data-stu-id="cd1ec-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cd1ec-106">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="cd1ec-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

<span data-ttu-id="cd1ec-107">"CreateIdentitySchema" name parametresi için `Add-Migration` komuttur rasgele.</span><span class="sxs-lookup"><span data-stu-id="cd1ec-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="cd1ec-108">' "CreateIdentitySchema" geçiş açıklar.</span><span class="sxs-lookup"><span data-stu-id="cd1ec-108">\`"CreateIdentitySchema" describes the migration.</span></span>