Aşağıdaki özellikleri ekleyin `Movie` sınıfı:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

`ID` Alan veritabanı için birincil anahtarı gerekli.

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>Veritabanı bağlamı sınıfı ekleme

Aşağıdakileri ekleyin *MovieContext.cs* sınıfının *modelleri* klasörü: [!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

Önceki kod oluşturur bir `DbSet` özelliği için varlık kümesi. Entity Framework terminoloji, bir varlık kümesine genellikle bir veritabanı tablosuna karşılık gelir ve bir varlık tablosunda bir satırı karşılık gelir.
