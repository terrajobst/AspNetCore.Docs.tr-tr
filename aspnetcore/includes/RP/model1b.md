<!-- THIS INCLUDE USED BY MVC AND RP -->
<span data-ttu-id="86fc9-101">Aşağıdaki özellikleri `Movie` sınıfına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="86fc9-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

<span data-ttu-id="86fc9-102">`Movie` sınıfı şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="86fc9-102">The `Movie` class contains:</span></span>

* <span data-ttu-id="86fc9-103">`ID` alanı birincil anahtar için veritabanı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="86fc9-103">The `ID` field is required by the database for the primary key.</span></span>
* <span data-ttu-id="86fc9-104">`[DataType(DataType.Date)]`: [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) özniteliği verilerin türünü belirtir (Tarih).</span><span class="sxs-lookup"><span data-stu-id="86fc9-104">`[DataType(DataType.Date)]`:  The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date).</span></span> <span data-ttu-id="86fc9-105">Bu öznitelikle:</span><span class="sxs-lookup"><span data-stu-id="86fc9-105">With this attribute:</span></span>

  * <span data-ttu-id="86fc9-106">Kullanıcının Tarih alanına saat bilgilerini girmesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="86fc9-106">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="86fc9-107">Zaman bilgisi değil yalnızca tarih görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="86fc9-107">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="86fc9-108">[Veri açıklamaları](/dotnet/api/system.componentmodel.dataannotations) sonraki bir öğreticide ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="86fc9-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>
