<!-- THIS INCLUDE USED BY MVC AND RP -->
<span data-ttu-id="ece50-101">Aşağıdaki özellikleri `Movie` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ece50-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

<span data-ttu-id="ece50-102">`Movie` Sınıfı içerir:</span><span class="sxs-lookup"><span data-stu-id="ece50-102">The `Movie` class contains:</span></span>

* <span data-ttu-id="ece50-103">`ID` Alan veritabanı için birincil anahtar tarafından gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ece50-103">The `ID` field is required by the database for the primary key.</span></span>
* <span data-ttu-id="ece50-104">`[DataType(DataType.Date)]`:  [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) özniteliği (tarih) veri türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="ece50-104">`[DataType(DataType.Date)]`:  The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date).</span></span> <span data-ttu-id="ece50-105">Bu öznitelik ile:</span><span class="sxs-lookup"><span data-stu-id="ece50-105">With this attribute:</span></span>

  * <span data-ttu-id="ece50-106">Kullanıcının tarih alanı saat bilgilerini girmek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="ece50-106">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="ece50-107">Yalnızca tarih görüntülenen, olmayan zaman bilgilerdir.</span><span class="sxs-lookup"><span data-stu-id="ece50-107">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="ece50-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) bir sonraki öğreticide ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ece50-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>