Aşağıdaki özellikleri `Movie` sınıfına ekleyin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/Movie.cs?name=snippet1)]

`Movie` sınıfı şunları içerir:

* Birincil anahtar için veritabanı için gerekli olan `Id` alanı.
* `[DataType(DataType.Date)]`: [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) özniteliği verilerin türünü belirtir (`Date`). Bu öznitelikle:

  * Kullanıcının Tarih alanına saat bilgilerini girmesi gerekli değildir.
  * Zaman bilgisi değil yalnızca tarih görüntülenir.

[Veri açıklamaları](/dotnet/api/system.componentmodel.dataannotations) sonraki bir öğreticide ele alınmıştır.