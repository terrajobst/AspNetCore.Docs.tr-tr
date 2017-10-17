<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Bir veritabanı bağlantı dizesi Ekle

Bir bağlantı dizesi eklemek *appsettings.json* dosya.

[!code-json[Ana](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Veritabanı bağlamı kaydetme

Veritabanı bağlamı ile kayıt [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında *haline* dosya.