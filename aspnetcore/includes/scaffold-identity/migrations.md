Oluşturulan kimlik veritabanı kod gerektirir [Entity Framework Çekirdek geçişler](/ef/core/managing-schemas/migrations/). Bir geçiş oluşturun ve veritabanı güncelleştirin. Örneğin, aşağıdaki komutları çalıştırın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio'da **Paket Yöneticisi Konsolu**:

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

"CreateIdentitySchema" name parametresi için `Add-Migration` komuttur rasgele. `"CreateIdentitySchema"` geçiş açıklar.