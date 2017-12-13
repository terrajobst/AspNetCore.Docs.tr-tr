---
title: "ASP.NET Core MVC EF çekirdek - sıralama, filtre, disk belleği - 10 3 ile"
author: tdykstra
description: "Bu öğreticide, sıralama, filtreleme ve ASP.NET Core ve Entity Framework Çekirdek kullanarak sayfa için işlevsellik disk belleği eklemeniz."
keywords: "ASP.NET Core, Entity Framework Çekirdek, sıralama, filtre, disk belleği, gruplandırma"
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: e6c1ff3c-5673-43bf-9c2d-077f6ada1f29
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 59fff4dbf4736f0776aac4072f3f4e2d40119842
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a><span data-ttu-id="fe7a0-104">Sıralama, filtreleme, disk belleği ve gruplandırma - EF çekirdek ASP.NET Core MVC Öğreticisi (3 / 10)</span><span class="sxs-lookup"><span data-stu-id="fe7a0-104">Sorting, filtering, paging, and grouping - EF Core with ASP.NET Core MVC tutorial (3 of 10)</span></span>

<span data-ttu-id="fe7a0-105">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fe7a0-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fe7a0-106">Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="fe7a0-107">Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).</span><span class="sxs-lookup"><span data-stu-id="fe7a0-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="fe7a0-108">Önceki öğreticide, bir dizi web sayfası Öğrenci varlıklar için temel CRUD işlemleri için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-108">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="fe7a0-109">Bu öğreticide Öğrenciler dizin sayfasına sıralama, filtreleme ve disk belleği işlevselliği ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-109">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="fe7a0-110">Ayrıca, basit gruplandırma yapan bir sayfa oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-110">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="fe7a0-111">Aşağıdaki çizimde tamamladığınızda sayfa nasıl görüneceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-111">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="fe7a0-112">Sütun başlıkları kullanıcının sütuna göre sıralamak için tıklatabileceği bağlantı bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-112">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="fe7a0-113">Bir sütun başlığına sürekli olarak artan veya azalan sıralama düzeni arasında geçiş yapar.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-113">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Öğrenciler dizin sayfası](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="fe7a0-115">Öğrenciler dizin sayfasına Sütun sıralama bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="fe7a0-115">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="fe7a0-116">Öğrenci dizin sayfasına sıralama eklemek için değiştireceğiz `Index` yöntemi Öğrenciler denetleyicisinin ve kod Öğrenci dizin görünümüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-116">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="fe7a0-117">İşlevleri sıralama için dizin yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="fe7a0-117">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="fe7a0-118">İçinde *StudentsController.cs*, yerine `Index` aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="fe7a0-118">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="fe7a0-119">Bu kod alan bir `sortOrder` URL'deki sorgu dizesi parametresi.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-119">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="fe7a0-120">Sorgu dizesi değerini eylem yönteminin bir parametresi olarak ASP.NET Core MVC tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-120">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="fe7a0-121">Parametre "Ad" veya "Tarih", ardından isteğe bağlı olarak bir alt çizgi ve azalan belirtmek için "desc" dizesi olan bir dize olur.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-121">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="fe7a0-122">Varsayılan sıralama düzeni artan.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-122">The default sort order is ascending.</span></span>

<span data-ttu-id="fe7a0-123">Dizin sayfası istenen ilk kez hiçbir sorgu dizesi yok.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-123">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="fe7a0-124">Öğrenciler artan düzende başarısızlık durumunda tarafından belirlenen varsayılan değer olan son adıyla görüntülenir `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-124">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="fe7a0-125">Kullanıcı uygun sütun başlığını köprü tıkladığında `sortOrder` değeri, sorgu dizesinde sağlanır.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-125">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="fe7a0-126">İki `ViewData` öğeleri (NameSortParm ve DateSortParm) uygun sorgu dizesi değerlerini sütun başlığını köprüler yapılandırmak için Görünüm tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-126">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="fe7a0-127">Üçlü deyimleri bunlar.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-127">These are ternary statements.</span></span> <span data-ttu-id="fe7a0-128">Birinci anlama gelir `sortOrder` parametresi null veya boş NameSortParm "name_desc" olarak ayarlanmalıdır; Aksi halde, boş bir dize olarak ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-128">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="fe7a0-129">Bu iki ifade sütun başlığını köprüler şu şekilde ayarlamak görünümü etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="fe7a0-129">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="fe7a0-130">Geçerli bir sıralama düzeni</span><span class="sxs-lookup"><span data-stu-id="fe7a0-130">Current sort order</span></span>  | <span data-ttu-id="fe7a0-131">Son adı köprü</span><span class="sxs-lookup"><span data-stu-id="fe7a0-131">Last Name Hyperlink</span></span> | <span data-ttu-id="fe7a0-132">Tarih köprü</span><span class="sxs-lookup"><span data-stu-id="fe7a0-132">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="fe7a0-133">Ad artan en son</span><span class="sxs-lookup"><span data-stu-id="fe7a0-133">Last Name ascending</span></span>  | <span data-ttu-id="fe7a0-134">descending</span><span class="sxs-lookup"><span data-stu-id="fe7a0-134">descending</span></span>          | <span data-ttu-id="fe7a0-135">ascending</span><span class="sxs-lookup"><span data-stu-id="fe7a0-135">ascending</span></span>      |
| <span data-ttu-id="fe7a0-136">Ad azalan en son</span><span class="sxs-lookup"><span data-stu-id="fe7a0-136">Last Name descending</span></span> | <span data-ttu-id="fe7a0-137">ascending</span><span class="sxs-lookup"><span data-stu-id="fe7a0-137">ascending</span></span>           | <span data-ttu-id="fe7a0-138">ascending</span><span class="sxs-lookup"><span data-stu-id="fe7a0-138">ascending</span></span>      |
| <span data-ttu-id="fe7a0-139">Artan tarihi</span><span class="sxs-lookup"><span data-stu-id="fe7a0-139">Date ascending</span></span>       | <span data-ttu-id="fe7a0-140">ascending</span><span class="sxs-lookup"><span data-stu-id="fe7a0-140">ascending</span></span>           | <span data-ttu-id="fe7a0-141">descending</span><span class="sxs-lookup"><span data-stu-id="fe7a0-141">descending</span></span>     |
| <span data-ttu-id="fe7a0-142">Azalan tarihi</span><span class="sxs-lookup"><span data-stu-id="fe7a0-142">Date descending</span></span>      | <span data-ttu-id="fe7a0-143">ascending</span><span class="sxs-lookup"><span data-stu-id="fe7a0-143">ascending</span></span>           | <span data-ttu-id="fe7a0-144">ascending</span><span class="sxs-lookup"><span data-stu-id="fe7a0-144">ascending</span></span>      |

<span data-ttu-id="fe7a0-145">Yöntemi, LINQ to Entities göre sıralamak için sütun belirlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-145">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="fe7a0-146">Kod oluşturur bir `IQueryable` değişken switch deyimi önce değiştirir, bunu, switch deyimi ve çağrıları `ToListAsync` sonra yöntemi `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-146">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="fe7a0-147">Ne zaman oluşturma ve değiştirme `IQueryable` değişkenleri, sorgu veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-147">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="fe7a0-148">Dönüştürülünceye kadar sorgu yürütülmedi `IQueryable` gibi bir yöntemini çağırarak bir koleksiyon nesnesine `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-148">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="fe7a0-149">Bu nedenle, bu kod kadar yürütülmedi tek bir sorgu sonuçları `return View` deyimi.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-149">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="fe7a0-150">Bu kod, çok sayıda sütun ayrıntılı alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-150">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="fe7a0-151">[Bu serideki son öğretici](advanced.md#dynamic-linq) adını geçirmenize olanak verir kodunun nasıl yazılacağını gösterir `OrderBy` bir dize değişkeni sütununda.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-151">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="fe7a0-152">Sütun başlık köprüleri için Öğrenci dizini görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="fe7a0-152">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="fe7a0-153">Kodla *Views/Students/Index.cshtml*, sütun başlığını köprüler eklemek için aşağıdaki kod ile.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-153">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="fe7a0-154">Değiştirilen satırları vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-154">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="fe7a0-155">Bu kod bilgileri kullanan `ViewData` uygun sorguyla köprüler ayarlamak için özellikler dize değerleri.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-155">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="fe7a0-156">Uygulama, belirleyin **Öğrenciler** sekmesine ve tıklayın **Soyadı** ve **kayıt tarihi** Bu sıralama doğrulamak için sütun başlıkları çalışır.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-156">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Ad sırayla Öğrenciler dizin sayfası](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="fe7a0-158">Bir arama kutusu Öğrenciler dizin sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="fe7a0-158">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="fe7a0-159">Öğrenciler dizin sayfasına filtre eklemek için metin kutusu ve bir gönderme düğmesi görünümüne ekleyin ve karşılık gelen değişiklik `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-159">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="fe7a0-160">Metin kutusunda, ad ve soyadı alanları aramak için bir dize girin olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-160">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="fe7a0-161">Filtreleme işlevselliği dizin yöntemine ekleyin</span><span class="sxs-lookup"><span data-stu-id="fe7a0-161">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="fe7a0-162">İçinde *StudentsController.cs*, yerine `Index` (değişiklikleri vurgulanır) aşağıdaki kod ile yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-162">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="fe7a0-163">Eklediğiniz bir `searchString` parametresi `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-163">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="fe7a0-164">Bir metin kutusundan dizin görünümüne ekleyeceksiniz arama dizesi değeri alındı.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-164">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="fe7a0-165">LINQ ifadesi bir where eklediğiniz yalnızca, ad ve Soyadı arama dizesini içeren Öğrenciler seçer yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-165">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="fe7a0-166">Where ekler deyimi yan tümcesi yalnızca arama için bir değer ise gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-166">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="fe7a0-167">Burada, aradığınız `Where` yöntemi bir `IQueryable` nesne ve filtre sunucuda işlenir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-167">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="fe7a0-168">Bazı senaryolarda, çağırma `Where` yöntemi bir bellek içi koleksiyonda bir genişletme yöntemi olarak.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-168">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="fe7a0-169">(Örneğin, referansını değiştirme varsayalım `_context.Students` bir EF, bu nedenle, bunun yerine `DbSet` döndüren bir depo yöntemi başvurduğu bir `IEnumerable` koleksiyonu.) Sonuç normalde aynı kalır ancak bazı durumlarda farklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-169">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="fe7a0-170">Örneğin, .NET Framework uygulamasını `Contains` yöntemi, varsayılan olarak büyük küçük harfe duyarlı karşılaştırma gerçekleştirir, ancak SQL Server'da bu SQL Server örneği harmanlama ayarı tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-170">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="fe7a0-171">Bu ayar varsayılan olarak çok büyük küçük harfe duyarsızdır.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-171">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="fe7a0-172">Çağrı `ToUpper` yöntemini test açıkça büyük küçük harf duyarsız sağlamak için: *nerede (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-172">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="fe7a0-173">Emin kodunu döndüren bir depo daha sonra değiştirirseniz, sonuçları aynı kalmasını bir `IEnumerable` koleksiyon yerine bir `IQueryable` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-173">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="fe7a0-174">(Çağırdığınızda `Contains` yöntemi bir `IEnumerable` koleksiyonu, .NET Framework uygulamasını alın; çağırdığınızda, üzerinde bir `IQueryable` nesnesi, veritabanı sağlayıcısı uygulaması alın.) Ancak, bulunmaktadır performans Bu çözüm için.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-174">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there is a performance penalty for this solution.</span></span> <span data-ttu-id="fe7a0-175">`ToUpper` Kod TSQL SELECT deyimi WHERE yan tümcesinde bir işlev yerleştirecek.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-175">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="fe7a0-176">Bir dizini kullanarak, iyileştirici engeller.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-176">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="fe7a0-177">SQL çoğunlukla olarak büyük küçük harf duyarsız yüklendi Bunu önlemek en iyi düşünüldüğünde `ToUpper` büyük küçük harfe duyarlı veri deposuna geçirene kadar kod.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-177">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="fe7a0-178">Bir arama kutusu Öğrenci dizin görünümüne ekleyin</span><span class="sxs-lookup"><span data-stu-id="fe7a0-178">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="fe7a0-179">İçinde *Views/Student/Index.cshtml*, hemen açılış tablo önce etiketi resim yazısı, bir metin kutusu oluşturma için vurgulanmış kodu ekleyin ve bir **arama** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-179">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="fe7a0-180">Bu kodu kullanır `<form>` [yardımcı etiketi](xref:mvc/views/tag-helpers/intro) düğmesi ve arama metin kutusuna eklemek için.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-180">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="fe7a0-181">Varsayılan olarak, `<form>` etiket Yardımcısı parametreleri HTTP ileti gövdesi yer alan ve URL sorgu dizeleri geçirilir, yani bir POST ile form verileri gönderir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-181">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="fe7a0-182">HTTP GET belirttiğinizde, form verilerini URL'de sorgu dizeleri kullanıcıların URL yer işareti sağlayan geçirilir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-182">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="fe7a0-183">Kullanmanız gereken W3C yönergeleri önerilen eylemi bir güncelleştirmede sonuçlanmaz zaman alın.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-183">The W3C guidelines recommend that you should use GET when the action does not result in an update.</span></span>

<span data-ttu-id="fe7a0-184">Uygulama, belirleyin **Öğrenciler** sekmesinde, bir arama dizesi girin ve filtreleme çalıştığını doğrulamak için Ara'yı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-184">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![Filtreleme ile Öğrenciler dizin sayfası](sort-filter-page/_static/filtering.png)

<span data-ttu-id="fe7a0-186">URL arama dizesi içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-186">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="fe7a0-187">Bu sayfaya yer işareti varsa, yer işareti kullandığınızda filtrelenmiş liste elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-187">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="fe7a0-188">Ekleme `method="get"` için `form` etiketi olan neyin neden olduğunu oluşturulacak sorgu dizesi.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-188">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="fe7a0-189">Bu aşamada, bir sütun başlığını sıralama bağlantısını tıklatırsanız girdiğiniz filtre değeri kaybedersiniz **arama** kutusu.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-189">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="fe7a0-190">Sonraki bölümde düzeltmesi.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-190">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="fe7a0-191">Sayfalama işlevselliğinin Öğrenciler dizin sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="fe7a0-191">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="fe7a0-192">Disk belleği Öğrenciler dizin sayfasına eklemek için oluşturacağınız bir `PaginatedList` kullanan sınıfı `Skip` ve `Take` her zaman tablonun tüm satırlarının almak yerine sunucusundaki verileri filtrelemek için deyimleri.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-192">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="fe7a0-193">Ek değişiklikler hale getireceğiz sonra `Index` yöntemi ve disk belleği düğmelere ekleme `Index` görünümü.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-193">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="fe7a0-194">Aşağıdaki çizimde, disk belleği düğmeleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-194">The following illustration shows the paging buttons.</span></span>

![Disk belleği bağlantılar sayfasıyla Öğrenciler dizin](sort-filter-page/_static/paging.png)

<span data-ttu-id="fe7a0-196">Proje klasöründe oluşturma `PaginatedList.cs`ve ardından şablon kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-196">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="fe7a0-197">`CreateAsync` Yöntemi bu kodda sayfa boyutu ve sayfa numarasını alır ve uygun geçerlidir `Skip` ve `Take` deyimlerini `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-197">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="fe7a0-198">Zaman `ToListAsync` üzerinde adlı `IQueryable`, yalnızca istenen sayfa içeren bir liste döndürür.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-198">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="fe7a0-199">Özellikler `HasPreviousPage` ve `HasNextPage` etkinleştirmek veya devre dışı bırakmak için kullanılan **önceki** ve **sonraki** düğmeleri disk belleği.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-199">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="fe7a0-200">A `CreateAsync` yöntemi yerine bir oluşturucu oluşturmak için kullanılan `PaginatedList<T>` zaman uyumsuz kod oluşturucuları çalışamadığı nesne.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-200">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="fe7a0-201">Sayfalama işlevselliğinin dizin yöntemine ekleyin</span><span class="sxs-lookup"><span data-stu-id="fe7a0-201">Add paging functionality to the Index method</span></span>

<span data-ttu-id="fe7a0-202">İçinde *StudentsController.cs*, yerine `Index` aşağıdaki kod ile yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-202">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="fe7a0-203">Bu kod, yöntem imzası sayfa numarası parametre, geçerli bir sıralama sipariş parametresi ve geçerli bir filtre parametresi ekler.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-203">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="fe7a0-204">Kullanıcı bir disk belleği veya bağlantı sıralama kurmadı tıkladıysanız, tüm parametreleri null veya ilk kez sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-204">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="fe7a0-205">Disk belleği bağlantıya tıkladıysanız sayfa değişkenini görüntülemek için sayfa numarasını içerir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-205">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="fe7a0-206">`ViewData` CurrentSort adlı öğe bu disk belleği bağlantıları sıralama sırasında disk belleği aynı tutmak için dahil edilmesi için geçerli sıralama düzenini görünümüyle sağlar.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-206">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="fe7a0-207">`ViewData` GeçerliFiltre adlı öğe geçerli filtre dizesi görünümüyle sağlar.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-207">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="fe7a0-208">Bu değer, disk belleği bağlantıları sırasında disk belleği filtre ayarlarını korumak için eklenmelidir ve sayfası görüntülendiğinde, metin kutusuna geri yüklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-208">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="fe7a0-209">Arama dizesi sırasında disk belleği değiştirdiyseniz, 1'e sıfırlamak için sayfanın yeni filtre görüntülemek için farklı veri kaybına neden çünkü gerekir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-209">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="fe7a0-210">Arama dizesi, metin kutusuna girilen değer ve Gönder düğmesine basıldığında değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-210">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="fe7a0-211">Bu durumda, `searchString` parametresi null değil.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-211">In that case, the `searchString` parameter is not null.</span></span>

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

<span data-ttu-id="fe7a0-212">Sonunda `Index` yöntemi, `PaginatedList.CreateAsync` yöntemi disk belleği destekleyen bir koleksiyon türü öğrencinin tek sayfalık Öğrenci sorgu dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-212">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="fe7a0-213">Bu sayfada Öğrenciler daha sonra görünümüne geçirilir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-213">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="fe7a0-214">`PaginatedList.CreateAsync` Yöntemi bir sayfa numarasını alır.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-214">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="fe7a0-215">İki soru işaretleri null birleşim işlecinin temsil eder.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-215">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="fe7a0-216">Null birleşim işleci, null atanabilir bir tür için varsayılan bir değer tanımlar; ifade `(page ?? 1)` anlamına gelir dönüş değerini `page` değerine sahip veya 1 döndürür, `page` null.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-216">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="fe7a0-217">Disk belleği bağlantılar için Öğrenci dizini görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="fe7a0-217">Add paging links to the Student Index view</span></span>

<span data-ttu-id="fe7a0-218">İçinde *Views/Students/Index.cshtml*, var olan kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-218">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="fe7a0-219">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-219">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="fe7a0-220">`@model` Sayfanın üst kısmındaki deyimi belirtir görünümü şimdi alır bir `PaginatedList<T>` yerine Nesne bir `List<T>` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-220">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="fe7a0-221">Sütun başlığı bağlantıları sorgu dizesi kullanıcı içinde filtre sonuçlarını sıralayabilmesi geçerli arama dizesi denetleyiciye geçirmek için kullanın:</span><span class="sxs-lookup"><span data-stu-id="fe7a0-221">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="fe7a0-222">Disk belleği düğmeler etiketi Yardımcılar tarafından görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="fe7a0-222">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="fe7a0-223">Uygulamayı çalıştırın ve öğrenciler sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-223">Run the app and go to the Students page.</span></span>

![Disk belleği bağlantılar sayfasıyla Öğrenciler dizin](sort-filter-page/_static/paging.png)

<span data-ttu-id="fe7a0-225">Disk belleği çalıştığından emin olmak için farklı sıralamalar disk belleği bağlantıları tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-225">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="fe7a0-226">Ardından bir arama dizesi girin ve yeniden disk belleği de doğru sıralama ve filtreleme ile çalıştığını doğrulamak için disk belleği deneyin.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-226">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="fe7a0-227">Öğrenci istatistiklerini gösterir hakkında sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="fe7a0-227">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="fe7a0-228">Contoso University Web sitesinin için **hakkında** sayfasında, kaç tane Öğrenciler her kayıt tarihi için kayıtlı olan görüntülersiniz.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-228">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="fe7a0-229">Bu grupları gruplandırma ve basit hesaplamaları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-229">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="fe7a0-230">Bunu gerçekleştirmek için şunları yapmanız:</span><span class="sxs-lookup"><span data-stu-id="fe7a0-230">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="fe7a0-231">Görünüme iletmek için gereken verileri için bir görünüm model sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-231">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="fe7a0-232">Giriş Denetleyicisi hakkında yönteminde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-232">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="fe7a0-233">Hakkında görünümü değiştirin.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-233">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="fe7a0-234">Görünüm modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="fe7a0-234">Create the view model</span></span>

<span data-ttu-id="fe7a0-235">Oluşturma bir *SchoolViewModels* klasöründe *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-235">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="fe7a0-236">Yeni klasör içinde bir sınıf dosyası ekleyin *EnrollmentDateGroup.cs* ve şablon kodu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="fe7a0-236">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="fe7a0-237">Ev denetleyicisi değiştirme</span><span class="sxs-lookup"><span data-stu-id="fe7a0-237">Modify the Home Controller</span></span>

<span data-ttu-id="fe7a0-238">İçinde *HomeController.cs*, aşağıdaki using deyimlerini dosyanın üst kısmında:</span><span class="sxs-lookup"><span data-stu-id="fe7a0-238">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="fe7a0-239">Veritabanı bağlamı için bir sınıf değişkeni parantezinden sınıfı için hemen sonra ekleyin ve ASP.NET Core dı bağlamının bir örneği elde:</span><span class="sxs-lookup"><span data-stu-id="fe7a0-239">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="fe7a0-240">Değiştir `About` aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="fe7a0-240">Replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="fe7a0-241">LINQ ifadesi Öğrenci varlıklar kayıt tarihe göre gruplar, her grup içindeki varlıkların sayısı hesaplar ve sonuçları bir koleksiyondaki depolar `EnrollmentDateGroup` model nesneleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-241">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="fe7a0-242">Entity Framework Çekirdek 1.0 sürümü, istemciye tüm sonuç kümesi döndürdü ve gruplandırma istemcide yapılır.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-242">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="fe7a0-243">Bazı senaryolarda bu performans sorunlarını oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-243">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="fe7a0-244">Veri üretim birimleri ile performansı test edin ve gerekirse ham SQL sunucusunda gruplandırma yapmak için kullanın emin olun.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-244">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="fe7a0-245">Ham SQL kullanma hakkında daha fazla bilgi için bkz: [bu serideki son Öğreticisi](advanced.md).</span><span class="sxs-lookup"><span data-stu-id="fe7a0-245">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="fe7a0-246">Değiştirme Görünüm hakkında</span><span class="sxs-lookup"><span data-stu-id="fe7a0-246">Modify the About View</span></span>

<span data-ttu-id="fe7a0-247">Kodla *Views/Home/About.cshtml* aşağıdaki kod ile dosya:</span><span class="sxs-lookup"><span data-stu-id="fe7a0-247">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="fe7a0-248">Uygulamayı çalıştırın ve hakkında sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-248">Run the app and go to the About page.</span></span> <span data-ttu-id="fe7a0-249">Öğrenciler için her kayıt tarihi sayısı bir tabloda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-249">The count of students for each enrollment date is displayed in a table.</span></span>

![Sayfa hakkında](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="fe7a0-251">Özet</span><span class="sxs-lookup"><span data-stu-id="fe7a0-251">Summary</span></span>

<span data-ttu-id="fe7a0-252">Bu öğreticide, sıralama, filtreleme, disk belleği ve gruplandırma gerçekleştirmeyi öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-252">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="fe7a0-253">Sonraki öğreticide geçişler kullanarak veri modeli değişikliklerini işlemek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="fe7a0-253">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fe7a0-254">[Önceki](crud.md)
[sonraki](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="fe7a0-254">[Previous](crud.md)
[Next](migrations.md)</span></span>  
