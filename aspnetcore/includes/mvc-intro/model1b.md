Aşağıdaki özellikleri `Movie` sınıfı:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/Movie.cs?name=snippet1)]

`Movie` Sınıfı içerir:

* `Id` Alanı ve veritabanı için birincil anahtarı gereklidir.
* `[DataType(DataType.Date)]`:  [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) özniteliği veri türünü belirtir (`Date`). Bu öznitelik ile:

  * Kullanıcının tarih alanı saat bilgilerini girmek için gerekli değildir.
  * Yalnızca tarih görüntülenen, olmayan zaman bilgilerdir.

[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) bir sonraki öğreticide ele alınmaktadır.