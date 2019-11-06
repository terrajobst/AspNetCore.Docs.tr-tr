<!-- THIS INCLUDE USED BY MVC AND RP -->
Aşağıdaki özellikleri `Movie` sınıfına ekleyin:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

`Movie` sınıfı şunları içerir:

* `ID` alanı birincil anahtar için veritabanı gerektirir.
* `[DataType(DataType.Date)]`: [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) özniteliği verilerin türünü belirtir (Tarih). Bu öznitelikle:

  * Kullanıcının Tarih alanına saat bilgilerini girmesi gerekli değildir.
  * Zaman bilgisi değil yalnızca tarih görüntülenir.

[Veri açıklamaları](/dotnet/api/system.componentmodel.dataannotations) sonraki bir öğreticide ele alınmıştır.
