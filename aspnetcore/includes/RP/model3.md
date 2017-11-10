<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>İskele araç ekleyin ve başlangıç geçiş gerçekleştirme

Komut satırından aşağıdaki .NET Core CLI komutları çalıştırın:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

`add package` Komutu yapı iskelesi altyapısı çalıştırmak için gerekli araçları yükler.

`ef migrations add InitialCreate` Komutu ilk veritabanı şeması oluşturmak için kod oluşturur. Belirtilen model şeması dayalı `DbContext` (içinde *Models/MovieContext.cs* dosyası). `Initial` Bağımsız değişkeni geçişler adlandırmak için kullanılır. Herhangi bir ad kullanabilirsiniz, ancak kurala göre geçiş açıklayan bir ad seçin. Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.

`ef database update` Komutu çalıştırır `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosyası bir veritabanı oluşturur.