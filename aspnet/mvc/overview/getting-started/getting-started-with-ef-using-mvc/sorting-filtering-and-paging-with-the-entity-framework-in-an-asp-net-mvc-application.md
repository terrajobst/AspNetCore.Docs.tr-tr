---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Öğretici: Sıralama, filtreleme ve bir ASP.NET MVC uygulamasındaki Entity Framework ile sayfalama ekleme | Microsoft Docs'
author: tdykstra
description: Bu öğreticide, sıralama, filtreleme ve sayfalama işlevsellik eklemek **Öğrenciler** dizin sayfası. Bir basit gruplandırma sayfa oluşturabilir.
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 1f18a15d39d58ffb4ac48cfccee6519d33294e85
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444201"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="d457b-104">Öğretici: Sıralama, filtreleme ve bir ASP.NET MVC uygulamasındaki Entity Framework ile sayfalama ekleme</span><span class="sxs-lookup"><span data-stu-id="d457b-104">Tutorial: Add sorting, filtering, and paging with the Entity Framework in an ASP.NET MVC application</span></span>

<span data-ttu-id="d457b-105">İçinde [önceki öğreticide](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), web sayfaları için temel CRUD işlemleri için bir dizi uygulanan `Student` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="d457b-105">In the [previous tutorial](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), you implemented a set of web pages for basic CRUD operations for `Student` entities.</span></span> <span data-ttu-id="d457b-106">Bu öğreticide, sıralama, filtreleme ve sayfalama işlevsellik eklemek **Öğrenciler** dizin sayfası.</span><span class="sxs-lookup"><span data-stu-id="d457b-106">In this tutorial you add sorting, filtering, and paging functionality to the **Students** Index page.</span></span> <span data-ttu-id="d457b-107">Bir basit gruplandırma sayfa oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="d457b-107">You also create a simple grouping page.</span></span>

<span data-ttu-id="d457b-108">Aşağıdaki görüntüde, hazır olduğunuzda sayfanın nasıl görüneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="d457b-108">The following image shows what the page will look like when you're done.</span></span> <span data-ttu-id="d457b-109">Sütun başlıkları, kullanıcının sütuna göre sıralamak için tıklayabileceği bağlantılar verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d457b-109">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="d457b-110">Bir sütun başlığına tekrar tekrar tıklayarak, artan veya azalan sıralama düzeni arasında geçiş yapar.</span><span class="sxs-lookup"><span data-stu-id="d457b-110">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="d457b-112">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="d457b-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d457b-113">Sütun sıralama bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="d457b-113">Add column sort links</span></span>
> * <span data-ttu-id="d457b-114">Bir arama kutusu ekleme</span><span class="sxs-lookup"><span data-stu-id="d457b-114">Add a Search box</span></span>
> * <span data-ttu-id="d457b-115">Sayfalama ekleme</span><span class="sxs-lookup"><span data-stu-id="d457b-115">Add paging</span></span>
> * <span data-ttu-id="d457b-116">Hakkında sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="d457b-116">Create an About page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d457b-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="d457b-117">Prerequisites</span></span>

* [<span data-ttu-id="d457b-118">Temel CRUD İşlevselliği Uygulama</span><span class="sxs-lookup"><span data-stu-id="d457b-118">Implementing Basic CRUD Functionality</span></span>](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a><span data-ttu-id="d457b-119">Sütun sıralama bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="d457b-119">Add column sort links</span></span>

<span data-ttu-id="d457b-120">Öğrenci dizin sayfasına sıralama eklemek için değiştireceksiniz `Index` yöntemi `Student` denetleyicisi ve kodu ekleyin `Student` dizin görünümü.</span><span class="sxs-lookup"><span data-stu-id="d457b-120">To add sorting to the Student Index page, you'll change the `Index` method of the `Student` controller and add code to the `Student` Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="d457b-121">Sıralama işlevleri dizin yöntemine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d457b-121">Add sorting functionality to the Index method</span></span>

- <span data-ttu-id="d457b-122">İçinde *Controllers\StudentController.cs*, değiştirin `Index` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="d457b-122">In *Controllers\StudentController.cs*, replace the `Index` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="d457b-123">Bu kod alır bir `sortOrder` URL'ye sorgu dizesi parametresi.</span><span class="sxs-lookup"><span data-stu-id="d457b-123">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="d457b-124">Sorgu dizesi değerini eylem yönteminin bir parametresi olarak ASP.NET MVC tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d457b-124">The query string value is provided by ASP.NET MVC as a parameter to the action method.</span></span> <span data-ttu-id="d457b-125">Ardından isteğe bağlı olarak bir alt çizgi ve azalan düzende belirtmek için "desc" dize "Name" veya "Tarih" olan dizesi parametresidir.</span><span class="sxs-lookup"><span data-stu-id="d457b-125">The parameter is a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="d457b-126">Varsayılan sıralama artan düzendedir.</span><span class="sxs-lookup"><span data-stu-id="d457b-126">The default sort order is ascending.</span></span>

<span data-ttu-id="d457b-127">Dizin Sayfası istendi, ilk kez hiçbir sorgu dizesi yoktur.</span><span class="sxs-lookup"><span data-stu-id="d457b-127">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="d457b-128">Öğrenciler tarafından artan düzende görüntülenen `LastName`, başarısızlık durumunda tarafından belirlenen varsayılan değerdir `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="d457b-128">The students are displayed in ascending order by `LastName`, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="d457b-129">Kullanıcı uygun sütun başlığına köprü tıkladığında `sortOrder` değeri, sorgu dizesinde sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d457b-129">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="d457b-130">İki `ViewBag` görünüm sütun başlığı köprüler uygun sorgu dizesi değerleri yapılandırabilirsiniz. böylece değişkenler kullanılır:</span><span class="sxs-lookup"><span data-stu-id="d457b-130">The two `ViewBag` variables are used so that the view can configure the column heading hyperlinks with the appropriate query string values:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="d457b-131">Üçlü deyimleri şunlardır.</span><span class="sxs-lookup"><span data-stu-id="d457b-131">These are ternary statements.</span></span> <span data-ttu-id="d457b-132">Olmadığını birincinin belirtir `sortOrder` parametresi null veya boş `ViewBag.NameSortParm` ayarlanması gerekir "adı\_desc"; Aksi takdirde boş bir dize olarak ayarlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d457b-132">The first one specifies that if the `sortOrder` parameter is null or empty, `ViewBag.NameSortParm` should be set to "name\_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="d457b-133">Bu iki deyimden sütun başlığı köprüler şu şekilde ayarlayın görünümün etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="d457b-133">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

| <span data-ttu-id="d457b-134">Geçerli bir sıralama düzeni</span><span class="sxs-lookup"><span data-stu-id="d457b-134">Current sort order</span></span> | <span data-ttu-id="d457b-135">Son adı köprü</span><span class="sxs-lookup"><span data-stu-id="d457b-135">Last Name Hyperlink</span></span> | <span data-ttu-id="d457b-136">Tarih köprü</span><span class="sxs-lookup"><span data-stu-id="d457b-136">Date Hyperlink</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d457b-137">Adı artan en son</span><span class="sxs-lookup"><span data-stu-id="d457b-137">Last Name ascending</span></span> | <span data-ttu-id="d457b-138">descending</span><span class="sxs-lookup"><span data-stu-id="d457b-138">descending</span></span> | <span data-ttu-id="d457b-139">ascending</span><span class="sxs-lookup"><span data-stu-id="d457b-139">ascending</span></span> |
| <span data-ttu-id="d457b-140">Son azalan düzende ad</span><span class="sxs-lookup"><span data-stu-id="d457b-140">Last Name descending</span></span> | <span data-ttu-id="d457b-141">ascending</span><span class="sxs-lookup"><span data-stu-id="d457b-141">ascending</span></span> | <span data-ttu-id="d457b-142">ascending</span><span class="sxs-lookup"><span data-stu-id="d457b-142">ascending</span></span> |
| <span data-ttu-id="d457b-143">Artan tarihi</span><span class="sxs-lookup"><span data-stu-id="d457b-143">Date ascending</span></span> | <span data-ttu-id="d457b-144">ascending</span><span class="sxs-lookup"><span data-stu-id="d457b-144">ascending</span></span> | <span data-ttu-id="d457b-145">descending</span><span class="sxs-lookup"><span data-stu-id="d457b-145">descending</span></span> |
| <span data-ttu-id="d457b-146">Azalan düzende tarihi</span><span class="sxs-lookup"><span data-stu-id="d457b-146">Date descending</span></span> | <span data-ttu-id="d457b-147">ascending</span><span class="sxs-lookup"><span data-stu-id="d457b-147">ascending</span></span> | <span data-ttu-id="d457b-148">ascending</span><span class="sxs-lookup"><span data-stu-id="d457b-148">ascending</span></span> |

<span data-ttu-id="d457b-149">Yöntemini kullanan [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) göre sıralamak için sütun belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="d457b-149">The method uses [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) to specify the column to sort by.</span></span> <span data-ttu-id="d457b-150">Kod oluşturur bir <xref:System.Linq.IQueryable%601> önce değişken `switch` ifadesi, değiştiren içinde `switch` deyimi ve çağrılarını `ToList` sonrasına `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="d457b-150">The code creates an <xref:System.Linq.IQueryable%601> variable before the `switch` statement, modifies it in the `switch` statement, and calls the `ToList` method after the `switch` statement.</span></span> <span data-ttu-id="d457b-151">Ne zaman oluşturma ve değiştirme `IQueryable` değişkenleri, sorgu veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="d457b-151">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="d457b-152">Sorgu, dönüştürmek kadar yürütülmez `IQueryable` nesne gibi bir yöntem çağırarak koleksiyonuna `ToList`.</span><span class="sxs-lookup"><span data-stu-id="d457b-152">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToList`.</span></span> <span data-ttu-id="d457b-153">Bu nedenle, bu kod, kadar yürütülmez tek bir sorgu sonuçları `return View` deyimi.</span><span class="sxs-lookup"><span data-stu-id="d457b-153">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="d457b-154">Her bir sıralama düzeni için farklı LINQ deyimleri yazılırken alternatif olarak, dinamik olarak bir LINQ ifadesini oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d457b-154">As an alternative to writing different LINQ statements for each sort order, you can dynamically create a LINQ statement.</span></span> <span data-ttu-id="d457b-155">Dinamik LINQ hakkında daha fazla bilgi için bkz. [dinamik LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).</span><span class="sxs-lookup"><span data-stu-id="d457b-155">For information about dynamic LINQ, see [Dynamic LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="d457b-156">Öğrenci dizin görünüme sütun başlık köprüler ekleyin</span><span class="sxs-lookup"><span data-stu-id="d457b-156">Add column heading hyperlinks to the Student index view</span></span>

1. <span data-ttu-id="d457b-157">İçinde *Views\Student\Index.cshtml*, değiştirin `<tr>` ve `<th>` vurgulanmış kodu başlık satırı için öğeleri:</span><span class="sxs-lookup"><span data-stu-id="d457b-157">In *Views\Student\Index.cshtml*, replace the `<tr>` and `<th>` elements for the heading row with the highlighted code:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   <span data-ttu-id="d457b-158">Bilgiler, bu kodu kullanan `ViewBag` uygun sorgu köprülerle ayarlamak için özellikler dize değerleri.</span><span class="sxs-lookup"><span data-stu-id="d457b-158">This code uses the information in the `ViewBag` properties to set up hyperlinks with the appropriate query string values.</span></span>

2. <span data-ttu-id="d457b-159">Sayfayı çalıştırın ve tıklayın **Soyadı** ve **kayıt tarihi** sütun başlıkları, sıralama doğrulamak için çalışır.</span><span class="sxs-lookup"><span data-stu-id="d457b-159">Run the page and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

   <span data-ttu-id="d457b-160">Tıkladıktan sonra **Soyadı** başlığı Öğrenciler soyadına göre azalan düzende görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d457b-160">After you click the **Last Name** heading, students are displayed in descending last name order.</span></span>

## <a name="add-a-search-box"></a><span data-ttu-id="d457b-161">Bir arama kutusu ekleme</span><span class="sxs-lookup"><span data-stu-id="d457b-161">Add a Search box</span></span>

<span data-ttu-id="d457b-162">Öğrenciler dizin sayfasına filtre eklemek için görünümü için metin kutusu ve bir Gönder düğmesi ekleyin ve karşılık gelen değişiklik `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d457b-162">To add filtering to the Students index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="d457b-163">Metin kutusu için ad ve soyadı alanları aramak için bir dize girmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="d457b-163">The text box lets you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="d457b-164">Filtreleme işlevselliği dizin yöntemine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d457b-164">Add filtering functionality to the Index method</span></span>

- <span data-ttu-id="d457b-165">İçinde *Controllers\StudentController.cs*, değiştirin `Index` yöntemini aşağıdaki kodla (değişiklikleri vurgulanır):</span><span class="sxs-lookup"><span data-stu-id="d457b-165">In *Controllers\StudentController.cs*, replace the `Index` method with the following code (the changes are highlighted):</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

<span data-ttu-id="d457b-166">Kod ekler bir `searchString` parametresi `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d457b-166">The code adds a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="d457b-167">Dizin görünümüne ekleyeceksiniz bir metin kutusundan arama dizesi değeri alındı.</span><span class="sxs-lookup"><span data-stu-id="d457b-167">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="d457b-168">Ayrıca ekler bir `where` yan tümcesine yalnızca Öğrenciler, ad ve Soyadı arama dizesini içeren seçer LINQ ifadesi.</span><span class="sxs-lookup"><span data-stu-id="d457b-168">It also adds a `where` clause to the LINQ statement that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="d457b-169">Ekler deyimi <xref:System.Linq.Queryable.Where%2A> yan tümcesi yalnızca aramak için bir değer ise yürütür.</span><span class="sxs-lookup"><span data-stu-id="d457b-169">The statement that adds the <xref:System.Linq.Queryable.Where%2A> clause executes only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="d457b-170">Çoğu durumda bir Entity Framework varlık kümesini veya bir bellek içi koleksiyonunda bir genişletme yöntemi olarak aynı yöntemi çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d457b-170">In many cases you can call the same method either on an Entity Framework entity set or as an extension method on an in-memory collection.</span></span> <span data-ttu-id="d457b-171">Sonuçları normalde aynıdır, ancak bazı durumlarda farklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="d457b-171">The results are normally the same but in some cases may be different.</span></span>
>
> <span data-ttu-id="d457b-172">Örneğin, .NET Framework uygulamasını `Contains` yöntem boş bir dizeyi geçirmek, ancak SQL Server Compact 4.0 için Entity Framework sağlayıcısı boş dizeler için sıfır satır döndürür. tüm satırları döndürür.</span><span class="sxs-lookup"><span data-stu-id="d457b-172">For example, the .NET Framework implementation of the `Contains` method returns all rows when you pass an empty string to it, but the Entity Framework provider for SQL Server Compact 4.0 returns zero rows for empty strings.</span></span> <span data-ttu-id="d457b-173">Bu nedenle örnek kodu (yerleştirme `Where` deyimi içinde bir `if` deyimi) tüm SQL Server sürümleri için aynı sonuçları elde emin olur.</span><span class="sxs-lookup"><span data-stu-id="d457b-173">Therefore the code in the example (putting the `Where` statement inside an `if` statement) makes sure that you get the same results for all versions of SQL Server.</span></span> <span data-ttu-id="d457b-174">Ayrıca, .NET Framework uygulamasını `Contains` yöntemi varsayılan olarak büyük küçük harfe duyarlı bir karşılaştırma gerçekleştirir, ancak varsayılan olarak Entity Framework SQL Server sağlayıcıları gerçekleştirmek büyük küçük harf duyarsız karşılaştırmalar.</span><span class="sxs-lookup"><span data-stu-id="d457b-174">Also, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but Entity Framework SQL Server providers perform case-insensitive comparisons by default.</span></span> <span data-ttu-id="d457b-175">Bu nedenle, çağırma `ToUpper` test açıkça duyarlı hale getirmek için yöntem sağlar döndüreceği bir depoyu daha sonra kullanmak için kodu değiştirdiğinizde sonuçları değiştirmeyin bir `IEnumerable` koleksiyonu yerine bir `IQueryable` nesne.</span><span class="sxs-lookup"><span data-stu-id="d457b-175">Therefore, calling the `ToUpper` method to make the test explicitly case-insensitive ensures that results do not change when you change the code later to use a repository, which will return an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="d457b-176">(Çağırdığınızda `Contains` metodunda bir `IEnumerable` koleksiyonu, .NET Framework uygulaması alın; çağırdığınızda, üzerinde bir `IQueryable` nesne veritabanı sağlayıcısı uygulamasını edinin.)</span><span class="sxs-lookup"><span data-stu-id="d457b-176">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.)</span></span>
>
> <span data-ttu-id="d457b-177">Null işleme ayrıca farklı veritabanı sağlayıcıları veya kullandığınızda için farklı olabilir bir `IQueryable` nesne karşılaştırma için kullandığınızda bir `IEnumerable` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="d457b-177">Null handling may also be different for different database providers or when you use an `IQueryable` object compared to when you use an `IEnumerable` collection.</span></span> <span data-ttu-id="d457b-178">Örneğin, bazı senaryolarda bir `Where` gibi koşul `table.Column != 0` sahip sütun döndürmeyebilir `null` değeri.</span><span class="sxs-lookup"><span data-stu-id="d457b-178">For example, in some scenarios a `Where` condition such as `table.Column != 0` may not return columns that have `null` as the value.</span></span> <span data-ttu-id="d457b-179">Daha fazla bilgi için [hatalı 'where' yan tümcesinde null değişkenleri işlenmesi](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).</span><span class="sxs-lookup"><span data-stu-id="d457b-179">For more information, see [Incorrect handling of null variables in 'where' clause](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="d457b-180">Bir arama kutusu Öğrenci dizini görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="d457b-180">Add a search box to the Student index view</span></span>

1. <span data-ttu-id="d457b-181">İçinde *Views\Student\Index.cshtml*, açmadan önce hemen vurgulanmış kodu ekleyin `table` resim yazısı, bir metin kutusu oluşturmak için etiket ve **arama** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="d457b-181">In *Views\Student\Index.cshtml*, add the highlighted code immediately before the opening `table` tag in order to create a caption, a text box, and a **Search** button.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. <span data-ttu-id="d457b-182">Çalıştırırsanız, bir arama dizesi girin ve tıklayın **arama** filtreleme çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="d457b-182">Run the page, enter a search string, and click **Search** to verify that filtering is working.</span></span>

   <span data-ttu-id="d457b-183">Bu sayfaya yer işareti, yer işareti kullandığınızda, filtrelenmiş liste vermeyecektir, yani "bir" arama dizesi, URL içermiyor dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="d457b-183">Notice the URL doesn't contain the "an" search string, which means that if you bookmark this page, you won't get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="d457b-184">Tam listeyi sıralamak şekilde bu sütun sıralama bağlantıları için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="d457b-184">This applies also to the column sort links, as they will sort the whole list.</span></span> <span data-ttu-id="d457b-185">Değiştireceksiniz **arama** sorgu dizeleri için filtre ölçütlerini öğreticinin ilerleyen bölümlerinde kullanmak için düğme.</span><span class="sxs-lookup"><span data-stu-id="d457b-185">You'll change the **Search** button to use query strings for filter criteria later in the tutorial.</span></span>

## <a name="add-paging"></a><span data-ttu-id="d457b-186">Sayfalama ekleme</span><span class="sxs-lookup"><span data-stu-id="d457b-186">Add paging</span></span>

<span data-ttu-id="d457b-187">Disk belleği Öğrenciler dizin sayfasına eklemek için yükleyerek başlayacaksınız **PagedList.Mvc** NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="d457b-187">To add paging to the Students index page, you'll start by installing the **PagedList.Mvc** NuGet package.</span></span> <span data-ttu-id="d457b-188">Sonra ek değişiklik yapacaksınız `Index` yöntemi ve disk belleği bağlantılar ekleme `Index` görünümü.</span><span class="sxs-lookup"><span data-stu-id="d457b-188">Then you'll make additional changes in the `Index` method and add paging links to the `Index` view.</span></span> <span data-ttu-id="d457b-189">**PagedList.Mvc** birçok iyi sayfalama ve paketler için ASP.NET MVC sıralamayı biridir ve kullanımını burada yalnızca diğer seçenekleri üzerinde onun için bir öneri olarak değil, örnek olarak tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="d457b-189">**PagedList.Mvc** is one of many good paging and sorting packages for ASP.NET MVC, and its use here is intended only as an example, not as a recommendation for it over other options.</span></span>

### <a name="install-the-pagedlistmvc-nuget-package"></a><span data-ttu-id="d457b-190">PagedList.MVC NuGet paketini yükle</span><span class="sxs-lookup"><span data-stu-id="d457b-190">Install the PagedList.MVC NuGet package</span></span>

<span data-ttu-id="d457b-191">NuGet **PagedList.Mvc** paket otomatik olarak yükler **PagedList** paketi bir bağımlılık olarak.</span><span class="sxs-lookup"><span data-stu-id="d457b-191">The NuGet **PagedList.Mvc** package automatically installs the **PagedList** package as a dependency.</span></span> <span data-ttu-id="d457b-192">**PagedList** paketini yükler bir `PagedList` için koleksiyon türü ve uzantısı yöntemleri `IQueryable` ve `IEnumerable` koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="d457b-192">The **PagedList** package installs a `PagedList` collection type and extension methods for `IQueryable` and `IEnumerable` collections.</span></span> <span data-ttu-id="d457b-193">Genişletme yöntemleri verilerin tek bir sayfa oluşturmak bir `PagedList` koleksiyon dışı, `IQueryable` veya `IEnumerable`ve `PagedList` koleksiyonu çeşitli özellikler ve disk belleği kolaylaştıran yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="d457b-193">The extension methods create a single page of data in a `PagedList` collection out of your `IQueryable` or `IEnumerable`, and the `PagedList` collection provides several properties and methods that facilitate paging.</span></span> <span data-ttu-id="d457b-194">**PagedList.Mvc** paket sayfalama düğmeleri görüntüleyen bir disk belleği Yardımcısı yükler.</span><span class="sxs-lookup"><span data-stu-id="d457b-194">The **PagedList.Mvc** package installs a paging helper that displays the paging buttons.</span></span>

1. <span data-ttu-id="d457b-195">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="d457b-195">From the **Tools** menu, select **NuGet Package Manager** and then **Package Manager Console**.</span></span>

2. <span data-ttu-id="d457b-196">İçinde **Paket Yöneticisi Konsolu** penceresinde emin **paket kaynağı** olduğu **nuget.org** ve **varsayılan proje** olan**ContosoUniversity**ve ardından aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="d457b-196">In the **Package Manager Console** window, make sure the **Package source** is **nuget.org** and the **Default project** is **ContosoUniversity**, and then enter the following command:</span></span>

   ```text
   Install-Package PagedList.Mvc
   ```

3. <span data-ttu-id="d457b-197">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d457b-197">Build the project.</span></span>

### <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="d457b-198">Sayfalama işlevselliğinin dizin yöntemine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d457b-198">Add paging functionality to the Index method</span></span>

1. <span data-ttu-id="d457b-199">İçinde *Controllers\StudentController.cs*, ekleme bir `using` bildirimi `PagedList` ad alanı:</span><span class="sxs-lookup"><span data-stu-id="d457b-199">In *Controllers\StudentController.cs*, add a `using` statement for the `PagedList` namespace:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. <span data-ttu-id="d457b-200">Değiştirin `Index` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="d457b-200">Replace the `Index` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   <span data-ttu-id="d457b-201">Bu kod ekleyen bir `page` parametresi, geçerli bir sıralama sipariş parametresi ve yöntem imzası için geçerli bir filtre parametresi:</span><span class="sxs-lookup"><span data-stu-id="d457b-201">This code adds a `page` parameter, a current sort order parameter, and a current filter parameter to the method signature:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   <span data-ttu-id="d457b-202">Kullanıcı bir disk belleği veya bağlantı sıralama taşınmadığından seçeneğine tıkladıysanız, tüm parametreleri null veya ilk kez sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d457b-202">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters are null.</span></span> <span data-ttu-id="d457b-203">Disk belleği bağlantıya tıkladıysanız `page` değişkeni görüntülemek için sayfa numarasını içerir.</span><span class="sxs-lookup"><span data-stu-id="d457b-203">If a paging link is clicked, the `page` variable contains the page number to display.</span></span>

   <span data-ttu-id="d457b-204">A `ViewBag` sıralama sırasında disk belleği aynı tutulabilmesi için disk belleği bağlantıları bu eklenmelidir çünkü geçerli sıralama düzenini görünümüyle özelliği sağlar:</span><span class="sxs-lookup"><span data-stu-id="d457b-204">A `ViewBag` property provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   <span data-ttu-id="d457b-205">Başka bir özellik `ViewBag.CurrentFilter`, geçerli bir filtre dizesi ile görünümü sağlar.</span><span class="sxs-lookup"><span data-stu-id="d457b-205">Another property, `ViewBag.CurrentFilter`, provides the view with the current filter string.</span></span> <span data-ttu-id="d457b-206">Bu değer sırasında disk belleği filtre ayarlarını sürdürmek için disk belleği bağlantıları eklenmelidir ve sayfası görüntülendiğinde, metin kutusuna geri yüklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="d457b-206">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span> <span data-ttu-id="d457b-207">Arama dizesi sırasında disk belleği değiştirilirse, yeni filtre görüntülemek için farklı veri kaybına neden çünkü sayfa 1'e sıfırlanması gereken.</span><span class="sxs-lookup"><span data-stu-id="d457b-207">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="d457b-208">Arama dizesi, metin kutusuna girilen değer ve Gönder düğmesine basıldığında değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="d457b-208">The search string is changed when a value is entered in the text box and the submit button is pressed.</span></span> <span data-ttu-id="d457b-209">Bu durumda, `searchString` parametresi null değil.</span><span class="sxs-lookup"><span data-stu-id="d457b-209">In that case, the `searchString` parameter is not null.</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   <span data-ttu-id="d457b-210">Yönteminin sonuna `ToPagedList` Öğrenciler genişletme yöntemini `IQueryable` nesne disk belleği destekleyen bir koleksiyon türü Öğrenci tek sayfalık Öğrenci sorgu dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="d457b-210">At the end of the method, the `ToPagedList` extension method on the students `IQueryable` object converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="d457b-211">Öğrenciler, tek sayfalık sonra görünümüne geçirilir:</span><span class="sxs-lookup"><span data-stu-id="d457b-211">That single page of students is then passed to the view:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   <span data-ttu-id="d457b-212">`ToPagedList` Yöntemi, bir sayfa numarasını alır.</span><span class="sxs-lookup"><span data-stu-id="d457b-212">The `ToPagedList` method takes a page number.</span></span> <span data-ttu-id="d457b-213">İki soru işareti temsil [null birleşim işleci](/dotnet/csharp/language-reference/operators/null-coalescing-operator).</span><span class="sxs-lookup"><span data-stu-id="d457b-213">The two question marks represent the [null-coalescing operator](/dotnet/csharp/language-reference/operators/null-coalescing-operator).</span></span> <span data-ttu-id="d457b-214">Boş değer atanabilir bir tür için varsayılan bir değer null birleşim işleci tanımlar; ifade `(page ?? 1)` anlamına gelir dönüş değerini `page` bir değere sahip veya 1 döndürür, `page` null.</span><span class="sxs-lookup"><span data-stu-id="d457b-214">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

### <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="d457b-215">Öğrenci dizin görünümüne sayfalama bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="d457b-215">Add paging links to the Student index view</span></span>

1. <span data-ttu-id="d457b-216">İçinde *Views\Student\Index.cshtml*, mevcut kodu şu kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d457b-216">In *Views\Student\Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="d457b-217">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="d457b-217">The changes are highlighted.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   <span data-ttu-id="d457b-218">`@model` Sayfanın üstündeki deyimi belirtir görünüme artık alır bir `PagedList` yerine Nesne bir `List` nesne.</span><span class="sxs-lookup"><span data-stu-id="d457b-218">The `@model` statement at the top of the page specifies that the view now gets a `PagedList` object instead of a `List` object.</span></span>

   <span data-ttu-id="d457b-219">`using` Bildirimi `PagedList.Mvc` erişimi verir MVC yardımcıya için disk belleği düğmeleri.</span><span class="sxs-lookup"><span data-stu-id="d457b-219">The `using` statement for `PagedList.Mvc` gives access to the MVC helper for the paging buttons.</span></span>

   <span data-ttu-id="d457b-220">Kod bir aşırı yüklemesini kullanır [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) belirtmek üzere sağlayan [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).</span><span class="sxs-lookup"><span data-stu-id="d457b-220">The code uses an overload of [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) that allows it to specify [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   <span data-ttu-id="d457b-221">Varsayılan [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) parametreleri HTTP ileti gövdesini ve URL'yi içinde değil sorgu dizeleri geçirilir, yani bir GÖNDERİ ile form verileri gönderir.</span><span class="sxs-lookup"><span data-stu-id="d457b-221">The default [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="d457b-222">HTTP GET belirttiğinizde, form verilerini URL'ye sorgu dizeleri kullanıcıların yer işareti URL'si sağlayan geçirilir.</span><span class="sxs-lookup"><span data-stu-id="d457b-222">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="d457b-223">[HTTP GET kullanımı için W3C yönergeleri](http://www.w3.org/2001/tag/doc/whenToUseGet.html) eylemi bir güncelleştirme olarak sonuçlanmaz olduğunda GET kullanmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="d457b-223">The [W3C guidelines for the use of HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recommend that you should use GET when the action does not result in an update.</span></span>

   <span data-ttu-id="d457b-224">Yeni bir sayfa tıkladığınızda geçerli arama dizesinin görebilmeniz için metin kutusuna geçerli bir arama dizesi ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="d457b-224">The text box is initialized with the current search string so when you click a new page you can see the current search string.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   <span data-ttu-id="d457b-225">Sütun üst bilgisi bağlantıları, kullanıcının içinde filtre sonuçlarını sıralayabilirsiniz, böylece geçerli arama dizesinin denetleyiciye geçirilecek sorgu dizesi kullanın:</span><span class="sxs-lookup"><span data-stu-id="d457b-225">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   <span data-ttu-id="d457b-226">Geçerli sayfayı ve toplam sayfa sayısı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d457b-226">The current page and total number of pages are displayed.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   <span data-ttu-id="d457b-227">Görüntülenecek sayfa varsa, "Sayfası 0 0" gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d457b-227">If there are no pages to display, "Page 0 of 0" is shown.</span></span> <span data-ttu-id="d457b-228">(Bu durumda sayfa numarası sayfanın sayısından büyük olduğundan `Model.PageNumber` 1 ' dir ve `Model.PageCount` 0'dır.)</span><span class="sxs-lookup"><span data-stu-id="d457b-228">(In that case the page number is greater than the page count because `Model.PageNumber` is 1, and `Model.PageCount` is 0.)</span></span>

   <span data-ttu-id="d457b-229">Disk belleği düğme tarafından görüntülenen `PagedListPager` yardımcı:</span><span class="sxs-lookup"><span data-stu-id="d457b-229">The paging buttons are displayed by the `PagedListPager` helper:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   <span data-ttu-id="d457b-230">`PagedListPager` Yardımcısı, özelleştirebileceğiniz, stil ve URL'leri dahil olmak üzere birkaç seçenek sağlar.</span><span class="sxs-lookup"><span data-stu-id="d457b-230">The `PagedListPager` helper provides a number of options that you can customize, including URLs and styling.</span></span> <span data-ttu-id="d457b-231">Daha fazla bilgi için [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) GitHub sitesinde.</span><span class="sxs-lookup"><span data-stu-id="d457b-231">For more information, see [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) on the GitHub site.</span></span>

2. <span data-ttu-id="d457b-232">Sayfayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d457b-232">Run the page.</span></span>

   <span data-ttu-id="d457b-233">Disk belleği works emin olmak için farklı sıralamalar sayfalama bağlantıları tıklatın.</span><span class="sxs-lookup"><span data-stu-id="d457b-233">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="d457b-234">Ardından bir arama dizesi girin ve yeniden disk belleği de doğru sıralama ve filtreleme ile çalıştığını doğrulamak için disk belleği'ni deneyin.</span><span class="sxs-lookup"><span data-stu-id="d457b-234">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page"></a><span data-ttu-id="d457b-235">Hakkında sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="d457b-235">Create an About page</span></span>

<span data-ttu-id="d457b-236">Contoso University sitesinin için sayfa hakkında kaç Öğrenciler her kayıt tarihi için kayıtlı olan görüntüleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="d457b-236">For the Contoso University website's About page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="d457b-237">Bu gruplar üzerinde gruplandırma ve basit hesaplama gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d457b-237">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="d457b-238">Bunu yapmak için aşağıdakileri:</span><span class="sxs-lookup"><span data-stu-id="d457b-238">To accomplish this, you'll do the following:</span></span>

- <span data-ttu-id="d457b-239">Görünüme iletmek için gereken verileri için bir görünüm modeli sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d457b-239">Create a view model class for the data that you need to pass to the view.</span></span>
- <span data-ttu-id="d457b-240">Değiştirme `About` yönteminde `Home` denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="d457b-240">Modify the `About` method in the `Home` controller.</span></span>
- <span data-ttu-id="d457b-241">Değiştirme `About` görünümü.</span><span class="sxs-lookup"><span data-stu-id="d457b-241">Modify the `About` view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="d457b-242">Görünüm modeli oluşturun</span><span class="sxs-lookup"><span data-stu-id="d457b-242">Create the View Model</span></span>

<span data-ttu-id="d457b-243">Oluşturma bir *Viewmodel'lar* proje klasöründe.</span><span class="sxs-lookup"><span data-stu-id="d457b-243">Create a *ViewModels* folder in the project folder.</span></span> <span data-ttu-id="d457b-244">Bu klasörde bir sınıf dosyası ekleyin *EnrollmentDateGroup.cs* ve şablon kodunu aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d457b-244">In that folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="d457b-245">Giriş denetleyicisini değiştirmek</span><span class="sxs-lookup"><span data-stu-id="d457b-245">Modify the Home Controller</span></span>

1. <span data-ttu-id="d457b-246">İçinde *HomeController.cs*, aşağıdaki `using` deyimini dosyanın üst:</span><span class="sxs-lookup"><span data-stu-id="d457b-246">In *HomeController.cs*, add the following `using` statements at the top of the file:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. <span data-ttu-id="d457b-247">Hemen sınıfı için açılış kaşlı ayracından sonra veritabanı bağlamı için bir sınıf değişkeni ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d457b-247">Add a class variable for the database context immediately after the opening curly brace for the class:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. <span data-ttu-id="d457b-248">Değiştirin `About` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="d457b-248">Replace the `About` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   <span data-ttu-id="d457b-249">LINQ deyiminden Öğrenci varlıkları kayıt tarihe göre gruplar, her grupta varlık sayısını hesaplar ve sonuçları bir koleksiyonda depolar `EnrollmentDateGroup` model nesneleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="d457b-249">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>

4. <span data-ttu-id="d457b-250">Ekleme bir `Dispose` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="d457b-250">Add a `Dispose` method:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a><span data-ttu-id="d457b-251">Değiştirme görünümü hakkında</span><span class="sxs-lookup"><span data-stu-id="d457b-251">Modify the About View</span></span>

1. <span data-ttu-id="d457b-252">Değiştirin *Views\Home\About.cshtml* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="d457b-252">Replace the code in the *Views\Home\About.cshtml* file with the following code:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. <span data-ttu-id="d457b-253">Uygulamayı çalıştırın ve tıklayın **hakkında** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="d457b-253">Run the app and click the **About** link.</span></span>

   <span data-ttu-id="d457b-254">Bir tablodaki her kayıt tarihi için Öğrenci sayısı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="d457b-254">The count of students for each enrollment date displays in a table.</span></span>

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a><span data-ttu-id="d457b-256">Kodu alma</span><span class="sxs-lookup"><span data-stu-id="d457b-256">Get the code</span></span>

[<span data-ttu-id="d457b-257">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="d457b-257">Download the Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a><span data-ttu-id="d457b-258">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d457b-258">Additional resources</span></span>

<span data-ttu-id="d457b-259">Entity Framework diğer kaynakların bağlantılarını bulunabilir [ASP.NET veri erişimi - önerilen kaynaklar](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="d457b-259">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d457b-260">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="d457b-260">Next steps</span></span>

<span data-ttu-id="d457b-261">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="d457b-261">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d457b-262">Sütun sıralama bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="d457b-262">Add column sort links</span></span>
> * <span data-ttu-id="d457b-263">Bir arama kutusu ekleme</span><span class="sxs-lookup"><span data-stu-id="d457b-263">Add a Search box</span></span>
> * <span data-ttu-id="d457b-264">Sayfalama ekleme</span><span class="sxs-lookup"><span data-stu-id="d457b-264">Add paging</span></span>
> * <span data-ttu-id="d457b-265">Hakkında sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="d457b-265">Create an About page</span></span>

<span data-ttu-id="d457b-266">Bağlantı dayanıklılığı ve komut durdurma kullanma hakkında bilgi edinmek için sonraki makaleye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="d457b-266">Advance to the next article to learn how to use connection resiliency and command interception.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="d457b-267">Bağlantı dayanıklılığı ve komut durdurma</span><span class="sxs-lookup"><span data-stu-id="d457b-267">Connection resiliency and command interception</span></span>](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)