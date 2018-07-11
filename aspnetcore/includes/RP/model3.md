<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Yapı iskelesi araçları ekleyin ve ilk geçiş gerçekleştirme

Komut satırından aşağıdaki .NET Core CLI komutları çalıştırın:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

`add package` Komut yapı iskelesi altyapısı çalıştırmak için gerekli araçları yükler.

`ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur. Belirtilen model şeması dayanır `DbContext` (içinde *Models/MovieContext.cs* dosyası). `InitialCreate` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır. Herhangi bir adı kullanabilirsiniz, ancak bu kurala göre geçiş tanımlayan bir ad seçin. Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.

`ef database update` Komutu çalıştırmaları `Up` yöntemi *geçişleri /\<zaman damgası > _InitialCreate.cs* dosyasını veritabanı oluşturur.
