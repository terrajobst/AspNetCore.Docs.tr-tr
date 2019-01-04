<!-- THIS INCLUDE USED BY MVC AND RP --> Aşağıdaki özellikleri `Movie` sınıfı:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

`Movie` Sınıfı içerir:

* `ID` Alan veritabanı için birincil anahtar tarafından gereklidir.
* `[DataType(DataType.Date)]`:  [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) özniteliği (tarih) veri türünü belirtir. Bu öznitelik ile:

  * Kullanıcının tarih alanı saat bilgilerini girmek için gerekli değildir.
  * Yalnızca tarih görüntülenen, olmayan zaman bilgilerdir.

[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) bir sonraki öğreticide ele alınmaktadır.