---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: ASP.NET Web API 2 OData sorgu seçeneklerini destekleme | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 3ce2b38a13e8684a88bb0ce6183671fae98795c7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380849"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="f3ad8-102">ASP.NET Web API 2 OData sorgu seçeneklerini destekleme</span><span class="sxs-lookup"><span data-stu-id="f3ad8-102">Supporting OData Query Options in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="f3ad8-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f3ad8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f3ad8-104">OData OData sorgu değiştirmek için kullanılan parametreleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-104">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="f3ad8-105">İstemci, bu parametreleri istek URI sorgu dizesinde gönderir.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-105">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="f3ad8-106">Örneğin, sonuçları sıralamak için bir istemci $orderby parametresini kullanır:</span><span class="sxs-lookup"><span data-stu-id="f3ad8-106">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="f3ad8-107">Bu parametreleri OData belirtiminden çağırır *sorgu seçenekleri*.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-107">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="f3ad8-108">Projenizdeki herhangi bir Web API denetleyicisi için OData sorgu seçenekleri etkinleştirebilirsiniz &#8212; denetleyici bir OData uç nokta olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-108">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="f3ad8-109">Bu, filtreleme ve sıralama için herhangi bir Web API'sini uygulama gibi özellikleri eklemek için kullanışlı bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-109">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="f3ad8-110">Lütfen bu makaleyi okuyun, sorgu seçeneklerini etkinleştirmeden önce [OData güvenlik rehberi](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="f3ad8-110">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="f3ad8-111">OData sorgu seçenekleri etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f3ad8-111">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="f3ad8-112">Örnek sorgular</span><span class="sxs-lookup"><span data-stu-id="f3ad8-112">Example Queries</span></span>](#examples)
- [<span data-ttu-id="f3ad8-113">Sunucu tabanlı disk belleği</span><span class="sxs-lookup"><span data-stu-id="f3ad8-113">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="f3ad8-114">Sorgu seçenekleri sınırlama</span><span class="sxs-lookup"><span data-stu-id="f3ad8-114">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="f3ad8-115">Sorgu seçenekleri doğrudan çağırma</span><span class="sxs-lookup"><span data-stu-id="f3ad8-115">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="f3ad8-116">Sorgu doğrulama</span><span class="sxs-lookup"><span data-stu-id="f3ad8-116">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="f3ad8-117">OData sorgu seçenekleri etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f3ad8-117">Enabling OData Query Options</span></span>

<span data-ttu-id="f3ad8-118">Web API'si, aşağıdaki OData sorgu seçeneklerini destekler:</span><span class="sxs-lookup"><span data-stu-id="f3ad8-118">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="f3ad8-119">Seçenek</span><span class="sxs-lookup"><span data-stu-id="f3ad8-119">Option</span></span> | <span data-ttu-id="f3ad8-120">Açıklama</span><span class="sxs-lookup"><span data-stu-id="f3ad8-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f3ad8-121">$expand</span><span class="sxs-lookup"><span data-stu-id="f3ad8-121">$expand</span></span> | <span data-ttu-id="f3ad8-122">İlgili varlıklar satır içi genişletir.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-122">Expands related entities inline.</span></span> |
| <span data-ttu-id="f3ad8-123">$filter</span><span class="sxs-lookup"><span data-stu-id="f3ad8-123">$filter</span></span> | <span data-ttu-id="f3ad8-124">Sonuçları bir Boolean koşulu temel alarak filtreler.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-124">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="f3ad8-125">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="f3ad8-125">$inlinecount</span></span> | <span data-ttu-id="f3ad8-126">Sunucu yanıt olarak eşleşen varlıkların toplam sayısı dahil etmek anlatır.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-126">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="f3ad8-127">(Sunucu tarafı disk belleği için kullanışlıdır.)</span><span class="sxs-lookup"><span data-stu-id="f3ad8-127">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="f3ad8-128">$orderby</span><span class="sxs-lookup"><span data-stu-id="f3ad8-128">$orderby</span></span> | <span data-ttu-id="f3ad8-129">Sonuçları sıralar.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-129">Sorts the results.</span></span> |
| <span data-ttu-id="f3ad8-130">$select</span><span class="sxs-lookup"><span data-stu-id="f3ad8-130">$select</span></span> | <span data-ttu-id="f3ad8-131">Yanıta dahil etmek için hangi özelliklerin seçer.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-131">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="f3ad8-132">$skip</span><span class="sxs-lookup"><span data-stu-id="f3ad8-132">$skip</span></span> | <span data-ttu-id="f3ad8-133">İlk n sonuç atlar.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-133">Skips the first n results.</span></span> |
| <span data-ttu-id="f3ad8-134">$top</span><span class="sxs-lookup"><span data-stu-id="f3ad8-134">$top</span></span> | <span data-ttu-id="f3ad8-135">Yalnızca ilk n sonuç döndürür.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-135">Returns only the first n the results.</span></span> |

<span data-ttu-id="f3ad8-136">OData sorgu seçeneklerini kullanmak için bunları açıkça etkinleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-136">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="f3ad8-137">Bunları etkinleştirmek için uygulamanın tamamını veya belirli denetleyicileri veya belirli eylemler için etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-137">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="f3ad8-138">OData sorgu seçeneklerini etkinleştirmek istiyorsanız arama **EnableQuerySupport** üzerinde **HttpConfiguration** başlangıçta sınıfı:</span><span class="sxs-lookup"><span data-stu-id="f3ad8-138">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="f3ad8-139">**EnableQuerySupport** yöntem sorgu seçenekleri için genel olarak döndüren bir denetleyici eylemi sağlar bir **Iqueryable** türü.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-139">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="f3ad8-140">Uygulamanın tamamı için etkin sorgu seçenekleri istemiyorsanız, bunları belirli bir denetleyici eylemleri için ekleyerek etkinleştirebilirsiniz **[Queryable]** özniteliği eylem yöntemine.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-140">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="f3ad8-141">Örnek sorgular</span><span class="sxs-lookup"><span data-stu-id="f3ad8-141">Example Queries</span></span>

<span data-ttu-id="f3ad8-142">Bu bölümde, OData sorgu seçeneklerini kullanarak olası sorgu türleri gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-142">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="f3ad8-143">Sorgu seçenekleri hakkındaki özel ayrıntılar için OData belgelerine bakın [www.odata.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="f3ad8-143">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="f3ad8-144">$ Hakkında bilgi için genişletin ve bkz $select, [$select kullanarak $expand ve ASP.NET Web API OData $value](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="f3ad8-144">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="f3ad8-145">**İstemci tabanlı disk belleği**</span><span class="sxs-lookup"><span data-stu-id="f3ad8-145">**Client-Driven Paging**</span></span>

<span data-ttu-id="f3ad8-146">Büyük varlık kümeleri için istemci sonuçları sayısını sınırlamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-146">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="f3ad8-147">Örneğin, bir istemci, sonraki sonuç sayfasını almak için "İleri" bağlantıları ile bir kerede 10 girişleri gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-147">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="f3ad8-148">Bunu yapmak için istemci $top ve $skip seçeneklerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-148">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="f3ad8-149">En fazla girdi sayısı bu değeri döndürmek için $top seçeneği sunar ve atlanacak girdi sayısı $skip seçeneği sunar.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-149">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="f3ad8-150">Önceki örnekte girişleri 21 ile 30 getirir.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-150">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="f3ad8-151">**Filtreleme**</span><span class="sxs-lookup"><span data-stu-id="f3ad8-151">**Filtering**</span></span>

<span data-ttu-id="f3ad8-152">$Filter seçeneği bir Boole ifadesi uygulayarak, sonuçları filtrelemek için bir istemci olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-152">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="f3ad8-153">Filtre ifadeleri oldukça güçlü; Bunlar, mantıksal ve aritmetik işleçler, dize işlevleri ve tarih işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-153">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="f3ad8-154">Kategorisindeki tüm ürünler "Toys" eşit döndürür.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-154">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="f3ad8-155">`http://localhost/Products?$filter=Category` EQ 'Toys'</span><span class="sxs-lookup"><span data-stu-id="f3ad8-155">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="f3ad8-156">Fiyat 10'dan küçük olan tüm ürünleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-156">Return all products with price less than 10.</span></span> | <span data-ttu-id="f3ad8-157">`http://localhost/Products?$filter=Price` lt 10</span><span class="sxs-lookup"><span data-stu-id="f3ad8-157">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="f3ad8-158">Mantıksal işleçler: tüm ürünleri döndürmek burada fiyat > 5 ve fiyat = < = 15.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-158">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="f3ad8-159">`http://localhost/Products?$filter=Price` Ge 5 ve fiyat le 15</span><span class="sxs-lookup"><span data-stu-id="f3ad8-159">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="f3ad8-160">Dize işlevleri: "zz" olan tüm ürünleri adını döndürür.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-160">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="f3ad8-161">Tarih işlevleri: 2005'ten sonra tüm ürünleriyle ReleaseDate döndürür.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-161">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="f3ad8-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span><span class="sxs-lookup"><span data-stu-id="f3ad8-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="f3ad8-163">**Sıralama**</span><span class="sxs-lookup"><span data-stu-id="f3ad8-163">**Sorting**</span></span>

<span data-ttu-id="f3ad8-164">Sonuçları sıralamak için $orderby filtreyi kullanın.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-164">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="f3ad8-165">Fiyat göre sıralayın.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-165">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="f3ad8-166">Fiyat (yüksekten en düşüğe) azalan düzende sırala.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-166">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="f3ad8-167">Kategoriye göre sıralayın ve ardından fiyat kategorilerde azalan düzende sırala.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-167">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="f3ad8-168">Sunucu tabanlı disk belleği</span><span class="sxs-lookup"><span data-stu-id="f3ad8-168">Server-Driven Paging</span></span>

<span data-ttu-id="f3ad8-169">Veritabanınızı milyonlarca kayıt varsa, bunları göndermek istemiyorsanız bir yük içinde tüm.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-169">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="f3ad8-170">Bunu önlemek için sunucu, tek bir yanıtta gönderen giriş sayısını sınırlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-170">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="f3ad8-171">Sunucu disk belleği'ni etkinleştirmek için **PageSize** özelliğinde **Queryable** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-171">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="f3ad8-172">En fazla girdi sayısı bu değeri döndürülecek değerdir.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-172">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="f3ad8-173">Denetleyicinizi OData biçimi döndürürse, yanıt gövdesi veri sonraki sayfaya bir bağlantı içerir:</span><span class="sxs-lookup"><span data-stu-id="f3ad8-173">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="f3ad8-174">İstemci, bir sonraki sayfayı almak için bu bağlantıyı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-174">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="f3ad8-175">İstemci sonuç kümesinde girişleri toplam sayısını öğrenmek $inlinecount sorgu seçeneği değerine sahip "inlinecount" ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-175">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="f3ad8-176">Değer "inlinecount" toplam sayı bu sayıyı yanıta eklenecek sunucunun bildirir:</span><span class="sxs-lookup"><span data-stu-id="f3ad8-176">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="f3ad8-177">Sonraki sayfa bağlantıları hem de satır içi sayı OData biçimini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-177">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="f3ad8-178">OData sayısı ve bağlantı tutacak yanıt gövdesi içinde özel alanlar tanımlar nedenidir.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-178">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>


<span data-ttu-id="f3ad8-179">OData olmayan biçimleri için sonraki sayfa ve satır içi bağlantıları sayısı, sorgu sonuçlarında sarmalama tarafından desteklemek için tanımlanabilir bir **PageResult&lt;T&gt;**  nesne.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-179">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="f3ad8-180">Ancak, biraz daha fazla kod gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-180">However, it requires a bit more code.</span></span> <span data-ttu-id="f3ad8-181">Aşağıda bir örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="f3ad8-181">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="f3ad8-182">Bir örnek JSON yanıtı aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="f3ad8-182">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="f3ad8-183">Sorgu seçenekleri sınırlama</span><span class="sxs-lookup"><span data-stu-id="f3ad8-183">Limiting the Query Options</span></span>

<span data-ttu-id="f3ad8-184">Sorgu seçenekleri istemci çok fazla sunucuda çalıştırılan sorguyu üzerinde denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-184">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="f3ad8-185">Bazı durumlarda, güvenlik veya performans nedenleriyle kullanılabilir seçenekler sınırlamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-185">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="f3ad8-186">**[Queryable]** özniteliği bazı yerleşik özelliklerinde bu.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-186">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="f3ad8-187">Bazı örnekleri aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-187">Here are some examples.</span></span>

<span data-ttu-id="f3ad8-188">Yalnızca $skip ve $top, sayfalama ve başka hiçbir şey desteklemek için izin ver:</span><span class="sxs-lookup"><span data-stu-id="f3ad8-188">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="f3ad8-189">Veritabanında oluşturulmuyor özellikleri sıralama önlemek belirli özellikleri yalnızca tarafından sıralanmasına izin vermek:</span><span class="sxs-lookup"><span data-stu-id="f3ad8-189">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="f3ad8-190">"Eq" mantıksal işlevi ancak diğer mantıksal işlevler izin ver:</span><span class="sxs-lookup"><span data-stu-id="f3ad8-190">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="f3ad8-191">Tüm aritmetik işleçlere izin verme:</span><span class="sxs-lookup"><span data-stu-id="f3ad8-191">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="f3ad8-192">Oluşturarak, genel bir seçenekler kısıtlayabilirsiniz bir **Queryableattribute'taki** örneği ve aktarması **EnableQuerySupport** işlevi:</span><span class="sxs-lookup"><span data-stu-id="f3ad8-192">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="f3ad8-193">Sorgu seçenekleri doğrudan çağırma</span><span class="sxs-lookup"><span data-stu-id="f3ad8-193">Invoking Query Options Directly</span></span>

<span data-ttu-id="f3ad8-194">Yerine **[Queryable]** özniteliği doğrudan denetleyicinizin sorgu seçeneklerini çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-194">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="f3ad8-195">Bunu yapmak için bir **ODataQueryOptions** denetleyici yöntemin parametre.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-195">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="f3ad8-196">Bu durumda, ihtiyacınız olmayan **[Queryable]** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-196">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="f3ad8-197">Web API doldurur **ODataQueryOptions** URI'SİNDEN sorgu dizesi.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-197">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="f3ad8-198">Sorgu uygulamak için başarılı bir **Iqueryable** için **ApplyTo** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-198">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="f3ad8-199">Başka bir yöntem döndürür **Iqueryable**.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-199">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="f3ad8-200">Yoksa, Gelişmiş senaryolar için bir **Iqueryable** sorgu sağlayıcısı inceleyebilirsiniz **ODataQueryOptions** ve başka bir forma sorgu seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-200">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="f3ad8-201">(Örneğin, RaghuRam Nadiminti'nın blog yayınına bakın [çevirme OData sorgularını hql'e](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), da içeren bir [örnek](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="f3ad8-201">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="f3ad8-202">Sorgu doğrulama</span><span class="sxs-lookup"><span data-stu-id="f3ad8-202">Query Validation</span></span>

<span data-ttu-id="f3ad8-203">**[Queryable]** özniteliği sorgu yürütülmeden önce doğrular.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-203">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="f3ad8-204">Doğrulama adımını içinde gerçekleştirilen **QueryableAttribute.ValidateQuery** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-204">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="f3ad8-205">Doğrulama işlemi ayrıca özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-205">You can also customize the validation process.</span></span>

<span data-ttu-id="f3ad8-206">Ayrıca bkz: [OData güvenlik rehberi](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="f3ad8-206">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="f3ad8-207">Geçersiz kılma sınıfları almak için diğer bir deyişle, Doğrulayıcı birini ilk olarak tanımlanan **Web.Http.OData.Query.Validators** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-207">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="f3ad8-208">Örneğin, aşağıdaki Doğrulayıcı sınıfını $orderby seçeneği için 'desc' seçeneğini devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-208">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="f3ad8-209">Alt **[Queryable]** geçersiz kılmak için öznitelik **ValidateQuery** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f3ad8-209">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="f3ad8-210">Ardından, özel özniteliği ayarlayın ya da genel olarak veya denetleyici başına:</span><span class="sxs-lookup"><span data-stu-id="f3ad8-210">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="f3ad8-211">Kullanıyorsanız **ODataQueryOptions** doğrudan Doğrulayıcı seçenekleri ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="f3ad8-211">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
