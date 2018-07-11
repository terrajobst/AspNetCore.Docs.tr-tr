<a name="test"></a>
### <a name="test-the-app"></a>Uygulamayı test etme

* Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).
* Test **Oluştur** bağlantı.

  ![sayfası oluşturma](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.

Şu hatayı alırsanız, geçişler çalıştırma ve veritabanına güncelleştirilmiş doğrulayın:

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```
