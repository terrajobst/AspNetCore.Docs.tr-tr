<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="3ff8f-101">Bir veritabanı bağlantı dizesi Ekle</span><span class="sxs-lookup"><span data-stu-id="3ff8f-101">Add a database connection string</span></span>

<span data-ttu-id="3ff8f-102">Bir bağlantı dizesi eklemek *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="3ff8f-102">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="3ff8f-103">[!code-json[Ana](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="3ff8f-103">[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="3ff8f-104">Veritabanı bağlamı kaydetme</span><span class="sxs-lookup"><span data-stu-id="3ff8f-104">Register the database context</span></span>

<span data-ttu-id="3ff8f-105">Veritabanı bağlamı ile kayıt [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="3ff8f-105">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>