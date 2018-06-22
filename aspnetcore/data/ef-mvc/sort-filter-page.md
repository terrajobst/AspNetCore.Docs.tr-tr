---
title: ASP.NET Core MVC EF çekirdek - sıralama, filtre, disk belleği - 10 3 ile
author: rick-anderson
description: Bu öğreticide, sıralama, filtreleme ve ASP.NET Core ve Entity Framework Çekirdek kullanarak sayfa için işlevsellik disk belleği eklemeniz.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 34097eacad16c0ffb989efb3b6a8656be4a076cd
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273656"
---
# <a name="aspnet-core-mvc-with-ef-core---sort-filter-paging---3-of-10"></a><span data-ttu-id="21f2e-103">ASP.NET Core MVC EF çekirdek - sıralama, filtre, disk belleği - 10 3 ile</span><span class="sxs-lookup"><span data-stu-id="21f2e-103">ASP.NET Core MVC with EF Core - Sort, Filter, Paging - 3 of 10</span></span>

<span data-ttu-id="21f2e-104">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="21f2e-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="21f2e-105">Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="21f2e-106">Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).</span><span class="sxs-lookup"><span data-stu-id="21f2e-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="21f2e-107">Önceki öğreticide, bir dizi web sayfası Öğrenci varlıklar için temel CRUD işlemleri için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="21f2e-107">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="21f2e-108">Bu öğreticide Öğrenciler dizin sayfasına sıralama, filtreleme ve disk belleği işlevselliği ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="21f2e-108">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="21f2e-109">Ayrıca, basit gruplandırma yapan bir sayfa oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="21f2e-109">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="21f2e-110">Aşağıdaki çizimde tamamladığınızda sayfa nasıl görüneceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-110">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="21f2e-111">Sütun başlıkları kullanıcının sütuna göre sıralamak için tıklatabileceği bağlantı bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="21f2e-111">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="21f2e-112">Bir sütun başlığına sürekli olarak artan veya azalan sıralama düzeni arasında geçiş yapar.</span><span class="sxs-lookup"><span data-stu-id="21f2e-112">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="21f2e-114">Öğrenciler dizin sayfasına Sütun sıralama bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="21f2e-114">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="21f2e-115">Öğrenci dizin sayfasına sıralama eklemek için değiştireceğiz `Index` yöntemi Öğrenciler denetleyicisinin ve kod Öğrenci dizin görünümüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="21f2e-115">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="21f2e-116">İşlevleri sıralama için dizin yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="21f2e-116">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="21f2e-117">İçinde *StudentsController.cs*, yerine `Index` aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="21f2e-117">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="21f2e-118">Bu kod alan bir `sortOrder` URL'deki sorgu dizesi parametresi.</span><span class="sxs-lookup"><span data-stu-id="21f2e-118">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="21f2e-119">Sorgu dizesi değerini eylem yönteminin bir parametresi olarak ASP.NET Core MVC tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="21f2e-119">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="21f2e-120">Parametre "Ad" veya "Tarih", ardından isteğe bağlı olarak bir alt çizgi ve azalan belirtmek için "desc" dizesi olan bir dize olur.</span><span class="sxs-lookup"><span data-stu-id="21f2e-120">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="21f2e-121">Varsayılan sıralama düzeni artan.</span><span class="sxs-lookup"><span data-stu-id="21f2e-121">The default sort order is ascending.</span></span>

<span data-ttu-id="21f2e-122">Dizin sayfası istenen ilk kez hiçbir sorgu dizesi yok.</span><span class="sxs-lookup"><span data-stu-id="21f2e-122">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="21f2e-123">Öğrenciler artan düzende başarısızlık durumunda tarafından belirlenen varsayılan değer olan son adıyla görüntülenir `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="21f2e-123">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="21f2e-124">Kullanıcı uygun sütun başlığını köprü tıkladığında `sortOrder` değeri, sorgu dizesinde sağlanır.</span><span class="sxs-lookup"><span data-stu-id="21f2e-124">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="21f2e-125">İki `ViewData` öğeleri (NameSortParm ve DateSortParm) uygun sorgu dizesi değerlerini sütun başlığını köprüler yapılandırmak için Görünüm tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="21f2e-125">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="21f2e-126">Üçlü deyimleri bunlar.</span><span class="sxs-lookup"><span data-stu-id="21f2e-126">These are ternary statements.</span></span> <span data-ttu-id="21f2e-127">Birinci anlama gelir `sortOrder` parametresi null veya boş NameSortParm "name_desc" olarak ayarlanmalıdır; Aksi halde, boş bir dize olarak ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="21f2e-127">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="21f2e-128">Bu iki ifade sütun başlığını köprüler şu şekilde ayarlamak görünümü etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="21f2e-128">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="21f2e-129">Geçerli bir sıralama düzeni</span><span class="sxs-lookup"><span data-stu-id="21f2e-129">Current sort order</span></span>  | <span data-ttu-id="21f2e-130">Son adı köprü</span><span class="sxs-lookup"><span data-stu-id="21f2e-130">Last Name Hyperlink</span></span> | <span data-ttu-id="21f2e-131">Tarih köprü</span><span class="sxs-lookup"><span data-stu-id="21f2e-131">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="21f2e-132">Ad artan en son</span><span class="sxs-lookup"><span data-stu-id="21f2e-132">Last Name ascending</span></span>  | <span data-ttu-id="21f2e-133">descending</span><span class="sxs-lookup"><span data-stu-id="21f2e-133">descending</span></span>          | <span data-ttu-id="21f2e-134">ascending</span><span class="sxs-lookup"><span data-stu-id="21f2e-134">ascending</span></span>      |
| <span data-ttu-id="21f2e-135">Ad azalan en son</span><span class="sxs-lookup"><span data-stu-id="21f2e-135">Last Name descending</span></span> | <span data-ttu-id="21f2e-136">ascending</span><span class="sxs-lookup"><span data-stu-id="21f2e-136">ascending</span></span>           | <span data-ttu-id="21f2e-137">ascending</span><span class="sxs-lookup"><span data-stu-id="21f2e-137">ascending</span></span>      |
| <span data-ttu-id="21f2e-138">Artan tarihi</span><span class="sxs-lookup"><span data-stu-id="21f2e-138">Date ascending</span></span>       | <span data-ttu-id="21f2e-139">ascending</span><span class="sxs-lookup"><span data-stu-id="21f2e-139">ascending</span></span>           | <span data-ttu-id="21f2e-140">descending</span><span class="sxs-lookup"><span data-stu-id="21f2e-140">descending</span></span>     |
| <span data-ttu-id="21f2e-141">Azalan tarihi</span><span class="sxs-lookup"><span data-stu-id="21f2e-141">Date descending</span></span>      | <span data-ttu-id="21f2e-142">ascending</span><span class="sxs-lookup"><span data-stu-id="21f2e-142">ascending</span></span>           | <span data-ttu-id="21f2e-143">ascending</span><span class="sxs-lookup"><span data-stu-id="21f2e-143">ascending</span></span>      |

<span data-ttu-id="21f2e-144">Yöntemi, LINQ to Entities göre sıralamak için sütun belirlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="21f2e-144">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="21f2e-145">Kod oluşturur bir `IQueryable` değişken switch deyimi önce değiştirir, bunu, switch deyimi ve çağrıları `ToListAsync` sonra yöntemi `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="21f2e-145">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="21f2e-146">Ne zaman oluşturma ve değiştirme `IQueryable` değişkenleri, sorgu veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-146">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="21f2e-147">Sorgu, dönüştürülünceye kadar yürütülür değil `IQueryable` gibi bir yöntemini çağırarak bir koleksiyon nesnesine `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="21f2e-147">The query isn't executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="21f2e-148">Bu nedenle, bu kod kadar yürütülmedi tek bir sorgu sonuçları `return View` deyimi.</span><span class="sxs-lookup"><span data-stu-id="21f2e-148">Therefore, this code results in a single query that's not executed until the `return View` statement.</span></span>

<span data-ttu-id="21f2e-149">Bu kod, çok sayıda sütun ayrıntılı alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="21f2e-149">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="21f2e-150">[Bu serideki son öğretici](advanced.md#dynamic-linq) adını geçirmenize olanak verir kodunun nasıl yazılacağını gösterir `OrderBy` bir dize değişkeni sütununda.</span><span class="sxs-lookup"><span data-stu-id="21f2e-150">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="21f2e-151">Sütun başlık köprüleri için Öğrenci dizini görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="21f2e-151">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="21f2e-152">Kodla *Views/Students/Index.cshtml*, sütun başlığını köprüler eklemek için aşağıdaki kod ile.</span><span class="sxs-lookup"><span data-stu-id="21f2e-152">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="21f2e-153">Değiştirilen satırları vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="21f2e-153">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="21f2e-154">Bu kod bilgileri kullanan `ViewData` uygun sorguyla köprüler ayarlamak için özellikler dize değerleri.</span><span class="sxs-lookup"><span data-stu-id="21f2e-154">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="21f2e-155">Uygulama, belirleyin **Öğrenciler** sekmesine ve tıklayın **Soyadı** ve **kayıt tarihi** Bu sıralama doğrulamak için sütun başlıkları çalışır.</span><span class="sxs-lookup"><span data-stu-id="21f2e-155">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Ad sırayla Öğrenciler dizin sayfası](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="21f2e-157">Bir arama kutusu Öğrenciler dizin sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="21f2e-157">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="21f2e-158">Öğrenciler dizin sayfasına filtre eklemek için metin kutusu ve bir gönderme düğmesi görünümüne ekleyin ve karşılık gelen değişiklik `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="21f2e-158">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="21f2e-159">Metin kutusunda, ad ve soyadı alanları aramak için bir dize girin olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="21f2e-159">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="21f2e-160">Filtreleme işlevselliği dizin yöntemine ekleyin</span><span class="sxs-lookup"><span data-stu-id="21f2e-160">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="21f2e-161">İçinde *StudentsController.cs*, yerine `Index` (değişiklikleri vurgulanır) aşağıdaki kod ile yöntemi.</span><span class="sxs-lookup"><span data-stu-id="21f2e-161">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="21f2e-162">Eklediğiniz bir `searchString` parametresi `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="21f2e-162">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="21f2e-163">Bir metin kutusundan dizin görünümüne ekleyeceksiniz arama dizesi değeri alındı.</span><span class="sxs-lookup"><span data-stu-id="21f2e-163">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="21f2e-164">LINQ ifadesi bir where eklediğiniz yalnızca, ad ve Soyadı arama dizesini içeren Öğrenciler seçer yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="21f2e-164">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="21f2e-165">Where ekler deyimi yan tümcesi yalnızca arama için bir değer ise gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-165">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="21f2e-166">Burada, aradığınız `Where` yöntemi bir `IQueryable` nesne ve filtre sunucuda işlenir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-166">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="21f2e-167">Bazı senaryolarda, çağırma `Where` yöntemi bir bellek içi koleksiyonda bir genişletme yöntemi olarak.</span><span class="sxs-lookup"><span data-stu-id="21f2e-167">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="21f2e-168">(Örneğin, referansını değiştirme varsayalım `_context.Students` bir EF, bu nedenle, bunun yerine `DbSet` döndüren bir depo yöntemi başvurduğu bir `IEnumerable` koleksiyonu.) Sonuç normalde aynı kalır ancak bazı durumlarda farklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-168">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="21f2e-169">Örneğin, .NET Framework uygulamasını `Contains` yöntemi, varsayılan olarak büyük küçük harfe duyarlı karşılaştırma gerçekleştirir, ancak SQL Server'da bu SQL Server örneği harmanlama ayarı tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-169">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="21f2e-170">Bu ayar varsayılan olarak çok büyük küçük harfe duyarsızdır.</span><span class="sxs-lookup"><span data-stu-id="21f2e-170">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="21f2e-171">Çağrı `ToUpper` yöntemini test açıkça büyük küçük harf duyarsız sağlamak için: *nerede (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*.</span><span class="sxs-lookup"><span data-stu-id="21f2e-171">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="21f2e-172">Emin kodunu döndüren bir depo daha sonra değiştirirseniz, sonuçları aynı kalmasını bir `IEnumerable` koleksiyon yerine bir `IQueryable` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="21f2e-172">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="21f2e-173">(Çağırdığınızda `Contains` yöntemi bir `IEnumerable` koleksiyonu, .NET Framework uygulamasını alın; çağırdığınızda, üzerinde bir `IQueryable` nesnesi, veritabanı sağlayıcısı uygulaması alın.) Ancak, bulunmaktadır performans Bu çözüm için.</span><span class="sxs-lookup"><span data-stu-id="21f2e-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there's a performance penalty for this solution.</span></span> <span data-ttu-id="21f2e-174">`ToUpper` Kod TSQL SELECT deyimi WHERE yan tümcesinde bir işlev yerleştirecek.</span><span class="sxs-lookup"><span data-stu-id="21f2e-174">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="21f2e-175">Bir dizini kullanarak, iyileştirici engeller.</span><span class="sxs-lookup"><span data-stu-id="21f2e-175">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="21f2e-176">SQL çoğunlukla olarak büyük küçük harf duyarsız yüklendi Bunu önlemek en iyi düşünüldüğünde `ToUpper` büyük küçük harfe duyarlı veri deposuna geçirene kadar kod.</span><span class="sxs-lookup"><span data-stu-id="21f2e-176">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="21f2e-177">Bir arama kutusu Öğrenci dizin görünümüne ekleyin</span><span class="sxs-lookup"><span data-stu-id="21f2e-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="21f2e-178">İçinde *Views/Student/Index.cshtml*, hemen açılış tablo önce etiketi resim yazısı, bir metin kutusu oluşturma için vurgulanmış kodu ekleyin ve bir **arama** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="21f2e-178">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="21f2e-179">Bu kodu kullanır `<form>` [yardımcı etiketi](xref:mvc/views/tag-helpers/intro) düğmesi ve arama metin kutusuna eklemek için.</span><span class="sxs-lookup"><span data-stu-id="21f2e-179">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="21f2e-180">Varsayılan olarak, `<form>` etiket Yardımcısı parametreleri HTTP ileti gövdesi yer alan ve URL sorgu dizeleri geçirilir, yani bir POST ile form verileri gönderir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-180">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="21f2e-181">HTTP GET belirttiğinizde, form verilerini URL'de sorgu dizeleri kullanıcıların URL yer işareti sağlayan geçirilir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-181">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="21f2e-182">Kullanmanız gereken W3C yönergeleri önerilen eylemi bir güncelleştirmede elde edemezseniz, alın.</span><span class="sxs-lookup"><span data-stu-id="21f2e-182">The W3C guidelines recommend that you should use GET when the action doesn't result in an update.</span></span>

<span data-ttu-id="21f2e-183">Uygulama, belirleyin **Öğrenciler** sekmesinde, bir arama dizesi girin ve filtreleme çalıştığını doğrulamak için Ara'yı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="21f2e-183">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![Filtreleme ile Öğrenciler dizin sayfası](sort-filter-page/_static/filtering.png)

<span data-ttu-id="21f2e-185">URL arama dizesi içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="21f2e-185">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="21f2e-186">Bu sayfaya yer işareti varsa, yer işareti kullandığınızda filtrelenmiş liste elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="21f2e-186">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="21f2e-187">Ekleme `method="get"` için `form` etiketi olan neyin neden olduğunu oluşturulacak sorgu dizesi.</span><span class="sxs-lookup"><span data-stu-id="21f2e-187">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="21f2e-188">Bu aşamada, bir sütun başlığını sıralama bağlantısını tıklatırsanız girdiğiniz filtre değeri kaybedersiniz **arama** kutusu.</span><span class="sxs-lookup"><span data-stu-id="21f2e-188">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="21f2e-189">Sonraki bölümde düzeltmesi.</span><span class="sxs-lookup"><span data-stu-id="21f2e-189">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="21f2e-190">Sayfalama işlevselliğinin Öğrenciler dizin sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="21f2e-190">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="21f2e-191">Disk belleği Öğrenciler dizin sayfasına eklemek için oluşturacağınız bir `PaginatedList` kullanan sınıfı `Skip` ve `Take` her zaman tablonun tüm satırlarının almak yerine sunucusundaki verileri filtrelemek için deyimleri.</span><span class="sxs-lookup"><span data-stu-id="21f2e-191">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="21f2e-192">Ek değişiklikler hale getireceğiz sonra `Index` yöntemi ve disk belleği düğmelere ekleme `Index` görünümü.</span><span class="sxs-lookup"><span data-stu-id="21f2e-192">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="21f2e-193">Aşağıdaki çizimde, disk belleği düğmeleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-193">The following illustration shows the paging buttons.</span></span>

![disk belleği bağlantılarla Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

<span data-ttu-id="21f2e-195">Proje klasöründe oluşturma `PaginatedList.cs`ve ardından şablon kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="21f2e-195">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="21f2e-196">`CreateAsync` Yöntemi bu kodda sayfa boyutu ve sayfa numarasını alır ve uygun geçerlidir `Skip` ve `Take` deyimlerini `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="21f2e-196">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="21f2e-197">Zaman `ToListAsync` üzerinde adlı `IQueryable`, yalnızca istenen sayfa içeren bir liste döndürür.</span><span class="sxs-lookup"><span data-stu-id="21f2e-197">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="21f2e-198">Özellikler `HasPreviousPage` ve `HasNextPage` etkinleştirmek veya devre dışı bırakmak için kullanılan **önceki** ve **sonraki** düğmeleri disk belleği.</span><span class="sxs-lookup"><span data-stu-id="21f2e-198">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="21f2e-199">A `CreateAsync` yöntemi yerine bir oluşturucu oluşturmak için kullanılan `PaginatedList<T>` zaman uyumsuz kod oluşturucuları çalışamadığı nesne.</span><span class="sxs-lookup"><span data-stu-id="21f2e-199">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="21f2e-200">Sayfalama işlevselliğinin dizin yöntemine ekleyin</span><span class="sxs-lookup"><span data-stu-id="21f2e-200">Add paging functionality to the Index method</span></span>

<span data-ttu-id="21f2e-201">İçinde *StudentsController.cs*, yerine `Index` aşağıdaki kod ile yöntemi.</span><span class="sxs-lookup"><span data-stu-id="21f2e-201">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="21f2e-202">Bu kod, yöntem imzası sayfa numarası parametre, geçerli bir sıralama sipariş parametresi ve geçerli bir filtre parametresi ekler.</span><span class="sxs-lookup"><span data-stu-id="21f2e-202">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="21f2e-203">Kullanıcı bir disk belleği veya bağlantı sıralama kurmadı tıkladıysanız, tüm parametreleri null veya ilk kez sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="21f2e-204">Disk belleği bağlantıya tıkladıysanız sayfa değişkenini görüntülemek için sayfa numarasını içerir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-204">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="21f2e-205">`ViewData` CurrentSort adlı öğe bu disk belleği bağlantıları sıralama sırasında disk belleği aynı tutmak için dahil edilmesi için geçerli sıralama düzenini görünümüyle sağlar.</span><span class="sxs-lookup"><span data-stu-id="21f2e-205">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="21f2e-206">`ViewData` GeçerliFiltre adlı öğe geçerli filtre dizesi görünümüyle sağlar.</span><span class="sxs-lookup"><span data-stu-id="21f2e-206">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="21f2e-207">Bu değer, disk belleği bağlantıları sırasında disk belleği filtre ayarlarını korumak için eklenmelidir ve sayfası görüntülendiğinde, metin kutusuna geri yüklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="21f2e-208">Arama dizesi sırasında disk belleği değiştirdiyseniz, 1'e sıfırlamak için sayfanın yeni filtre görüntülemek için farklı veri kaybına neden çünkü gerekir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="21f2e-209">Arama dizesi, metin kutusuna girilen değer ve Gönder düğmesine basıldığında değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-209">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="21f2e-210">Bu durumda, `searchString` parametresi null değil.</span><span class="sxs-lookup"><span data-stu-id="21f2e-210">In that case, the `searchString` parameter isn't null.</span></span>

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

<span data-ttu-id="21f2e-211">Sonunda `Index` yöntemi, `PaginatedList.CreateAsync` yöntemi disk belleği destekleyen bir koleksiyon türü öğrencinin tek sayfalık Öğrenci sorgu dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="21f2e-211">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="21f2e-212">Bu sayfada Öğrenciler daha sonra görünümüne geçirilir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-212">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="21f2e-213">`PaginatedList.CreateAsync` Yöntemi bir sayfa numarasını alır.</span><span class="sxs-lookup"><span data-stu-id="21f2e-213">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="21f2e-214">İki soru işaretleri null birleşim işlecinin temsil eder.</span><span class="sxs-lookup"><span data-stu-id="21f2e-214">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="21f2e-215">Null birleşim işleci, null atanabilir bir tür için varsayılan bir değer tanımlar; ifade `(page ?? 1)` anlamına gelir dönüş değerini `page` değerine sahip veya 1 döndürür, `page` null.</span><span class="sxs-lookup"><span data-stu-id="21f2e-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="21f2e-216">Disk belleği bağlantılar için Öğrenci dizini görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="21f2e-216">Add paging links to the Student Index view</span></span>

<span data-ttu-id="21f2e-217">İçinde *Views/Students/Index.cshtml*, var olan kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="21f2e-217">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="21f2e-218">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="21f2e-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="21f2e-219">`@model` Sayfanın üst kısmındaki deyimi belirtir görünümü şimdi alır bir `PaginatedList<T>` yerine Nesne bir `List<T>` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="21f2e-219">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="21f2e-220">Sütun başlığı bağlantıları sorgu dizesi kullanıcı içinde filtre sonuçlarını sıralayabilmesi geçerli arama dizesi denetleyiciye geçirmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="21f2e-220">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="21f2e-221">Disk belleği düğmeler etiketi Yardımcılar tarafından görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="21f2e-221">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="21f2e-222">Uygulamayı çalıştırın ve öğrenciler sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="21f2e-222">Run the app and go to the Students page.</span></span>

![disk belleği bağlantılarla Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

<span data-ttu-id="21f2e-224">Disk belleği çalıştığından emin olmak için farklı sıralamalar disk belleği bağlantıları tıklatın.</span><span class="sxs-lookup"><span data-stu-id="21f2e-224">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="21f2e-225">Ardından bir arama dizesi girin ve yeniden disk belleği de doğru sıralama ve filtreleme ile çalıştığını doğrulamak için disk belleği deneyin.</span><span class="sxs-lookup"><span data-stu-id="21f2e-225">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="21f2e-226">Öğrenci istatistiklerini gösterir hakkında sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="21f2e-226">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="21f2e-227">Contoso University Web sitesinin için **hakkında** sayfasında, kaç tane Öğrenciler her kayıt tarihi için kayıtlı olan görüntülersiniz.</span><span class="sxs-lookup"><span data-stu-id="21f2e-227">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="21f2e-228">Bu grupları gruplandırma ve basit hesaplamaları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-228">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="21f2e-229">Bunu gerçekleştirmek için şunları yapmanız:</span><span class="sxs-lookup"><span data-stu-id="21f2e-229">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="21f2e-230">Görünüme iletmek için gereken verileri için bir görünüm model sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="21f2e-230">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="21f2e-231">Giriş Denetleyicisi hakkında yönteminde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="21f2e-231">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="21f2e-232">Hakkında görünümü değiştirin.</span><span class="sxs-lookup"><span data-stu-id="21f2e-232">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="21f2e-233">Görünüm modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="21f2e-233">Create the view model</span></span>

<span data-ttu-id="21f2e-234">Oluşturma bir *SchoolViewModels* klasöründe *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="21f2e-234">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="21f2e-235">Yeni klasör içinde bir sınıf dosyası ekleyin *EnrollmentDateGroup.cs* ve şablon kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="21f2e-235">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="21f2e-236">Ev denetleyicisi değiştirme</span><span class="sxs-lookup"><span data-stu-id="21f2e-236">Modify the Home Controller</span></span>

<span data-ttu-id="21f2e-237">İçinde *HomeController.cs*, aşağıdaki using deyimlerini dosyanın üst kısmında:</span><span class="sxs-lookup"><span data-stu-id="21f2e-237">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="21f2e-238">Veritabanı bağlamı için bir sınıf değişkeni parantezinden sınıfı için hemen sonra ekleyin ve ASP.NET Core dı bağlamının bir örneği elde:</span><span class="sxs-lookup"><span data-stu-id="21f2e-238">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="21f2e-239">Değiştir `About` aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="21f2e-239">Replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="21f2e-240">LINQ ifadesi Öğrenci varlıklar kayıt tarihe göre gruplar, her grup içindeki varlıkların sayısı hesaplar ve sonuçları bir koleksiyondaki depolar `EnrollmentDateGroup` model nesneleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="21f2e-240">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="21f2e-241">Entity Framework Çekirdek 1.0 sürümü, istemciye tüm sonuç kümesi döndürdü ve gruplandırma istemcide yapılır.</span><span class="sxs-lookup"><span data-stu-id="21f2e-241">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="21f2e-242">Bazı senaryolarda bu performans sorunlarını oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="21f2e-242">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="21f2e-243">Veri üretim birimleri ile performansı test edin ve gerekirse ham SQL sunucusunda gruplandırma yapmak için kullanın emin olun.</span><span class="sxs-lookup"><span data-stu-id="21f2e-243">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="21f2e-244">Ham SQL kullanma hakkında daha fazla bilgi için bkz: [bu serideki son Öğreticisi](advanced.md).</span><span class="sxs-lookup"><span data-stu-id="21f2e-244">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="21f2e-245">Değiştirme Görünüm hakkında</span><span class="sxs-lookup"><span data-stu-id="21f2e-245">Modify the About View</span></span>

<span data-ttu-id="21f2e-246">Kodla *Views/Home/About.cshtml* aşağıdaki kod ile dosya:</span><span class="sxs-lookup"><span data-stu-id="21f2e-246">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="21f2e-247">Uygulamayı çalıştırın ve hakkında sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="21f2e-247">Run the app and go to the About page.</span></span> <span data-ttu-id="21f2e-248">Öğrenciler için her kayıt tarihi sayısı bir tabloda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="21f2e-248">The count of students for each enrollment date is displayed in a table.</span></span>

![Sayfa hakkında](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="21f2e-250">Özet</span><span class="sxs-lookup"><span data-stu-id="21f2e-250">Summary</span></span>

<span data-ttu-id="21f2e-251">Bu öğreticide, sıralama, filtreleme, disk belleği ve gruplandırma gerçekleştirmeyi öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="21f2e-251">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="21f2e-252">Sonraki öğreticide geçişler kullanarak veri modeli değişikliklerini işlemek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="21f2e-252">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="21f2e-253">[Önceki](crud.md)
> [sonraki](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="21f2e-253">[Previous](crud.md)
[Next](migrations.md)</span></span>  
