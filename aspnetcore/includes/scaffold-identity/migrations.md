<span data-ttu-id="9db8c-101">Oluşturulan kimlik veritabanı kodu [Entity Framework Core geçişleri](/ef/core/managing-schemas/migrations/)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="9db8c-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="9db8c-102">Bir geçiş oluşturmak ve veritabanı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="9db8c-102">Create a migration and update the database.</span></span> <span data-ttu-id="9db8c-103">Örneğin, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9db8c-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9db8c-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9db8c-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9db8c-105">Visual Studio **Paket Yöneticisi Konsolu**:</span><span class="sxs-lookup"><span data-stu-id="9db8c-105">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9db8c-106">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9db8c-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="9db8c-107">`Add-Migration` Komut için "createıdentityschema" ad parametresi rastgele.</span><span class="sxs-lookup"><span data-stu-id="9db8c-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="9db8c-108">`"CreateIdentitySchema"`geçişi açıklar.</span><span class="sxs-lookup"><span data-stu-id="9db8c-108">`"CreateIdentitySchema"` describes the migration.</span></span>
