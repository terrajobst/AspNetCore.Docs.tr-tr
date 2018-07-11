<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="acab1-101">Bir veritabanı bağlantı dizesi Ekle</span><span class="sxs-lookup"><span data-stu-id="acab1-101">Add a database connection string</span></span>

<span data-ttu-id="acab1-102">Bir bağlantı dizesi Ekle *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="acab1-102">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="acab1-103">Veritabanı bağlamı Kaydet</span><span class="sxs-lookup"><span data-stu-id="acab1-103">Register the database context</span></span>

<span data-ttu-id="acab1-104">Veritabanı bağlamı ile kaydetme [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="acab1-104">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>
