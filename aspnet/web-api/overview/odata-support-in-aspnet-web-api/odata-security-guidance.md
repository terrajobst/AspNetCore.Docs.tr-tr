---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Güvenlik Kılavuzu ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/06/2013
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 4ba53e15dab83368097a58ba4d0d2e46d113d1d2
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325724"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="0240b-102">Güvenlik Kılavuzu ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="0240b-102">Security Guidance for ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="0240b-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0240b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="0240b-104">Bu konuda bazı OData aracılığıyla bir veri kümesi gösterme, dikkate almanız gereken güvenlik konuları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0240b-104">This topic describes some of the security issues that you should consider when exposing a dataset through OData.</span></span>

## <a name="edm-security"></a><span data-ttu-id="0240b-105">EDM güvenlik</span><span class="sxs-lookup"><span data-stu-id="0240b-105">EDM Security</span></span>

<span data-ttu-id="0240b-106">Sorgu semantiği, varlık veri Modeli'ni temel (EDM), temel alınan model türleri değil temel alır.</span><span class="sxs-lookup"><span data-stu-id="0240b-106">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="0240b-107">EDM bir özellik hariç tutabilir ve sorgu için görünür olmaz.</span><span class="sxs-lookup"><span data-stu-id="0240b-107">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="0240b-108">Örneğin, bir çalışan türü maaş özelliğine sahip modelinizi içerdiğini varsayın.</span><span class="sxs-lookup"><span data-stu-id="0240b-108">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="0240b-109">Bu özellik istemcilerden gizlemek için EDM dışlanacak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0240b-109">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="0240b-110">Bir EDM özelliği için exlude iki yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="0240b-110">There are two ways to exlude a property from the EDM.</span></span> <span data-ttu-id="0240b-111">Ayarlayabileceğiniz **[IgnoreDataMember]** model sınıfı özelliğinde özniteliği:</span><span class="sxs-lookup"><span data-stu-id="0240b-111">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="0240b-112">Ayrıca özelliği EDM programlı bir şekilde kaldırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0240b-112">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="0240b-113">Sorgu güvenliği</span><span class="sxs-lookup"><span data-stu-id="0240b-113">Query Security</span></span>

<span data-ttu-id="0240b-114">Kötü amaçlı veya naïve istemci yürütmek için çok uzun zaman alan bir sorgu oluşturmak mümkün olabilir.</span><span class="sxs-lookup"><span data-stu-id="0240b-114">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="0240b-115">En kötü durumda bu, hizmete erişimi kesintiye uğratabilir.</span><span class="sxs-lookup"><span data-stu-id="0240b-115">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="0240b-116">**[Queryable]** özniteliktir ayrıştırır, doğrular ve sorgu geçerli bir eylem filtresi.</span><span class="sxs-lookup"><span data-stu-id="0240b-116">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="0240b-117">Filtre sorgu seçeneklerini bir LINQ ifadesini dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="0240b-117">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="0240b-118">OData denetleyicisi döndürdüğünde bir **Iqueryable** türü **Iqueryable** LINQ sağlayıcısı bir sorguda LINQ ifadesi dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="0240b-118">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="0240b-119">Bu nedenle, performansı, kullanılan LINQ sağlayıcısı ve aynı zamanda veri kümesi veya veritabanı şemanızı belirli özelliklerine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="0240b-119">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="0240b-120">ASP.NET Web API OData sorgu seçenekleri kullanma hakkında daha fazla bilgi için bkz. [OData sorgu seçeneklerini destekleme](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="0240b-120">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="0240b-121">Tüm istemciler (örneğin, bir kuruluş ortamında) güvenilir biliyorsanız ya da veri kümeniz küçükse, sorgu performansı sorunu olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="0240b-121">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="0240b-122">Aksi takdirde, aşağıdaki önerileri göz önünde bulundurmalısınız.</span><span class="sxs-lookup"><span data-stu-id="0240b-122">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="0240b-123">Çeşitli sorgularla hizmetinizi test etme ve DB profil.</span><span class="sxs-lookup"><span data-stu-id="0240b-123">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="0240b-124">Büyük bir veri kümesi döndüren bir sorgu önlemek sunucu tabanlı disk belleği, etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="0240b-124">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="0240b-125">Daha fazla bilgi için [Server-Driven sayfalama](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="0240b-125">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="0240b-126">$Filter ve $orderby gerekiyor mu?</span><span class="sxs-lookup"><span data-stu-id="0240b-126">Do you need $filter and $orderby?</span></span> <span data-ttu-id="0240b-127">Bazı uygulamalar disk belleği, $top ve $skip kullanarak istemci izin ver, ancak başka sorgu seçenekleriyle devre dışı.</span><span class="sxs-lookup"><span data-stu-id="0240b-127">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="0240b-128">Kümelenmiş bir dizin özelliklerinde $orderby kısıtlama göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="0240b-128">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="0240b-129">Kümelenmiş bir dizin olmadan büyük veri sıralama yavaştır.</span><span class="sxs-lookup"><span data-stu-id="0240b-129">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="0240b-130">En yüksek düğüm sayısı: **MaxNodeCount** özelliği **[Queryable]** izin $filter sözdizimi ağacı maksimum düğüm sayısına ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0240b-130">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="0240b-131">Varsayılan değer 100'dür, ancak çok sayıda düğümü derlemek yavaş olabilir çünkü daha düşük bir değere ayarlamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0240b-131">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="0240b-132">LINQ to Objects'in (yani, bir koleksiyonda Ara bir LINQ sağlayıcısı kullanmadan bellekte LINQ sorguları) kullanıyorsanız, bu özellikle doğrudur.</span><span class="sxs-lookup"><span data-stu-id="0240b-132">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="0240b-133">Bunlar yavaş olması gibi any() ve all() işlevler devre dışı bırakmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="0240b-133">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="0240b-134">Herhangi bir dize özelliği büyük dizelerin içeriyorsa&#8212;ürün açıklaması veya blog girişi&#8212;dize işlevleri devre dışı bırakmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="0240b-134">If any string properties contain large strings&#8212;for example, a product description or a blog entry&#8212;consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="0240b-135">Gezinti özellikleri filtreleme engelleyerek göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="0240b-135">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="0240b-136">Gezinti özellikleri filtreleme bağlı olarak, veritabanı şemasını yavaş olabilir bir birleştirme neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="0240b-136">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="0240b-137">Aşağıdaki kod, gezinti özellikleri filtreleme engelleyen bir sorgu Doğrulayıcı gösterir.</span><span class="sxs-lookup"><span data-stu-id="0240b-137">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="0240b-138">Sorgu doğrulayıcıları hakkında daha fazla bilgi için bkz: [sorgu doğrulama](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="0240b-138">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="0240b-139">$Filter sorgu veritabanınız için özelleştirilmiş bir doğrulayıcı yazarak kısıtlamayı göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="0240b-139">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="0240b-140">Örneğin, bu iki sorguları göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="0240b-140">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="0240b-141">Aktörler son adı 'A' ile başlayan tüm filmlerle.</span><span class="sxs-lookup"><span data-stu-id="0240b-141">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="0240b-142">Tüm filmlere 1994'te yayımlanan.</span><span class="sxs-lookup"><span data-stu-id="0240b-142">All movies released in 1994.</span></span>

    <span data-ttu-id="0240b-143">İlk sorgu, filmler aktörler tarafından dizinlenen sürece filmler listesinin tümünü taramak üzere veritabanı altyapısı gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="0240b-143">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="0240b-144">İkinci sorgu kabul edilebilir ise varsayılıyor filmler yayın yıla göre dizine eklenir.</span><span class="sxs-lookup"><span data-stu-id="0240b-144">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="0240b-145">Aşağıdaki kod, "ReleaseYear" ve "Title" özellikleri ancak başka hiçbir özellik filtrelemeye olanak tanır, Doğrulayıcıyı gösterir.</span><span class="sxs-lookup"><span data-stu-id="0240b-145">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="0240b-146">Genel olarak, gereksinim duyduğunuz $filter işlev göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="0240b-146">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="0240b-147">$Filter, tam anlamlılık istemcilerinize ihtiyacınız yoksa, izin verilen işlevler sınırlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0240b-147">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
