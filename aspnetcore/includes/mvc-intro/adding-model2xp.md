<a name="cli"></a>
## <a name="perform-initial-migration"></a>İlk geçiş gerçekleştirme

Komut satırından aşağıdaki .NET Core CLI komutları çalıştırın:

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

`dotnet ef migrations add InitialCreate` Komutu ilk veritabanı şeması oluşturmak için kod oluşturur. Belirtilen model şeması dayalı `DbContext` (içinde *Models/MovieContext.cs* dosyası). `Initial` Bağımsız değişkeni geçişler adlandırmak için kullanılır. Herhangi bir ad kullanabilirsiniz, ancak kurala göre geçiş açıklayan bir ad seçin. Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.

`dotnet ef database update` Komutu çalıştırır `Up` yönteminde *geçişleri /\<zaman damgası > _InitialCreate.cs* dosyası bir veritabanı oluşturur.