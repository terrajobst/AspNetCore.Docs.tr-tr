<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="2f6f3-101">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="2f6f3-101">Test the app</span></span>

* <span data-ttu-id="2f6f3-102">Uygulamayı çalıştırın ve append `/Movies` URL tarayıcıda (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="2f6f3-102">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="2f6f3-103">Test **oluşturma** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="2f6f3-103">Test the **Create** link.</span></span>

  ![Sayfa oluşturma](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="2f6f3-105">Test **Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="2f6f3-105">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="2f6f3-106">Aşağıdaki hata alırsanız, geçişler çalıştırın ve veritabanı güncelleştirilmiş doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="2f6f3-106">If you get the following error, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```