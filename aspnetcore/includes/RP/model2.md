<span data-ttu-id="d1dff-101">Aşağıdaki özellikleri ekleyin `Movie` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="d1dff-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="d1dff-102">`ID` Alan veritabanı için birincil anahtarı gerekli.</span><span class="sxs-lookup"><span data-stu-id="d1dff-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="d1dff-103">Veritabanı bağlamı sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="d1dff-103">Add a database context class</span></span>

<span data-ttu-id="d1dff-104">Aşağıdakileri ekleyin *MovieContext.cs* sınıfının *modelleri* klasörü: [!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="d1dff-104">Add the following  *MovieContext.cs* class to the *Models* folder: [!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span></span>

<span data-ttu-id="d1dff-105">Önceki kod oluşturur bir `DbSet` özelliği için varlık kümesi.</span><span class="sxs-lookup"><span data-stu-id="d1dff-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="d1dff-106">Entity Framework terminoloji, bir varlık kümesine genellikle bir veritabanı tablosuna karşılık gelir ve bir varlık tablosunda bir satırı karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="d1dff-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>
