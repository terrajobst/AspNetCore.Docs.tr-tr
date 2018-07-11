<span data-ttu-id="f754f-101">Aşağıdaki özellikleri `Movie` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="f754f-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="f754f-102">`ID` Alan veritabanı için birincil anahtar tarafından gereklidir.</span><span class="sxs-lookup"><span data-stu-id="f754f-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="f754f-103">Veritabanı bağlamı sınıfının Ekle</span><span class="sxs-lookup"><span data-stu-id="f754f-103">Add a database context class</span></span>

<span data-ttu-id="f754f-104">Aşağıdaki *MovieContext.cs* sınıfının *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="f754f-104">Add the following *MovieContext.cs* class to the *Models* folder:</span></span>  

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

<span data-ttu-id="f754f-105">Yukarıdaki kod oluşturur bir `DbSet` varlık kümesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="f754f-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="f754f-106">Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir ve bir varlık tablosunda bir satıra karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="f754f-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>
