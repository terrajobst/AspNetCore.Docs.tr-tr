---
title: "Dağıtılmış önbellek etiket Yardımcısı | Microsoft Docs"
author: pkellner
description: "Önbellek etiket Yardımcısı ile çalışmaya nasıl gösterir"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: f5844dade218fdba1169a55fe3ce251a9cc03db2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="d4767-103">Dağıtılmış önbellek etiket Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="d4767-103">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="d4767-104">Tarafından [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="d4767-104">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="d4767-105">Dağıtılmış önbellek etiket Yardımcısı dağıtılmış önbellek kaynağı için içeriği önbelleğe alarak ASP.NET Core uygulamanızın performansını önemli ölçüde artırmak için yeteneği sağlar.</span><span class="sxs-lookup"><span data-stu-id="d4767-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="d4767-106">Dağıtılmış önbellek etiket Yardımcısı önbellek etiket Yardımcısı ile aynı temel sınıfından devralır.</span><span class="sxs-lookup"><span data-stu-id="d4767-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="d4767-107">Önbellek etiketi Yardımcıyla ilişkili tüm öznitelikleri üzerinde dağıtılmış etiket Yardımcısı da çalışır.</span><span class="sxs-lookup"><span data-stu-id="d4767-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="d4767-108">Dağıtılmış önbellek etiket Yardımcısı izleyen **açık bağımlılıkları ilkesine** olarak bilinen **Oluşturucu ekleme**.</span><span class="sxs-lookup"><span data-stu-id="d4767-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="d4767-109">Özellikle, `IDistributedCache` arabirimi kapsayıcı dağıtılmış önbellek etiketi yardımcının oluşturucuya geçirilir.</span><span class="sxs-lookup"><span data-stu-id="d4767-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="d4767-110">Belirli hiçbir somut uygulaması varsa `IDistributedCache` içinde oluşturulan `ConfigureServices`, genellikle haline içinde bulunan ve dağıtılmış önbellek etiket Yardımcısı aynı bellek içi sağlayıcısı temel önbellek etiket Yardımcısı olarak önbelleğe alınmış verileri depolamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="d4767-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="d4767-111">Dağıtılmış önbellek etiketi yardımcı öznitelik</span><span class="sxs-lookup"><span data-stu-id="d4767-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="d4767-112">süresi dolmadan üzerinde süresi dolduktan sonra süresi dolar-kayan etkin farklılık tarafından-üstbilgi farklılık-by-sorgusu farklılık tarafından-yönlendirme farklılık-tarafından-cookie farklılık kullanıcıya göre farklılık-önceliğe göre</span><span class="sxs-lookup"><span data-stu-id="d4767-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="d4767-113">Önbellek etiket Yardımcısı tanımları için bkz.</span><span class="sxs-lookup"><span data-stu-id="d4767-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="d4767-114">Bu öznitelikler önbellek etiket Yardımcısı yaygın şekilde dağıtılmış önbellek etiket Yardımcısı önbellek etiket Yardımcısı ile aynı sınıfta devralır.</span><span class="sxs-lookup"><span data-stu-id="d4767-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="d4767-115">adı (gerekli)</span><span class="sxs-lookup"><span data-stu-id="d4767-115">name (required)</span></span>

| <span data-ttu-id="d4767-116">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="d4767-116">Attribute Type</span></span>    | <span data-ttu-id="d4767-117">Örnek değer</span><span class="sxs-lookup"><span data-stu-id="d4767-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="d4767-118">dize</span><span class="sxs-lookup"><span data-stu-id="d4767-118">string</span></span>    | <span data-ttu-id="d4767-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="d4767-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="d4767-120">Gerekli `name` özniteliği dağıtılmış önbellek etiket Yardımcısı her örneği için depolanan bu önbelleğine anahtarı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d4767-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="d4767-121">Temel önbellek etiketi Razor sayfa adı ve razor sayfasını etiketi yardımcı konumunu temel alarak her önbellek etiket Yardımcısı örneği için bir anahtar atayan yardımcıyı farklı olarak, dağıtılmış önbellek etiket Yardımcısı yalnızca, anahtar özniteliği taban`name`</span><span class="sxs-lookup"><span data-stu-id="d4767-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="d4767-122">Kullanım örneği:</span><span class="sxs-lookup"><span data-stu-id="d4767-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="d4767-123">Dağıtılmış önbellek etiket Yardımcısı IDistributedCache uygulamaları</span><span class="sxs-lookup"><span data-stu-id="d4767-123">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="d4767-124">İki uygulamaları vardır `IDistributedCache` ASP.NET Core yerleşik olarak bulunur.</span><span class="sxs-lookup"><span data-stu-id="d4767-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="d4767-125">Bir temel **Sql Server** ve diğer dayanır **Redis**.</span><span class="sxs-lookup"><span data-stu-id="d4767-125">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="d4767-126">Bu uygulamaların ayrıntılarını adlandırılmış "ile çalışmayı dağıtılmış önbellek" aşağıda başvurulan kaynakta bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="d4767-126">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="d4767-127">Bir örneği iki uygulamaları içeren `IDistributedCache` ASP.NET Core'nın içinde **haline**.</span><span class="sxs-lookup"><span data-stu-id="d4767-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="d4767-128">Özellikle herhangi belirli uyarlamasını kullanımıyla ilişkili hiçbir etiket öznitelik `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="d4767-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="d4767-129">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d4767-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
