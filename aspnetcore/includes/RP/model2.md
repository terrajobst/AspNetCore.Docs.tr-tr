Aşağıdaki özellikleri `Movie` sınıfı:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

`ID` Alan veritabanı için birincil anahtar tarafından gereklidir.

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>Veritabanı bağlamı sınıfının Ekle

Aşağıdaki *MovieContext.cs* sınıfının *modelleri* klasörü:  

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

Yukarıdaki kod oluşturur bir `DbSet` varlık kümesi özelliği. Entity Framework terminolojisinde, bir varlık kümesini genellikle bir veritabanı tablosuna karşılık gelir ve bir varlık tablosunda bir satıra karşılık gelir.
