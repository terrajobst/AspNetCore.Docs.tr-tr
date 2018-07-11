<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="680c6-101">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="680c6-101">Test the app</span></span>

* <span data-ttu-id="680c6-102">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="680c6-102">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="680c6-103">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="680c6-103">Test the **Create** link.</span></span>

  ![sayfası oluşturma](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="680c6-105">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="680c6-105">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="680c6-106">Şu hatayı alırsanız, geçişler çalıştırma ve veritabanına güncelleştirilmiş doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="680c6-106">If you get the following error, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```
