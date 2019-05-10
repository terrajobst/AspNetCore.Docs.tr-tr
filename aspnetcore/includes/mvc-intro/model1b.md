<span data-ttu-id="0e950-101">Aşağıdaki özellikleri `Movie` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="0e950-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/Movie.cs?name=snippet1)]

<span data-ttu-id="0e950-102">`Movie` Sınıfı içerir:</span><span class="sxs-lookup"><span data-stu-id="0e950-102">The `Movie` class contains:</span></span>

* <span data-ttu-id="0e950-103">`Id` Alanı ve veritabanı için birincil anahtarı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="0e950-103">The `Id` field which is required by the database for the primary key.</span></span>
* <span data-ttu-id="0e950-104">`[DataType(DataType.Date)]`:  [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) özniteliği veri türünü belirtir (`Date`).</span><span class="sxs-lookup"><span data-stu-id="0e950-104">`[DataType(DataType.Date)]`:  The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (`Date`).</span></span> <span data-ttu-id="0e950-105">Bu öznitelik ile:</span><span class="sxs-lookup"><span data-stu-id="0e950-105">With this attribute:</span></span>

  * <span data-ttu-id="0e950-106">Kullanıcının tarih alanı saat bilgilerini girmek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="0e950-106">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="0e950-107">Yalnızca tarih görüntülenen, olmayan zaman bilgilerdir.</span><span class="sxs-lookup"><span data-stu-id="0e950-107">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="0e950-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) bir sonraki öğreticide ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0e950-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>