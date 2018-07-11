---
title: EF Core - sıralama, filtreleme, sayfalama - 3, 10 ile ASP.NET Core MVC
author: rick-anderson
description: Bu öğreticide, sıralama, filtreleme ve sayfalama ASP.NET Core ve Entity Framework Core kullanan sayfa işlevselliği ekleyeceksiniz.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 1f80faf0e36332c28e8337ddc331cc8b4c4970d7
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38193955"
---
# <a name="aspnet-core-mvc-with-ef-core---sort-filter-paging---3-of-10"></a><span data-ttu-id="56fe8-103">EF Core - sıralama, filtreleme, sayfalama - 3, 10 ile ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="56fe8-103">ASP.NET Core MVC with EF Core - Sort, Filter, Paging - 3 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="56fe8-104">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="56fe8-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="56fe8-105">Contoso University örnek web uygulaması, Entity Framework Core ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="56fe8-106">Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](intro.md).</span><span class="sxs-lookup"><span data-stu-id="56fe8-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="56fe8-107">Önceki öğreticide, bir dizi web sayfaları için Öğrenci varlıkları temel CRUD işlemleri için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="56fe8-107">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="56fe8-108">Bu öğreticide, Öğrenciler dizin sayfasına sıralama, filtreleme ve sayfalama işlevselliğinin ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="56fe8-108">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="56fe8-109">Basit gruplandırma yapan bir sayfa da oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="56fe8-109">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="56fe8-110">Aşağıdaki çizim, hazır olduğunuzda sayfanın nasıl görüneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-110">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="56fe8-111">Sütun başlıkları, kullanıcının sütuna göre sıralamak için tıklayabileceği bağlantılar verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-111">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="56fe8-112">Bir sütun başlığına tekrar tekrar tıklayarak, artan veya azalan sıralama düzeni arasında geçiş yapar.</span><span class="sxs-lookup"><span data-stu-id="56fe8-112">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="56fe8-114">Öğrenciler dizin sayfasına Sütun sıralama bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="56fe8-114">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="56fe8-115">Öğrenci dizin sayfasına sıralama eklemek için değiştireceksiniz `Index` Öğrenciler denetleyicinin yöntemi ve kodu Öğrenci dizin görünümüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="56fe8-115">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="56fe8-116">İşlevleri sıralama için dizin yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="56fe8-116">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="56fe8-117">İçinde *StudentsController.cs*, değiştirin `Index` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="56fe8-117">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="56fe8-118">Bu kod alır bir `sortOrder` URL'ye sorgu dizesi parametresi.</span><span class="sxs-lookup"><span data-stu-id="56fe8-118">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="56fe8-119">Sorgu dizesi değerini eylem yönteminin bir parametresi olarak ASP.NET Core MVC tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="56fe8-119">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="56fe8-120">Parametre "Name" veya "Tarih", ardından isteğe bağlı olarak bir alt çizgi ve azalan düzende belirtmek için "desc" dizesi bir dize olur.</span><span class="sxs-lookup"><span data-stu-id="56fe8-120">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="56fe8-121">Varsayılan sıralama artan düzendedir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-121">The default sort order is ascending.</span></span>

<span data-ttu-id="56fe8-122">Dizin Sayfası istendi, ilk kez hiçbir sorgu dizesi yoktur.</span><span class="sxs-lookup"><span data-stu-id="56fe8-122">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="56fe8-123">Öğrenciler artan düzende başarısızlık durumunda tarafından belirlenen varsayılan olan soyadına göre görüntülenen `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="56fe8-123">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="56fe8-124">Kullanıcı uygun sütun başlığına köprü tıkladığında `sortOrder` değeri, sorgu dizesinde sağlanır.</span><span class="sxs-lookup"><span data-stu-id="56fe8-124">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="56fe8-125">İki `ViewData` öğelerini (NameSortParm ve DateSortParm) sütun başlığı köprüler uygun sorgu dize değerleri ile yapılandırmak için Görünüm tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="56fe8-125">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="56fe8-126">Üçlü deyimleri şunlardır.</span><span class="sxs-lookup"><span data-stu-id="56fe8-126">These are ternary statements.</span></span> <span data-ttu-id="56fe8-127">Olmadığını birincinin belirtir `sortOrder` parametresi null veya boş NameSortParm "name_desc için" olarak ayarlanmalıdır; Aksi takdirde, boş bir dize olarak ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="56fe8-127">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="56fe8-128">Bu iki deyimden sütun başlığı köprüler şu şekilde ayarlayın görünümün etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="56fe8-128">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="56fe8-129">Geçerli bir sıralama düzeni</span><span class="sxs-lookup"><span data-stu-id="56fe8-129">Current sort order</span></span>  | <span data-ttu-id="56fe8-130">Son adı köprü</span><span class="sxs-lookup"><span data-stu-id="56fe8-130">Last Name Hyperlink</span></span> | <span data-ttu-id="56fe8-131">Tarih köprü</span><span class="sxs-lookup"><span data-stu-id="56fe8-131">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="56fe8-132">Adı artan en son</span><span class="sxs-lookup"><span data-stu-id="56fe8-132">Last Name ascending</span></span>  | <span data-ttu-id="56fe8-133">descending</span><span class="sxs-lookup"><span data-stu-id="56fe8-133">descending</span></span>          | <span data-ttu-id="56fe8-134">ascending</span><span class="sxs-lookup"><span data-stu-id="56fe8-134">ascending</span></span>      |
| <span data-ttu-id="56fe8-135">Son azalan düzende ad</span><span class="sxs-lookup"><span data-stu-id="56fe8-135">Last Name descending</span></span> | <span data-ttu-id="56fe8-136">ascending</span><span class="sxs-lookup"><span data-stu-id="56fe8-136">ascending</span></span>           | <span data-ttu-id="56fe8-137">ascending</span><span class="sxs-lookup"><span data-stu-id="56fe8-137">ascending</span></span>      |
| <span data-ttu-id="56fe8-138">Artan tarihi</span><span class="sxs-lookup"><span data-stu-id="56fe8-138">Date ascending</span></span>       | <span data-ttu-id="56fe8-139">ascending</span><span class="sxs-lookup"><span data-stu-id="56fe8-139">ascending</span></span>           | <span data-ttu-id="56fe8-140">descending</span><span class="sxs-lookup"><span data-stu-id="56fe8-140">descending</span></span>     |
| <span data-ttu-id="56fe8-141">Azalan düzende tarihi</span><span class="sxs-lookup"><span data-stu-id="56fe8-141">Date descending</span></span>      | <span data-ttu-id="56fe8-142">ascending</span><span class="sxs-lookup"><span data-stu-id="56fe8-142">ascending</span></span>           | <span data-ttu-id="56fe8-143">ascending</span><span class="sxs-lookup"><span data-stu-id="56fe8-143">ascending</span></span>      |

<span data-ttu-id="56fe8-144">Yöntemi, LINQ to Entities göre sıralamak için sütun belirtmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="56fe8-144">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="56fe8-145">Kod oluşturur bir `IQueryable` switch deyiminden önce değişkeni değiştirir, bu, switch ifadesi ve çağrıları `ToListAsync` sonrasına `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="56fe8-145">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="56fe8-146">Ne zaman oluşturma ve değiştirme `IQueryable` değişkenleri, sorgu veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-146">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="56fe8-147">Sorgu dönüştürmeniz kadar yürütülen değil `IQueryable` nesne gibi bir yöntem çağırarak koleksiyonuna `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="56fe8-147">The query isn't executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="56fe8-148">Bu nedenle, bu kod, kadar yürütülmez tek bir sorgu sonuçları `return View` deyimi.</span><span class="sxs-lookup"><span data-stu-id="56fe8-148">Therefore, this code results in a single query that's not executed until the `return View` statement.</span></span>

<span data-ttu-id="56fe8-149">Bu kod, çok sayıda sütun ayrıntılı alabilir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-149">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="56fe8-150">[Bu serideki son öğretici](advanced.md#dynamic-linq) adını geçirmenize olanak tanıyan kodunun nasıl yazılacağını gösterir `OrderBy` bir dize değişkeni sütunu.</span><span class="sxs-lookup"><span data-stu-id="56fe8-150">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="56fe8-151">Öğrenci dizini görünümü için sütun başlığını köprüler ekleme</span><span class="sxs-lookup"><span data-stu-id="56fe8-151">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="56fe8-152">Değiştirin *Views/Students/Index.cshtml*, sütun başlığını köprüler eklemek için aşağıdaki kod ile.</span><span class="sxs-lookup"><span data-stu-id="56fe8-152">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="56fe8-153">Değiştirilen satırlar vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="56fe8-153">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="56fe8-154">Bilgiler, bu kodu kullanan `ViewData` uygun sorgu köprülerle ayarlamak için özellikler dize değerleri.</span><span class="sxs-lookup"><span data-stu-id="56fe8-154">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="56fe8-155">Uygulamayı çalıştırın, seçin **Öğrenciler** sekmesine ve tıklayın **Soyadı** ve **kayıt tarihi** sütun başlıkları, sıralama doğrulamak için çalışır.</span><span class="sxs-lookup"><span data-stu-id="56fe8-155">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Öğrenciler dizin sayfası adı sırayla](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="56fe8-157">Bir arama kutusu Öğrenciler dizin sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="56fe8-157">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="56fe8-158">Öğrenciler dizin sayfasına filtre eklemek için görünümü için metin kutusu ve bir Gönder düğmesi ekleyin ve karşılık gelen değişiklik `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="56fe8-158">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="56fe8-159">Metin kutusunda, ad ve soyadı alanları aramak için bir dize girin olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="56fe8-159">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="56fe8-160">Filtreleme işlevselliği dizin yöntemine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="56fe8-160">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="56fe8-161">İçinde *StudentsController.cs*, değiştirin `Index` yöntemini aşağıdaki kodla (değişiklikleri vurgulanır).</span><span class="sxs-lookup"><span data-stu-id="56fe8-161">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="56fe8-162">Eklediğiniz bir `searchString` parametresi `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="56fe8-162">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="56fe8-163">Dizin görünümüne ekleyeceksiniz bir metin kutusundan arama dizesi değeri alındı.</span><span class="sxs-lookup"><span data-stu-id="56fe8-163">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="56fe8-164">LINQ deyime where ekledik, ad ve Soyadı arama dizesini içeren Öğrenciler seçer yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="56fe8-164">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="56fe8-165">Where ekler deyimi yan tümcesi yalnızca aramak için bir değer ise yürütülür.</span><span class="sxs-lookup"><span data-stu-id="56fe8-165">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="56fe8-166">Burada, aradığınız `Where` metodunda bir `IQueryable` nesne ve filtre, sunucuda işlenir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-166">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="56fe8-167">Bazı senaryolarda, çağırma `Where` yöntemi olarak bir genişletme yöntemi bir bellek içi koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="56fe8-167">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="56fe8-168">(Örneğin, başvuru değiştirmeniz varsayalım `_context.Students` bir EF, bu nedenle, bunun yerine `DbSet` döndüren bir depo yöntemin başvurduğu bir `IEnumerable` koleksiyonu.) Sonuç, normalde aynı kalır ancak bazı durumlarda farklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-168">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="56fe8-169">Örneğin, .NET Framework uygulamasını `Contains` yöntemi varsayılan olarak büyük küçük harfe duyarlı bir karşılaştırma yapar, ancak SQL Server'da bu SQL Server örneğinin harmanlama ayarı tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-169">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="56fe8-170">Bu ayar için büyük küçük harf duyarsız varsayar.</span><span class="sxs-lookup"><span data-stu-id="56fe8-170">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="56fe8-171">Çağırabilir `ToUpper` test açıkça duyarlı hale getirmek için yöntemi: *nerede (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*.</span><span class="sxs-lookup"><span data-stu-id="56fe8-171">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="56fe8-172">Emin döndüren bir depoyu daha sonra kullanmak için kodu değiştirirseniz sonuçları aynı kalmasını bir `IEnumerable` koleksiyonu yerine bir `IQueryable` nesne.</span><span class="sxs-lookup"><span data-stu-id="56fe8-172">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="56fe8-173">(Çağırdığınızda `Contains` metodunda bir `IEnumerable` koleksiyonu, .NET Framework uygulaması alın; çağırdığınızda, üzerinde bir `IQueryable` nesne veritabanı sağlayıcısı uygulamasını edinin.) Ancak, bu çözüm için bir performans cezası yoktur.</span><span class="sxs-lookup"><span data-stu-id="56fe8-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there's a performance penalty for this solution.</span></span> <span data-ttu-id="56fe8-174">`ToUpper` Kod TSQL seçin deyiminin WHERE yan tümcesinde bir işlev yerleştirecek.</span><span class="sxs-lookup"><span data-stu-id="56fe8-174">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="56fe8-175">Bu, iyileştirici dizin kullanmasını önler.</span><span class="sxs-lookup"><span data-stu-id="56fe8-175">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="56fe8-176">SQL çoğunlukla olarak büyük küçük harf duyarsız yüklü olduğu düşünüldüğünde, kaçınmanız en iyisidir `ToUpper` büyük/küçük harfe veri deposuna aktarana kadar kodu.</span><span class="sxs-lookup"><span data-stu-id="56fe8-176">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="56fe8-177">Bir arama kutusu Öğrenci dizini görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="56fe8-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="56fe8-178">İçinde *Views/Student/Index.cshtml*, resim yazısı, bir metin kutusu oluşturmak için bir etiket hemen açılış tablo önce vurgulanmış kodu ekleyin ve bir **arama** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="56fe8-178">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="56fe8-179">Bu kod `<form>` [etiket Yardımcısı](xref:mvc/views/tag-helpers/intro) arama metin kutusu ve düğme.</span><span class="sxs-lookup"><span data-stu-id="56fe8-179">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="56fe8-180">Varsayılan olarak, `<form>` etiketi Yardımcısı parametreleri HTTP ileti gövdesini ve URL'yi içinde değil sorgu dizeleri geçirilir, yani bir GÖNDERİ ile form verileri gönderir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-180">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="56fe8-181">HTTP GET belirttiğinizde, form verilerini URL'ye sorgu dizeleri kullanıcıların yer işareti URL'si sağlayan geçirilir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-181">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="56fe8-182">Kullanmanız gereken W3C yönergeleri önerilen eylemi bir güncelleştirmeye neden olmaz, alın.</span><span class="sxs-lookup"><span data-stu-id="56fe8-182">The W3C guidelines recommend that you should use GET when the action doesn't result in an update.</span></span>

<span data-ttu-id="56fe8-183">Uygulamayı çalıştırın, seçin **Öğrenciler** sekmesinde, bir arama dizesi girin ve filtreleme çalışıp çalışmadığını doğrulamak için Ara'yı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="56fe8-183">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![Filtreleme ile Öğrenciler dizin sayfası](sort-filter-page/_static/filtering.png)

<span data-ttu-id="56fe8-185">URL arama dizesi içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="56fe8-185">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="56fe8-186">Bu sayfaya yer işareti, yer işareti kullandığınızda filtrelenmiş liste elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="56fe8-186">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="56fe8-187">Ekleme `method="get"` için `form` etikettir neyin neden oluşturulacak sorgu dizesi.</span><span class="sxs-lookup"><span data-stu-id="56fe8-187">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="56fe8-188">Bu aşamada bir sütun başlığını sıralama bağlantı tıklarsanız girdiğiniz filtre değeri kaybedeceksiniz **arama** kutusu.</span><span class="sxs-lookup"><span data-stu-id="56fe8-188">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="56fe8-189">Sonraki bölümde, çözeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="56fe8-189">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="56fe8-190">Disk belleği işlevsellik Öğrenciler dizin sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="56fe8-190">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="56fe8-191">Disk belleği Öğrenciler dizin sayfasına eklemek için oluşturacağınız bir `PaginatedList` kullanan sınıf `Skip` ve `Take` yerine her zaman tablonun tüm satırlarının alınırken sunucu üzerindeki verileri filtrelemek için deyimleri.</span><span class="sxs-lookup"><span data-stu-id="56fe8-191">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="56fe8-192">Sonra ek değişiklik yapacaksınız `Index` yöntemi ve disk belleği düğmeleri ekleme `Index` görünümü.</span><span class="sxs-lookup"><span data-stu-id="56fe8-192">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="56fe8-193">Aşağıdaki çizim, disk belleği düğme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-193">The following illustration shows the paging buttons.</span></span>

![Disk belleği bağlantılarla sayfası Öğrenciler dizin](sort-filter-page/_static/paging.png)

<span data-ttu-id="56fe8-195">Proje klasöründe oluşturma `PaginatedList.cs`ve sonra şablon kodunu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="56fe8-195">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="56fe8-196">`CreateAsync` Yöntemi bu kodda sayfa boyutu ve sayfa numarası alır ve uygun geçerlidir `Skip` ve `Take` deyimleriyle `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="56fe8-196">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="56fe8-197">Zaman `ToListAsync` üzerinde çağrılır `IQueryable`, yalnızca istenen sayfayı içeren bir liste döndürür.</span><span class="sxs-lookup"><span data-stu-id="56fe8-197">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="56fe8-198">Özellikleri `HasPreviousPage` ve `HasNextPage` etkinleştirmek veya devre dışı bırakmak için kullanılabilir **önceki** ve **sonraki** düğmeleri sayfalama.</span><span class="sxs-lookup"><span data-stu-id="56fe8-198">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="56fe8-199">A `CreateAsync` yöntemi oluşturmak için bir oluşturucu yerine kullanılır `PaginatedList<T>` oluşturucular zaman uyumsuz kod çalıştırılamaz nesne.</span><span class="sxs-lookup"><span data-stu-id="56fe8-199">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="56fe8-200">Sayfalama işlevselliğinin dizin yöntemine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="56fe8-200">Add paging functionality to the Index method</span></span>

<span data-ttu-id="56fe8-201">İçinde *StudentsController.cs*, değiştirin `Index` yöntemini aşağıdaki kod ile.</span><span class="sxs-lookup"><span data-stu-id="56fe8-201">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="56fe8-202">Bu kod, yöntem imzası için sayfa sayı parametresi, geçerli bir sıralama sipariş parametresi ve geçerli bir filtre parametresi ekler.</span><span class="sxs-lookup"><span data-stu-id="56fe8-202">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="56fe8-203">Kullanıcı bir disk belleği veya bağlantı sıralama taşınmadığından seçeneğine tıkladıysanız, tüm parametreleri null veya ilk kez sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="56fe8-204">Disk belleği bağlantıya tıkladıysanız, sayfa değişkenini görüntülemek için sayfa numarasını içerir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-204">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="56fe8-205">`ViewData` CurrentSort adlı bir öğe sıralama sırasında disk belleği aynı tutulabilmesi için disk belleği bağlantıları bu eklenmelidir çünkü geçerli sıralama düzenini görünümüyle sağlar.</span><span class="sxs-lookup"><span data-stu-id="56fe8-205">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="56fe8-206">`ViewData` GeçerliFiltre adlı öğesi, geçerli bir filtre dizesi görünümüyle sağlar.</span><span class="sxs-lookup"><span data-stu-id="56fe8-206">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="56fe8-207">Bu değer sırasında disk belleği filtre ayarlarını sürdürmek için disk belleği bağlantıları eklenmelidir ve sayfası görüntülendiğinde, metin kutusuna geri yüklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="56fe8-208">Arama dizesi sırasında disk belleği değiştirilirse, yeni filtre görüntülemek için farklı veri kaybına neden çünkü sayfa 1'e sıfırlanması gereken.</span><span class="sxs-lookup"><span data-stu-id="56fe8-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="56fe8-209">Arama dizesi, metin kutusuna girilen değer ve Gönder düğmesine basıldığında değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-209">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="56fe8-210">Bu durumda, `searchString` parametresi null değil.</span><span class="sxs-lookup"><span data-stu-id="56fe8-210">In that case, the `searchString` parameter isn't null.</span></span>

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="56fe8-211">Sonunda `Index` yöntemi `PaginatedList.CreateAsync` yöntemi disk belleği destekleyen bir koleksiyon türü Öğrenci tek sayfalık Öğrenci sorgu dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="56fe8-211">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="56fe8-212">Öğrenciler, tek sayfalık ardından görünüme iletilir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-212">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="56fe8-213">`PaginatedList.CreateAsync` Yöntemi, bir sayfa numarasını alır.</span><span class="sxs-lookup"><span data-stu-id="56fe8-213">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="56fe8-214">İki soru işareti null birleşim işleci temsil eder.</span><span class="sxs-lookup"><span data-stu-id="56fe8-214">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="56fe8-215">Boş değer atanabilir bir tür için varsayılan bir değer null birleşim işleci tanımlar; ifade `(page ?? 1)` anlamına gelir dönüş değerini `page` bir değere sahip veya 1 döndürür, `page` null.</span><span class="sxs-lookup"><span data-stu-id="56fe8-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="56fe8-216">Öğrenci dizini görünümü için disk belleği bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="56fe8-216">Add paging links to the Student Index view</span></span>

<span data-ttu-id="56fe8-217">İçinde *Views/Students/Index.cshtml*, mevcut kodu şu kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="56fe8-217">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="56fe8-218">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="56fe8-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="56fe8-219">`@model` Sayfanın üstündeki deyimi belirtir görünüme artık alır bir `PaginatedList<T>` yerine Nesne bir `List<T>` nesne.</span><span class="sxs-lookup"><span data-stu-id="56fe8-219">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="56fe8-220">Sütun üst bilgisi bağlantıları, kullanıcının içinde filtre sonuçlarını sıralayabilirsiniz, böylece geçerli arama dizesinin denetleyiciye geçirilecek sorgu dizesi kullanın:</span><span class="sxs-lookup"><span data-stu-id="56fe8-220">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="56fe8-221">Disk belleği düğmeler tarafından etiket Yardımcıları görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="56fe8-221">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="56fe8-222">Uygulamayı çalıştırın ve öğrenciler sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="56fe8-222">Run the app and go to the Students page.</span></span>

![Disk belleği bağlantılarla sayfası Öğrenciler dizin](sort-filter-page/_static/paging.png)

<span data-ttu-id="56fe8-224">Disk belleği works emin olmak için farklı sıralamalar sayfalama bağlantıları tıklatın.</span><span class="sxs-lookup"><span data-stu-id="56fe8-224">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="56fe8-225">Ardından bir arama dizesi girin ve yeniden disk belleği de doğru sıralama ve filtreleme ile çalıştığını doğrulamak için disk belleği'ni deneyin.</span><span class="sxs-lookup"><span data-stu-id="56fe8-225">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="56fe8-226">Öğrenci istatistiklerini gösteren bir hakkında sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="56fe8-226">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="56fe8-227">Contoso University Web sitesinin için **hakkında** sayfasında, her kayıt tarihi için kaç Öğrenciler kaydedilmiş görüntüleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="56fe8-227">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="56fe8-228">Bu gruplar üzerinde gruplandırma ve basit hesaplama gerektirir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-228">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="56fe8-229">Bunu yapmak için aşağıdakileri:</span><span class="sxs-lookup"><span data-stu-id="56fe8-229">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="56fe8-230">Görünüme iletmek için gereken verileri için bir görünüm modeli sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="56fe8-230">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="56fe8-231">Giriş denetleyicisine hakkında yöntemi değiştirin.</span><span class="sxs-lookup"><span data-stu-id="56fe8-231">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="56fe8-232">Hakkında görünümü değiştirin.</span><span class="sxs-lookup"><span data-stu-id="56fe8-232">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="56fe8-233">Görünüm modeli oluşturun</span><span class="sxs-lookup"><span data-stu-id="56fe8-233">Create the view model</span></span>

<span data-ttu-id="56fe8-234">Oluşturma bir *SchoolViewModels* klasöründe *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="56fe8-234">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="56fe8-235">Yeni klasörde bir sınıf dosyası ekleyin *EnrollmentDateGroup.cs* ve şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="56fe8-235">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="56fe8-236">Giriş denetleyicisini değiştirmek</span><span class="sxs-lookup"><span data-stu-id="56fe8-236">Modify the Home Controller</span></span>

<span data-ttu-id="56fe8-237">İçinde *HomeController.cs*, aşağıdaki using deyimlerini dosyanın üstüne:</span><span class="sxs-lookup"><span data-stu-id="56fe8-237">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="56fe8-238">Hemen sınıfı için açılış kaşlı ayracından sonra veritabanı bağlamı için bir sınıf değişkeni ekleyin ve ASP.NET Core DI bağlam örneğini alın:</span><span class="sxs-lookup"><span data-stu-id="56fe8-238">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="56fe8-239">Değiştirin `About` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="56fe8-239">Replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="56fe8-240">LINQ deyiminden Öğrenci varlıkları kayıt tarihe göre gruplar, her grupta varlık sayısını hesaplar ve sonuçları bir koleksiyonda depolar `EnrollmentDateGroup` model nesneleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="56fe8-240">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE]
> <span data-ttu-id="56fe8-241">Entity Framework Core 1.0 sürümünde, sonuç kümesinin tamamı istemciye döndürülür ve gruplandırma istemcide gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-241">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="56fe8-242">Bazı senaryolarda bu performans sorunlarını oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="56fe8-242">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="56fe8-243">Veri hacimleri üretim performansını test edin ve gerekirse, ham SQL sunucu üzerinde gruplandırma yapmak için kullanın emin olun.</span><span class="sxs-lookup"><span data-stu-id="56fe8-243">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="56fe8-244">Ham SQL kullanma hakkında daha fazla bilgi için bkz: [bu serinin son öğreticide](advanced.md).</span><span class="sxs-lookup"><span data-stu-id="56fe8-244">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="56fe8-245">Değiştirme görünümü hakkında</span><span class="sxs-lookup"><span data-stu-id="56fe8-245">Modify the About View</span></span>

<span data-ttu-id="56fe8-246">Değiştirin *Views/Home/About.cshtml* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="56fe8-246">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="56fe8-247">Uygulamayı çalıştırın ve hakkında sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="56fe8-247">Run the app and go to the About page.</span></span> <span data-ttu-id="56fe8-248">Bir tablodaki her kayıt tarihi için Öğrenci sayısı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="56fe8-248">The count of students for each enrollment date is displayed in a table.</span></span>

![Sayfa hakkında](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="56fe8-250">Özet</span><span class="sxs-lookup"><span data-stu-id="56fe8-250">Summary</span></span>

<span data-ttu-id="56fe8-251">Bu öğreticide, sıralama, filtreleme, sayfalama ve gruplandırma gerçekleştirmeyi öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="56fe8-251">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="56fe8-252">Sonraki öğreticide, veri modeli değişikliklerini geçişleri kullanarak nasıl ele alınacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="56fe8-252">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="56fe8-253">[Önceki](crud.md)
> [İleri](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="56fe8-253">[Previous](crud.md)
[Next](migrations.md)</span></span>
