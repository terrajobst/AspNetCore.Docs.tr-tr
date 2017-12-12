---
title: "Dağıtılmış önbellek etiket Yardımcısı | Microsoft Docs"
author: pkellner
description: "Önbellek etiket Yardımcısı ile çalışmaya nasıl gösterir"
keywords: "ASP.NET Core, etiket Yardımcısı"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a022
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 462c3775677924fc7b9b715cd6de75fe53ada89e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="bab1f-104">Dağıtılmış önbellek etiket Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="bab1f-104">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="bab1f-105">Tarafından [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="bab1f-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="bab1f-106">Dağıtılmış önbellek etiket Yardımcısı dağıtılmış önbellek kaynağı için içeriği önbelleğe alarak ASP.NET Core uygulamanızın performansını önemli ölçüde artırmak için yeteneği sağlar.</span><span class="sxs-lookup"><span data-stu-id="bab1f-106">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="bab1f-107">Dağıtılmış önbellek etiket Yardımcısı önbellek etiket Yardımcısı ile aynı temel sınıfından devralır.</span><span class="sxs-lookup"><span data-stu-id="bab1f-107">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="bab1f-108">Önbellek etiketi Yardımcıyla ilişkili tüm öznitelikleri üzerinde dağıtılmış etiket Yardımcısı da çalışır.</span><span class="sxs-lookup"><span data-stu-id="bab1f-108">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="bab1f-109">Dağıtılmış önbellek etiket Yardımcısı izleyen **açık bağımlılıkları ilkesine** olarak bilinen **Oluşturucu ekleme**.</span><span class="sxs-lookup"><span data-stu-id="bab1f-109">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="bab1f-110">Özellikle, `IDistributedCache` arabirimi kapsayıcı dağıtılmış önbellek etiketi yardımcının oluşturucuya geçirilir.</span><span class="sxs-lookup"><span data-stu-id="bab1f-110">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="bab1f-111">Belirli hiçbir somut uygulaması varsa `IDistributedCache` içinde oluşturulan `ConfigureServices`, genellikle haline içinde bulunan ve dağıtılmış önbellek etiket Yardımcısı aynı bellek içi sağlayıcısı temel önbellek etiket Yardımcısı olarak önbelleğe alınmış verileri depolamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="bab1f-111">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="bab1f-112">Dağıtılmış önbellek etiketi yardımcı öznitelik</span><span class="sxs-lookup"><span data-stu-id="bab1f-112">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="bab1f-113">süresi dolmadan üzerinde süresi dolduktan sonra süresi dolar-kayan etkin farklılık tarafından-üstbilgi farklılık-by-sorgusu farklılık tarafından-yönlendirme farklılık-tarafından-cookie farklılık kullanıcıya göre farklılık-önceliğe göre</span><span class="sxs-lookup"><span data-stu-id="bab1f-113">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="bab1f-114">Önbellek etiket Yardımcısı tanımları için bkz.</span><span class="sxs-lookup"><span data-stu-id="bab1f-114">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="bab1f-115">Bu öznitelikler önbellek etiket Yardımcısı yaygın şekilde dağıtılmış önbellek etiket Yardımcısı önbellek etiket Yardımcısı ile aynı sınıfta devralır.</span><span class="sxs-lookup"><span data-stu-id="bab1f-115">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="bab1f-116">adı (gerekli)</span><span class="sxs-lookup"><span data-stu-id="bab1f-116">name (required)</span></span>

| <span data-ttu-id="bab1f-117">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="bab1f-117">Attribute Type</span></span>    | <span data-ttu-id="bab1f-118">Örnek değer</span><span class="sxs-lookup"><span data-stu-id="bab1f-118">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="bab1f-119">dize</span><span class="sxs-lookup"><span data-stu-id="bab1f-119">string</span></span>    | <span data-ttu-id="bab1f-120">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="bab1f-120">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="bab1f-121">Gerekli `name` özniteliği dağıtılmış önbellek etiket Yardımcısı her örneği için depolanan bu önbelleğine anahtarı olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bab1f-121">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="bab1f-122">Temel önbellek etiketi Razor sayfa adı ve razor sayfasını etiketi yardımcı konumunu temel alarak her önbellek etiket Yardımcısı örneği için bir anahtar atayan yardımcıyı farklı olarak, dağıtılmış önbellek etiket Yardımcısı yalnızca, anahtar özniteliği taban`name`</span><span class="sxs-lookup"><span data-stu-id="bab1f-122">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="bab1f-123">Kullanım örneği:</span><span class="sxs-lookup"><span data-stu-id="bab1f-123">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="bab1f-124">Dağıtılmış önbellek etiket Yardımcısı IDistributedCache uygulamaları</span><span class="sxs-lookup"><span data-stu-id="bab1f-124">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="bab1f-125">İki uygulamaları vardır `IDistributedCache` ASP.NET Core yerleşik olarak bulunur.</span><span class="sxs-lookup"><span data-stu-id="bab1f-125">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="bab1f-126">Bir temel **Sql Server** ve diğer dayanır **Redis**.</span><span class="sxs-lookup"><span data-stu-id="bab1f-126">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="bab1f-127">Bu uygulamaların ayrıntılarını adlandırılmış "ile çalışmayı dağıtılmış önbellek" aşağıda başvurulan kaynakta bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="bab1f-127">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="bab1f-128">Bir örneği iki uygulamaları içeren `IDistributedCache` ASP.NET Core'nın içinde **haline**.</span><span class="sxs-lookup"><span data-stu-id="bab1f-128">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="bab1f-129">Özellikle herhangi belirli uyarlamasını kullanımıyla ilişkili hiçbir etiket öznitelik `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="bab1f-129">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="bab1f-130">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bab1f-130">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
