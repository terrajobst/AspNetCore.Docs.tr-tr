<span data-ttu-id="56244-101">Aşağıdaki özellikleri ekleyin `Movie` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="56244-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="56244-102">`ID` Alan veritabanı için birincil anahtarı gerekli.</span><span class="sxs-lookup"><span data-stu-id="56244-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="56244-103">Veritabanı bağlamı sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="56244-103">Add a database context class</span></span>

<span data-ttu-id="56244-104">Ekleme bir `DbContext` türetilmiş sınıf adlı *MovieContext.cs* için *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="56244-104">Add a `DbContext` derived class named *MovieContext.cs* to the *Models* folder.</span></span>
[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

<span data-ttu-id="56244-105">Önceki kod oluşturur bir `DbSet` özelliği için varlık kümesi.</span><span class="sxs-lookup"><span data-stu-id="56244-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="56244-106">Entity Framework terminoloji, bir varlık kümesine genellikle bir veritabanı tablosuna karşılık gelir ve bir varlık tablosunda bir satırı karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="56244-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>
