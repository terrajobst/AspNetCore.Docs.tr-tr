Oluşturulan kimlik veritabanı kodu [Entity Framework Core geçişleri](/ef/core/managing-schemas/migrations/)gerektirir. Bir geçiş oluşturmak ve veritabanı güncelleştirin. Örneğin, aşağıdaki komutları çalıştırın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio **Paket Yöneticisi Konsolu**:

```powershell
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

`Add-Migration` Komut için "createıdentityschema" ad parametresi rastgele. `"CreateIdentitySchema"`geçişi açıklar.
