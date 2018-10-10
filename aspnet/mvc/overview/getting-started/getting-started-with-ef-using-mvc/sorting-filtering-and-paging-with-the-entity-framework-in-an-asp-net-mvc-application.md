---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Sıralama, filtreleme ve bir ASP.NET MVC uygulamasındaki Entity Framework ile sayfalama | Microsoft Docs
author: tdykstra
description: Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: riande
ms.date: 10/08/2018
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9fabb5a90af715d4e96ff79b43bfff5a4600ac08
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912780"
---
# <a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="01b01-103">Sıralama, filtreleme ve bir ASP.NET MVC uygulamasındaki Entity Framework ile sayfalama</span><span class="sxs-lookup"><span data-stu-id="01b01-103">Sorting, filtering, and paging with the Entity Framework in an ASP.NET MVC application</span></span>

<span data-ttu-id="01b01-104">tarafından [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="01b01-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="01b01-105">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="01b01-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> <span data-ttu-id="01b01-106">Contoso University örnek web uygulaması Entity Framework 6 Code First ve Visual Studio kullanarak ASP.NET MVC 5 uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="01b01-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio.</span></span> <span data-ttu-id="01b01-107">Öğretici serisinin hakkında daha fazla bilgi için bkz. [serideki ilk öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="01b01-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="01b01-108">Önceki öğreticide, bir dizi web sayfaları için temel CRUD işlemleri için uygulanan `Student` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="01b01-108">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for `Student` entities.</span></span> <span data-ttu-id="01b01-109">Bu öğreticide, sıralama, filtreleme ve sayfalama işlevselliğinin ekleyeceksiniz **Öğrenciler** dizin sayfası.</span><span class="sxs-lookup"><span data-stu-id="01b01-109">In this tutorial you'll add sorting, filtering, and paging functionality to the **Students** Index page.</span></span> <span data-ttu-id="01b01-110">Basit gruplandırma yapan bir sayfa da oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="01b01-110">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="01b01-111">Aşağıdaki çizim, hazır olduğunuzda sayfanın nasıl görüneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="01b01-111">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="01b01-112">Sütun başlıkları, kullanıcının sütuna göre sıralamak için tıklayabileceği bağlantılar verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="01b01-112">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="01b01-113">Bir sütun başlığına tekrar tekrar tıklayarak, artan veya azalan sıralama düzeni arasında geçiş yapar.</span><span class="sxs-lookup"><span data-stu-id="01b01-113">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="01b01-115">Öğrenciler dizin sayfasına Sütun sıralama bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="01b01-115">Add column sort links to the Students index page</span></span>

<span data-ttu-id="01b01-116">Öğrenci dizin sayfasına sıralama eklemek için değiştireceksiniz `Index` yöntemi `Student` denetleyicisi ve kodu ekleyin `Student` dizin görünümü.</span><span class="sxs-lookup"><span data-stu-id="01b01-116">To add sorting to the Student Index page, you'll change the `Index` method of the `Student` controller and add code to the `Student` Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="01b01-117">Sıralama işlevleri dizin yöntemine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="01b01-117">Add sorting functionality to the Index method</span></span>

- <span data-ttu-id="01b01-118">İçinde *Controllers\StudentController.cs*, değiştirin `Index` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="01b01-118">In *Controllers\StudentController.cs*, replace the `Index` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="01b01-119">Bu kod alır bir `sortOrder` URL'ye sorgu dizesi parametresi.</span><span class="sxs-lookup"><span data-stu-id="01b01-119">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="01b01-120">Sorgu dizesi değerini eylem yönteminin bir parametresi olarak ASP.NET MVC tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="01b01-120">The query string value is provided by ASP.NET MVC as a parameter to the action method.</span></span> <span data-ttu-id="01b01-121">Ardından isteğe bağlı olarak bir alt çizgi ve azalan düzende belirtmek için "desc" dize "Name" veya "Tarih" olan dizesi parametresidir.</span><span class="sxs-lookup"><span data-stu-id="01b01-121">The parameter is a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="01b01-122">Varsayılan sıralama artan düzendedir.</span><span class="sxs-lookup"><span data-stu-id="01b01-122">The default sort order is ascending.</span></span>

<span data-ttu-id="01b01-123">Dizin Sayfası istendi, ilk kez hiçbir sorgu dizesi yoktur.</span><span class="sxs-lookup"><span data-stu-id="01b01-123">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="01b01-124">Öğrenciler tarafından artan düzende görüntülenen `LastName`, başarısızlık durumunda tarafından belirlenen varsayılan değerdir `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="01b01-124">The students are displayed in ascending order by `LastName`, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="01b01-125">Kullanıcı uygun sütun başlığına köprü tıkladığında `sortOrder` değeri, sorgu dizesinde sağlanır.</span><span class="sxs-lookup"><span data-stu-id="01b01-125">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="01b01-126">İki `ViewBag` görünüm sütun başlığı köprüler uygun sorgu dizesi değerleri yapılandırabilirsiniz. böylece değişkenler kullanılır:</span><span class="sxs-lookup"><span data-stu-id="01b01-126">The two `ViewBag` variables are used so that the view can configure the column heading hyperlinks with the appropriate query string values:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="01b01-127">Üçlü deyimleri şunlardır.</span><span class="sxs-lookup"><span data-stu-id="01b01-127">These are ternary statements.</span></span> <span data-ttu-id="01b01-128">Olmadığını birincinin belirtir `sortOrder` parametresi null veya boş `ViewBag.NameSortParm` ayarlanması gerekir "adı\_desc"; Aksi takdirde boş bir dize olarak ayarlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="01b01-128">The first one specifies that if the `sortOrder` parameter is null or empty, `ViewBag.NameSortParm` should be set to "name\_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="01b01-129">Bu iki deyimden sütun başlığı köprüler şu şekilde ayarlayın görünümün etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="01b01-129">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

| <span data-ttu-id="01b01-130">Geçerli bir sıralama düzeni</span><span class="sxs-lookup"><span data-stu-id="01b01-130">Current sort order</span></span> | <span data-ttu-id="01b01-131">Son adı köprü</span><span class="sxs-lookup"><span data-stu-id="01b01-131">Last Name Hyperlink</span></span> | <span data-ttu-id="01b01-132">Tarih köprü</span><span class="sxs-lookup"><span data-stu-id="01b01-132">Date Hyperlink</span></span> |
| --- | --- | --- |
| <span data-ttu-id="01b01-133">Adı artan en son</span><span class="sxs-lookup"><span data-stu-id="01b01-133">Last Name ascending</span></span> | <span data-ttu-id="01b01-134">descending</span><span class="sxs-lookup"><span data-stu-id="01b01-134">descending</span></span> | <span data-ttu-id="01b01-135">ascending</span><span class="sxs-lookup"><span data-stu-id="01b01-135">ascending</span></span> |
| <span data-ttu-id="01b01-136">Son azalan düzende ad</span><span class="sxs-lookup"><span data-stu-id="01b01-136">Last Name descending</span></span> | <span data-ttu-id="01b01-137">ascending</span><span class="sxs-lookup"><span data-stu-id="01b01-137">ascending</span></span> | <span data-ttu-id="01b01-138">ascending</span><span class="sxs-lookup"><span data-stu-id="01b01-138">ascending</span></span> |
| <span data-ttu-id="01b01-139">Artan tarihi</span><span class="sxs-lookup"><span data-stu-id="01b01-139">Date ascending</span></span> | <span data-ttu-id="01b01-140">ascending</span><span class="sxs-lookup"><span data-stu-id="01b01-140">ascending</span></span> | <span data-ttu-id="01b01-141">descending</span><span class="sxs-lookup"><span data-stu-id="01b01-141">descending</span></span> |
| <span data-ttu-id="01b01-142">Azalan düzende tarihi</span><span class="sxs-lookup"><span data-stu-id="01b01-142">Date descending</span></span> | <span data-ttu-id="01b01-143">ascending</span><span class="sxs-lookup"><span data-stu-id="01b01-143">ascending</span></span> | <span data-ttu-id="01b01-144">ascending</span><span class="sxs-lookup"><span data-stu-id="01b01-144">ascending</span></span> |

<span data-ttu-id="01b01-145">Yöntemini kullanan [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) göre sıralamak için sütun belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="01b01-145">The method uses [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) to specify the column to sort by.</span></span> <span data-ttu-id="01b01-146">Kod oluşturur bir <xref:System.Linq.IQueryable%601> önce değişken `switch` ifadesi, değiştiren içinde `switch` deyimi ve çağrılarını `ToList` sonrasına `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="01b01-146">The code creates an <xref:System.Linq.IQueryable%601> variable before the `switch` statement, modifies it in the `switch` statement, and calls the `ToList` method after the `switch` statement.</span></span> <span data-ttu-id="01b01-147">Ne zaman oluşturma ve değiştirme `IQueryable` değişkenleri, sorgu veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="01b01-147">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="01b01-148">Sorgu, dönüştürmek kadar yürütülmez `IQueryable` nesne gibi bir yöntem çağırarak koleksiyonuna `ToList`.</span><span class="sxs-lookup"><span data-stu-id="01b01-148">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToList`.</span></span> <span data-ttu-id="01b01-149">Bu nedenle, bu kod, kadar yürütülmez tek bir sorgu sonuçları `return View` deyimi.</span><span class="sxs-lookup"><span data-stu-id="01b01-149">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="01b01-150">Her bir sıralama düzeni için farklı LINQ deyimleri yazılırken alternatif olarak, dinamik olarak bir LINQ ifadesini oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="01b01-150">As an alternative to writing different LINQ statements for each sort order, you can dynamically create a LINQ statement.</span></span> <span data-ttu-id="01b01-151">Dinamik LINQ hakkında daha fazla bilgi için bkz. [dinamik LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).</span><span class="sxs-lookup"><span data-stu-id="01b01-151">For information about dynamic LINQ, see [Dynamic LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="01b01-152">Öğrenci dizin görünüme sütun başlık köprüler ekleyin</span><span class="sxs-lookup"><span data-stu-id="01b01-152">Add column heading hyperlinks to the Student index view</span></span>

1. <span data-ttu-id="01b01-153">İçinde *Views\Student\Index.cshtml*, değiştirin `<tr>` ve `<th>` vurgulanmış kodu başlık satırı için öğeleri:</span><span class="sxs-lookup"><span data-stu-id="01b01-153">In *Views\Student\Index.cshtml*, replace the `<tr>` and `<th>` elements for the heading row with the highlighted code:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   <span data-ttu-id="01b01-154">Bilgiler, bu kodu kullanan `ViewBag` uygun sorgu köprülerle ayarlamak için özellikler dize değerleri.</span><span class="sxs-lookup"><span data-stu-id="01b01-154">This code uses the information in the `ViewBag` properties to set up hyperlinks with the appropriate query string values.</span></span>

2. <span data-ttu-id="01b01-155">Sayfayı çalıştırın ve tıklayın **Soyadı** ve **kayıt tarihi** sütun başlıkları, sıralama doğrulamak için çalışır.</span><span class="sxs-lookup"><span data-stu-id="01b01-155">Run the page and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

   ![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

   <span data-ttu-id="01b01-157">Tıkladıktan sonra **Soyadı** başlığı Öğrenciler soyadına göre azalan düzende görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="01b01-157">After you click the **Last Name** heading, students are displayed in descending last name order.</span></span>

   ![Web tarayıcısında Öğrenci dizini görüntüle](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="01b01-159">Bir arama kutusu Öğrenciler dizin sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="01b01-159">Add a search box to the Students index page</span></span>

<span data-ttu-id="01b01-160">Öğrenciler dizin sayfasına filtre eklemek için görünümü için metin kutusu ve bir Gönder düğmesi ekleyin ve karşılık gelen değişiklik `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="01b01-160">To add filtering to the Students index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="01b01-161">Metin kutusu için ad ve soyadı alanları aramak için bir dize girmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="01b01-161">The text box lets you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="01b01-162">Filtreleme işlevselliği dizin yöntemine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="01b01-162">Add filtering functionality to the Index method</span></span>

- <span data-ttu-id="01b01-163">İçinde *Controllers\StudentController.cs*, değiştirin `Index` yöntemini aşağıdaki kodla (değişiklikleri vurgulanır):</span><span class="sxs-lookup"><span data-stu-id="01b01-163">In *Controllers\StudentController.cs*, replace the `Index` method with the following code (the changes are highlighted):</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

<span data-ttu-id="01b01-164">Kod ekler bir `searchString` parametresi `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="01b01-164">The code adds a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="01b01-165">Dizin görünümüne ekleyeceksiniz bir metin kutusundan arama dizesi değeri alındı.</span><span class="sxs-lookup"><span data-stu-id="01b01-165">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="01b01-166">Ayrıca ekler bir `where` yan tümcesine yalnızca Öğrenciler, ad ve Soyadı arama dizesini içeren seçer LINQ ifadesi.</span><span class="sxs-lookup"><span data-stu-id="01b01-166">It also adds a `where` clause to the LINQ statement that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="01b01-167">Ekler deyimi <xref:System.Linq.Queryable.Where%2A> yan tümcesi yalnızca aramak için bir değer ise yürütür.</span><span class="sxs-lookup"><span data-stu-id="01b01-167">The statement that adds the <xref:System.Linq.Queryable.Where%2A> clause executes only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="01b01-168">Çoğu durumda bir Entity Framework varlık kümesini veya bir bellek içi koleksiyonunda bir genişletme yöntemi olarak aynı yöntemi çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="01b01-168">In many cases you can call the same method either on an Entity Framework entity set or as an extension method on an in-memory collection.</span></span> <span data-ttu-id="01b01-169">Sonuçları normalde aynıdır, ancak bazı durumlarda farklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="01b01-169">The results are normally the same but in some cases may be different.</span></span>
>
> <span data-ttu-id="01b01-170">Örneğin, .NET Framework uygulamasını `Contains` yöntem boş bir dizeyi geçirmek, ancak SQL Server Compact 4.0 için Entity Framework sağlayıcısı boş dizeler için sıfır satır döndürür. tüm satırları döndürür.</span><span class="sxs-lookup"><span data-stu-id="01b01-170">For example, the .NET Framework implementation of the `Contains` method returns all rows when you pass an empty string to it, but the Entity Framework provider for SQL Server Compact 4.0 returns zero rows for empty strings.</span></span> <span data-ttu-id="01b01-171">Bu nedenle örnek kodu (yerleştirme `Where` deyimi içinde bir `if` deyimi) tüm SQL Server sürümleri için aynı sonuçları elde emin olur.</span><span class="sxs-lookup"><span data-stu-id="01b01-171">Therefore the code in the example (putting the `Where` statement inside an `if` statement) makes sure that you get the same results for all versions of SQL Server.</span></span> <span data-ttu-id="01b01-172">Ayrıca, .NET Framework uygulamasını `Contains` yöntemi varsayılan olarak büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirir, ancak varsayılan olarak Entity Framework SQL Server sağlayıcıları gerçekleştirmek büyük küçük harf duyarsız karşılaştırmalar.</span><span class="sxs-lookup"><span data-stu-id="01b01-172">Also, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but Entity Framework SQL Server providers perform case-insensitive comparisons by default.</span></span> <span data-ttu-id="01b01-173">Bu nedenle, çağırma `ToUpper` test açıkça duyarlı hale getirmek için yöntem sağlar döndüreceği bir depoyu daha sonra kullanmak için kodu değiştirdiğinizde sonuçları değiştirmeyin bir `IEnumerable` koleksiyonu yerine bir `IQueryable` nesne.</span><span class="sxs-lookup"><span data-stu-id="01b01-173">Therefore, calling the `ToUpper` method to make the test explicitly case-insensitive ensures that results do not change when you change the code later to use a repository, which will return an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="01b01-174">(Çağırdığınızda `Contains` metodunda bir `IEnumerable` koleksiyonu, .NET Framework uygulaması alın; çağırdığınızda, üzerinde bir `IQueryable` nesne veritabanı sağlayıcısı uygulamasını edinin.)</span><span class="sxs-lookup"><span data-stu-id="01b01-174">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.)</span></span>
>
> <span data-ttu-id="01b01-175">Null işleme ayrıca farklı veritabanı sağlayıcıları veya kullandığınızda için farklı olabilir bir `IQueryable` nesne karşılaştırma için kullandığınızda bir `IEnumerable` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="01b01-175">Null handling may also be different for different database providers or when you use an `IQueryable` object compared to when you use an `IEnumerable` collection.</span></span> <span data-ttu-id="01b01-176">Örneğin, bazı senaryolarda bir `Where` gibi koşul `table.Column != 0` sahip sütun döndürmeyebilir `null` değeri.</span><span class="sxs-lookup"><span data-stu-id="01b01-176">For example, in some scenarios a `Where` condition such as `table.Column != 0` may not return columns that have `null` as the value.</span></span> <span data-ttu-id="01b01-177">Daha fazla bilgi için [hatalı 'where' yan tümcesinde null değişkenleri işlenmesi](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).</span><span class="sxs-lookup"><span data-stu-id="01b01-177">For more information, see [Incorrect handling of null variables in 'where' clause](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="01b01-178">Bir arama kutusu Öğrenci dizini görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="01b01-178">Add a search box to the Student index view</span></span>

1. <span data-ttu-id="01b01-179">İçinde *Views\Student\Index.cshtml*, açmadan önce hemen vurgulanmış kodu ekleyin `table` resim yazısı, bir metin kutusu oluşturmak için etiket ve **arama** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="01b01-179">In *Views\Student\Index.cshtml*, add the highlighted code immediately before the opening `table` tag in order to create a caption, a text box, and a **Search** button.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. <span data-ttu-id="01b01-180">Çalıştırırsanız, bir arama dizesi girin ve tıklayın **arama** filtreleme çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="01b01-180">Run the page, enter a search string, and click **Search** to verify that filtering is working.</span></span>

   ![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

   <span data-ttu-id="01b01-182">Bu sayfaya yer işareti, yer işareti kullandığınızda, filtrelenmiş liste vermeyecektir, yani "bir" arama dizesi, URL içermiyor dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="01b01-182">Notice the URL doesn't contain the "an" search string, which means that if you bookmark this page, you won't get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="01b01-183">Tam listeyi sıralamak şekilde bu sütun sıralama bağlantıları için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="01b01-183">This applies also to the column sort links, as they will sort the whole list.</span></span> <span data-ttu-id="01b01-184">Değiştireceksiniz **arama** sorgu dizeleri için filtre ölçütlerini öğreticinin ilerleyen bölümlerinde kullanmak için düğme.</span><span class="sxs-lookup"><span data-stu-id="01b01-184">You'll change the **Search** button to use query strings for filter criteria later in the tutorial.</span></span>

## <a name="add-paging-to-the-students-index-page"></a><span data-ttu-id="01b01-185">Disk belleği Öğrenciler dizin sayfasına ekleme</span><span class="sxs-lookup"><span data-stu-id="01b01-185">Add paging to the Students index page</span></span>

<span data-ttu-id="01b01-186">Disk belleği Öğrenciler dizin sayfasına eklemek için yükleyerek başlayacaksınız **PagedList.Mvc** NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="01b01-186">To add paging to the Students index page, you'll start by installing the **PagedList.Mvc** NuGet package.</span></span> <span data-ttu-id="01b01-187">Sonra ek değişiklik yapacaksınız `Index` yöntemi ve disk belleği bağlantılar ekleme `Index` görünümü.</span><span class="sxs-lookup"><span data-stu-id="01b01-187">Then you'll make additional changes in the `Index` method and add paging links to the `Index` view.</span></span> <span data-ttu-id="01b01-188">**PagedList.Mvc** birçok iyi sayfalama ve paketler için ASP.NET MVC sıralamayı biridir ve kullanımını burada yalnızca diğer seçenekleri üzerinde onun için bir öneri olarak değil, örnek olarak tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="01b01-188">**PagedList.Mvc** is one of many good paging and sorting packages for ASP.NET MVC, and its use here is intended only as an example, not as a recommendation for it over other options.</span></span> <span data-ttu-id="01b01-189">Aşağıdaki çizimde, disk belleği bağlantılarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="01b01-189">The following illustration shows the paging links.</span></span>

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a><span data-ttu-id="01b01-191">PagedList.MVC NuGet paketini yükle</span><span class="sxs-lookup"><span data-stu-id="01b01-191">Install the PagedList.MVC NuGet package</span></span>

<span data-ttu-id="01b01-192">NuGet **PagedList.Mvc** paket otomatik olarak yükler **PagedList** paketi bir bağımlılık olarak.</span><span class="sxs-lookup"><span data-stu-id="01b01-192">The NuGet **PagedList.Mvc** package automatically installs the **PagedList** package as a dependency.</span></span> <span data-ttu-id="01b01-193">**PagedList** paketini yükler bir `PagedList` için koleksiyon türü ve uzantısı yöntemleri `IQueryable` ve `IEnumerable` koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="01b01-193">The **PagedList** package installs a `PagedList` collection type and extension methods for `IQueryable` and `IEnumerable` collections.</span></span> <span data-ttu-id="01b01-194">Genişletme yöntemleri verilerin tek bir sayfa oluşturmak bir `PagedList` koleksiyon dışı, `IQueryable` veya `IEnumerable`ve `PagedList` koleksiyonu çeşitli özellikler ve disk belleği kolaylaştıran yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="01b01-194">The extension methods create a single page of data in a `PagedList` collection out of your `IQueryable` or `IEnumerable`, and the `PagedList` collection provides several properties and methods that facilitate paging.</span></span> <span data-ttu-id="01b01-195">**PagedList.Mvc** paket sayfalama düğmeleri görüntüleyen bir disk belleği Yardımcısı yükler.</span><span class="sxs-lookup"><span data-stu-id="01b01-195">The **PagedList.Mvc** package installs a paging helper that displays the paging buttons.</span></span>

1. <span data-ttu-id="01b01-196">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="01b01-196">From the **Tools** menu, select **NuGet Package Manager** and then **Package Manager Console**.</span></span>

2. <span data-ttu-id="01b01-197">İçinde **Paket Yöneticisi Konsolu** penceresinde emin **paket kaynağı** olduğu **nuget.org** ve **varsayılan proje** olan**ContosoUniversity**ve ardından aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="01b01-197">In the **Package Manager Console** window, make sure the **Package source** is **nuget.org** and the **Default project** is **ContosoUniversity**, and then enter the following command:</span></span>

   ```text
   Install-Package PagedList.Mvc
   ```

   ![PagedList.Mvc yükleyin](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

3. <span data-ttu-id="01b01-199">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="01b01-199">Build the project.</span></span>

### <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="01b01-200">Sayfalama işlevselliğinin dizin yöntemine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="01b01-200">Add paging functionality to the Index method</span></span>

1. <span data-ttu-id="01b01-201">İçinde *Controllers\StudentController.cs*, ekleme bir `using` bildirimi `PagedList` ad alanı:</span><span class="sxs-lookup"><span data-stu-id="01b01-201">In *Controllers\StudentController.cs*, add a `using` statement for the `PagedList` namespace:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. <span data-ttu-id="01b01-202">Değiştirin `Index` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="01b01-202">Replace the `Index` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   <span data-ttu-id="01b01-203">Bu kod ekleyen bir `page` parametresi, geçerli bir sıralama sipariş parametresi ve yöntem imzası için geçerli bir filtre parametresi:</span><span class="sxs-lookup"><span data-stu-id="01b01-203">This code adds a `page` parameter, a current sort order parameter, and a current filter parameter to the method signature:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   <span data-ttu-id="01b01-204">Kullanıcı bir disk belleği veya bağlantı sıralama taşınmadığından seçeneğine tıkladıysanız, tüm parametreleri null veya ilk kez sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="01b01-204">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters are null.</span></span> <span data-ttu-id="01b01-205">Disk belleği bağlantıya tıkladıysanız `page` değişkeni görüntülemek için sayfa numarasını içerir.</span><span class="sxs-lookup"><span data-stu-id="01b01-205">If a paging link is clicked, the `page` variable contains the page number to display.</span></span>

   <span data-ttu-id="01b01-206">A `ViewBag` sıralama sırasında disk belleği aynı tutulabilmesi için disk belleği bağlantıları bu eklenmelidir çünkü geçerli sıralama düzenini görünümüyle özelliği sağlar:</span><span class="sxs-lookup"><span data-stu-id="01b01-206">A `ViewBag` property provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   <span data-ttu-id="01b01-207">Başka bir özellik `ViewBag.CurrentFilter`, geçerli bir filtre dizesi ile görünümü sağlar.</span><span class="sxs-lookup"><span data-stu-id="01b01-207">Another property, `ViewBag.CurrentFilter`, provides the view with the current filter string.</span></span> <span data-ttu-id="01b01-208">Bu değer sırasında disk belleği filtre ayarlarını sürdürmek için disk belleği bağlantıları eklenmelidir ve sayfası görüntülendiğinde, metin kutusuna geri yüklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="01b01-208">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span> <span data-ttu-id="01b01-209">Arama dizesi sırasında disk belleği değiştirilirse, yeni filtre görüntülemek için farklı veri kaybına neden çünkü sayfa 1'e sıfırlanması gereken.</span><span class="sxs-lookup"><span data-stu-id="01b01-209">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="01b01-210">Arama dizesi, metin kutusuna girilen değer ve Gönder düğmesine basıldığında değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="01b01-210">The search string is changed when a value is entered in the text box and the submit button is pressed.</span></span> <span data-ttu-id="01b01-211">Bu durumda, `searchString` parametresi null değil.</span><span class="sxs-lookup"><span data-stu-id="01b01-211">In that case, the `searchString` parameter is not null.</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   <span data-ttu-id="01b01-212">Yönteminin sonuna `ToPagedList` Öğrenciler genişletme yöntemini `IQueryable` nesne disk belleği destekleyen bir koleksiyon türü Öğrenci tek sayfalık Öğrenci sorgu dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="01b01-212">At the end of the method, the `ToPagedList` extension method on the students `IQueryable` object converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="01b01-213">Öğrenciler, tek sayfalık sonra görünümüne geçirilir:</span><span class="sxs-lookup"><span data-stu-id="01b01-213">That single page of students is then passed to the view:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   <span data-ttu-id="01b01-214">`ToPagedList` Yöntemi, bir sayfa numarasını alır.</span><span class="sxs-lookup"><span data-stu-id="01b01-214">The `ToPagedList` method takes a page number.</span></span> <span data-ttu-id="01b01-215">İki soru işareti temsil [null birleşim işleci](/dotnet/csharp/language-reference/operators/null-coalescing-operator).</span><span class="sxs-lookup"><span data-stu-id="01b01-215">The two question marks represent the [null-coalescing operator](/dotnet/csharp/language-reference/operators/null-coalescing-operator).</span></span> <span data-ttu-id="01b01-216">Boş değer atanabilir bir tür için varsayılan bir değer null birleşim işleci tanımlar; ifade `(page ?? 1)` anlamına gelir dönüş değerini `page` bir değere sahip veya 1 döndürür, `page` null.</span><span class="sxs-lookup"><span data-stu-id="01b01-216">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

### <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="01b01-217">Öğrenci dizin görünümüne sayfalama bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="01b01-217">Add paging links to the Student index view</span></span>

1. <span data-ttu-id="01b01-218">İçinde *Views\Student\Index.cshtml*, mevcut kodu şu kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="01b01-218">In *Views\Student\Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="01b01-219">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="01b01-219">The changes are highlighted.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   <span data-ttu-id="01b01-220">`@model` Sayfanın üstündeki deyimi belirtir görünüme artık alır bir `PagedList` yerine Nesne bir `List` nesne.</span><span class="sxs-lookup"><span data-stu-id="01b01-220">The `@model` statement at the top of the page specifies that the view now gets a `PagedList` object instead of a `List` object.</span></span>

   <span data-ttu-id="01b01-221">`using` Bildirimi `PagedList.Mvc` erişimi verir MVC yardımcıya için disk belleği düğmeleri.</span><span class="sxs-lookup"><span data-stu-id="01b01-221">The `using` statement for `PagedList.Mvc` gives access to the MVC helper for the paging buttons.</span></span>

   <span data-ttu-id="01b01-222">Kod bir aşırı yüklemesini kullanır [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) belirtmek üzere sağlayan [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).</span><span class="sxs-lookup"><span data-stu-id="01b01-222">The code uses an overload of [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) that allows it to specify [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   <span data-ttu-id="01b01-223">Varsayılan [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) parametreleri HTTP ileti gövdesini ve URL'yi içinde değil sorgu dizeleri geçirilir, yani bir GÖNDERİ ile form verileri gönderir.</span><span class="sxs-lookup"><span data-stu-id="01b01-223">The default [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="01b01-224">HTTP GET belirttiğinizde, form verilerini URL'ye sorgu dizeleri kullanıcıların yer işareti URL'si sağlayan geçirilir.</span><span class="sxs-lookup"><span data-stu-id="01b01-224">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="01b01-225">[HTTP GET kullanımı için W3C yönergeleri](http://www.w3.org/2001/tag/doc/whenToUseGet.html) eylemi bir güncelleştirme olarak sonuçlanmaz olduğunda GET kullanmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="01b01-225">The [W3C guidelines for the use of HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recommend that you should use GET when the action does not result in an update.</span></span>

   <span data-ttu-id="01b01-226">Yeni bir sayfa tıkladığınızda geçerli arama dizesinin görebilmeniz için metin kutusuna geçerli bir arama dizesi ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="01b01-226">The text box is initialized with the current search string so when you click a new page you can see the current search string.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   <span data-ttu-id="01b01-227">Sütun üst bilgisi bağlantıları, kullanıcının içinde filtre sonuçlarını sıralayabilirsiniz, böylece geçerli arama dizesinin denetleyiciye geçirilecek sorgu dizesi kullanın:</span><span class="sxs-lookup"><span data-stu-id="01b01-227">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   <span data-ttu-id="01b01-228">Geçerli sayfayı ve toplam sayfa sayısı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="01b01-228">The current page and total number of pages are displayed.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   <span data-ttu-id="01b01-229">Görüntülenecek sayfa varsa, "Sayfası 0 0" gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="01b01-229">If there are no pages to display, "Page 0 of 0" is shown.</span></span> <span data-ttu-id="01b01-230">(Bu durumda sayfa numarası sayfanın sayısından büyük olduğundan `Model.PageNumber` 1 ' dir ve `Model.PageCount` 0'dır.)</span><span class="sxs-lookup"><span data-stu-id="01b01-230">(In that case the page number is greater than the page count because `Model.PageNumber` is 1, and `Model.PageCount` is 0.)</span></span>

   <span data-ttu-id="01b01-231">Disk belleği düğme tarafından görüntülenen `PagedListPager` yardımcı:</span><span class="sxs-lookup"><span data-stu-id="01b01-231">The paging buttons are displayed by the `PagedListPager` helper:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   <span data-ttu-id="01b01-232">`PagedListPager` Yardımcısı, özelleştirebileceğiniz, stil ve URL'leri dahil olmak üzere birkaç seçenek sağlar.</span><span class="sxs-lookup"><span data-stu-id="01b01-232">The `PagedListPager` helper provides a number of options that you can customize, including URLs and styling.</span></span> <span data-ttu-id="01b01-233">Daha fazla bilgi için [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) GitHub sitesinde.</span><span class="sxs-lookup"><span data-stu-id="01b01-233">For more information, see [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) on the GitHub site.</span></span>

2. <span data-ttu-id="01b01-234">Sayfayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="01b01-234">Run the page.</span></span>

   ![Disk belleği ile Öğrenciler dizin sayfası](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

   <span data-ttu-id="01b01-236">Disk belleği works emin olmak için farklı sıralamalar sayfalama bağlantıları tıklatın.</span><span class="sxs-lookup"><span data-stu-id="01b01-236">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="01b01-237">Ardından bir arama dizesi girin ve yeniden disk belleği de doğru sıralama ve filtreleme ile çalıştığını doğrulamak için disk belleği'ni deneyin.</span><span class="sxs-lookup"><span data-stu-id="01b01-237">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

   ![Öğrenciler, sayfanın arama filtre metni olan dizin](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="01b01-239">Öğrenci istatistiklerini gösteren bir hakkında sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="01b01-239">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="01b01-240">Contoso University sitesinin için sayfa hakkında kaç Öğrenciler her kayıt tarihi için kayıtlı olan görüntüleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="01b01-240">For the Contoso University website's About page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="01b01-241">Bu gruplar üzerinde gruplandırma ve basit hesaplama gerektirir.</span><span class="sxs-lookup"><span data-stu-id="01b01-241">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="01b01-242">Bunu yapmak için aşağıdakileri:</span><span class="sxs-lookup"><span data-stu-id="01b01-242">To accomplish this, you'll do the following:</span></span>

- <span data-ttu-id="01b01-243">Görünüme iletmek için gereken verileri için bir görünüm modeli sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="01b01-243">Create a view model class for the data that you need to pass to the view.</span></span>
- <span data-ttu-id="01b01-244">Değiştirme `About` yönteminde `Home` denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="01b01-244">Modify the `About` method in the `Home` controller.</span></span>
- <span data-ttu-id="01b01-245">Değiştirme `About` görünümü.</span><span class="sxs-lookup"><span data-stu-id="01b01-245">Modify the `About` view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="01b01-246">Görünüm modeli oluşturun</span><span class="sxs-lookup"><span data-stu-id="01b01-246">Create the View Model</span></span>

<span data-ttu-id="01b01-247">Oluşturma bir *Viewmodel'lar* proje klasöründe.</span><span class="sxs-lookup"><span data-stu-id="01b01-247">Create a *ViewModels* folder in the project folder.</span></span> <span data-ttu-id="01b01-248">Bu klasörde bir sınıf dosyası ekleyin *EnrollmentDateGroup.cs* ve şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="01b01-248">In that folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="01b01-249">Giriş denetleyicisini değiştirmek</span><span class="sxs-lookup"><span data-stu-id="01b01-249">Modify the Home Controller</span></span>

1. <span data-ttu-id="01b01-250">İçinde *HomeController.cs*, aşağıdaki `using` deyimini dosyanın üst:</span><span class="sxs-lookup"><span data-stu-id="01b01-250">In *HomeController.cs*, add the following `using` statements at the top of the file:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. <span data-ttu-id="01b01-251">Hemen sınıfı için açılış kaşlı ayracından sonra veritabanı bağlamı için bir sınıf değişkeni ekleyin:</span><span class="sxs-lookup"><span data-stu-id="01b01-251">Add a class variable for the database context immediately after the opening curly brace for the class:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. <span data-ttu-id="01b01-252">Değiştirin `About` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="01b01-252">Replace the `About` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   <span data-ttu-id="01b01-253">LINQ deyiminden Öğrenci varlıkları kayıt tarihe göre gruplar, her grupta varlık sayısını hesaplar ve sonuçları bir koleksiyonda depolar `EnrollmentDateGroup` model nesneleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="01b01-253">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>

4. <span data-ttu-id="01b01-254">Ekleme bir `Dispose` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="01b01-254">Add a `Dispose` method:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a><span data-ttu-id="01b01-255">Değiştirme görünümü hakkında</span><span class="sxs-lookup"><span data-stu-id="01b01-255">Modify the About View</span></span>

1. <span data-ttu-id="01b01-256">Değiştirin *Views\Home\About.cshtml* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="01b01-256">Replace the code in the *Views\Home\About.cshtml* file with the following code:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. <span data-ttu-id="01b01-257">Uygulamayı çalıştırın ve tıklayın **hakkında** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="01b01-257">Run the app and click the **About** link.</span></span>

   <span data-ttu-id="01b01-258">Bir tablodaki her kayıt tarihi için Öğrenci sayısı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="01b01-258">The count of students for each enrollment date displays in a table.</span></span>

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a><span data-ttu-id="01b01-260">Özet</span><span class="sxs-lookup"><span data-stu-id="01b01-260">Summary</span></span>

<span data-ttu-id="01b01-261">Bu öğreticide bir veri modeli oluşturma ve sıralama, filtreleme, sayfalama ve gruplandırma işlevi temel CRUD uygulama gördünüz.</span><span class="sxs-lookup"><span data-stu-id="01b01-261">In this tutorial you've seen how to create a data model and implement basic CRUD, sorting, filtering, paging, and grouping functionality.</span></span> <span data-ttu-id="01b01-262">Sonraki öğreticide, veri modelini genişleterek daha ileri seviyeli konulara arama başlarsınız.</span><span class="sxs-lookup"><span data-stu-id="01b01-262">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span>

<span data-ttu-id="01b01-263">Lütfen bu öğreticide sevmediğinizi nasıl ve ne geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="01b01-263">Please leave feedback on how you liked this tutorial and what we could improve.</span></span>

<span data-ttu-id="01b01-264">Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET veri erişimi - önerilen kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="01b01-264">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="01b01-265">[Önceki](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [İleri](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="01b01-265">[Previous](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[Next](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
