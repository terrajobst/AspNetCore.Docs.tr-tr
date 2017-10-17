Aşağıdaki tabloda Ayrıntılar ASP.NET Core oluşturucuları parametreleri kod:

| Parametre               | Açıklama|
| ----------------- | ------------ |
| -m  | Modelin adı. |
| -dc  | Veri bağlamı. |
| -udl | Varsayılan düzenini kullanın. |
| -outDir | Görünümleri oluşturmak için göreli çıkış klasörü yolu. |
| --referenceScriptLibraries | Ekler `_ValidationScriptsPartial` sayfaları oluşturmak ve düzenlemek için |

Kullanım `h` hakkında Yardım almak için anahtar `aspnet-codegenerator razorpage` komutu:

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a>Uygulamayı test etme

* Uygulamayı çalıştırın ve append `/Movies` URL tarayıcıda (`http://localhost:port/movies`).
* Test **oluşturma** bağlantı.

 ![Sayfa oluşturma](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Test **Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar.

Aşağıdaki hata alırsanız, geçişler çalıştırın ve veritabanı güncelleştirilmiş doğrulayın:

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```