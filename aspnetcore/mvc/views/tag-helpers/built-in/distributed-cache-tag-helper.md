---
title: Dağıtılmış önbellek etiketi Yardımcısı ASP.NET core'da
author: pkellner
description: Dağıtılmış önbellek etiketi Yardımcısı'nı kullanmayı öğrenin.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 1b51164a6d3dab2eeaf64262d6f0d9961bd00d12
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028104"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="34cde-103">Dağıtılmış önbellek etiketi Yardımcısı ASP.NET core'da</span><span class="sxs-lookup"><span data-stu-id="34cde-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="34cde-104">Tarafından [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="34cde-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="34cde-105">Dağıtılmış önbellek etiketi Yardımcısı, dağıtılmış önbellek kaynağına içeriği önbelleğe alarak ASP.NET Core uygulamanızı performansını önemli ölçüde artırmak olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="34cde-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="34cde-106">Dağıtılmış önbellek etiketi Yardımcısı, önbellek etiketi Yardımcısı olarak aynı temel sınıfından devralır.</span><span class="sxs-lookup"><span data-stu-id="34cde-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="34cde-107">Önbellek etiketi Yardımcısı ile ilişkili tüm öznitelikler, üzerinde dağıtılmış etiketi Yardımcısı olarak da çalışır.</span><span class="sxs-lookup"><span data-stu-id="34cde-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>

<span data-ttu-id="34cde-108">Dağıtılmış önbellek etiketi Yardımcısı izleyen **açık bağımlılıkları İlkesi** olarak bilinen **Oluşturucu ekleme**.</span><span class="sxs-lookup"><span data-stu-id="34cde-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span> <span data-ttu-id="34cde-109">Özellikle, `IDistributedCache` arabirimi kapsayıcı, dağıtılmış önbellek etiketi Yardımcısı'nın oluşturucuya geçirilir.</span><span class="sxs-lookup"><span data-stu-id="34cde-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="34cde-110">Belirli bir somut uygulamasına varsa `IDistributedCache` içinde oluşturulan `ConfigureServices`, genellikle startup.cs içinde bulunan ve ardından dağıtılmış önbellek etiketi Yardımcısı temel önbellek etiketi Yardımcısı önbelleğe alınmış verileri depolamak için aynı bellek içi sağlayıcısı kullanır.</span><span class="sxs-lookup"><span data-stu-id="34cde-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="34cde-111">Dağıtılmış önbellek etiketi Yardımcısı öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="34cde-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="34cde-112">süresi dolmadan açma süresi dolduktan sonra süresi dolar-kayan etkin farklılık-tarafından-header farklılık-tarafından-sorgu değişir-tarafından-route farklılık-tarafından-tanımlama bilgisi değişiklik kullanıcı tarafından farklılık-önceliğe göre</span><span class="sxs-lookup"><span data-stu-id="34cde-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="34cde-113">Önbellek etiketi Yardımcısı tanımları için bkz.</span><span class="sxs-lookup"><span data-stu-id="34cde-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="34cde-114">Bu öznitelikler önbellek etiketi Yardımcısı ' yaygın şekilde dağıtılmış önbellek etiketi Yardımcısı önbellek etiketi Yardımcısı olarak aynı sınıfından devralır.</span><span class="sxs-lookup"><span data-stu-id="34cde-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="34cde-115">ad (gerekli)</span><span class="sxs-lookup"><span data-stu-id="34cde-115">name (required)</span></span>

| <span data-ttu-id="34cde-116">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="34cde-116">Attribute Type</span></span>    | <span data-ttu-id="34cde-117">Örnek değer</span><span class="sxs-lookup"><span data-stu-id="34cde-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="34cde-118">dize</span><span class="sxs-lookup"><span data-stu-id="34cde-118">string</span></span>    | <span data-ttu-id="34cde-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="34cde-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="34cde-120">Gerekli `name` özniteliği, bu önbelleğe dağıtılmış önbellek etiketi Yardımcısı her örneği için depolanan bir anahtar olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="34cde-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span> <span data-ttu-id="34cde-121">Temel önbellek etiketi Razor sayfası adı ve konumu sayfasındaki razor etiket Yardımcısı'nın temel alarak her önbellek etiketi Yardımcısı örneği için bir anahtar atayan Yardımcısı, dağıtılmış önbellek etiketi Yardımcısı yalnızca anahtarıyla özniteliğini alır `name`</span><span class="sxs-lookup"><span data-stu-id="34cde-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the Tag Helper in the razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`</span></span>

<span data-ttu-id="34cde-122">Kullanım örneği:</span><span class="sxs-lookup"><span data-stu-id="34cde-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="34cde-123">Dağıtılmış önbellek etiketi Yardımcısı IDistributedCache uygulamaları</span><span class="sxs-lookup"><span data-stu-id="34cde-123">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="34cde-124">İki uygulamaları vardır `IDistributedCache` ASP.NET Core için yerleşik olarak bulunur.</span><span class="sxs-lookup"><span data-stu-id="34cde-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span> <span data-ttu-id="34cde-125">Bir SQL Sunucusu'nu temel alır ve diğer Redis temel alır.</span><span class="sxs-lookup"><span data-stu-id="34cde-125">One is based on SQL Server and the other is based on Redis.</span></span> <span data-ttu-id="34cde-126">Bu uygulamalar ayrıntıları bulunabilir <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="34cde-126">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="34cde-127">Bir örneği her iki uygulamaları içeren `IDistributedCache` ASP.NET core'da'nın *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="34cde-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's *Startup.cs*.</span></span>

<span data-ttu-id="34cde-128">Tüm özel uygulanışı kullanmaya özellikle ilişkili hiçbir etiket öznitelik `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="34cde-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="34cde-129">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="34cde-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
