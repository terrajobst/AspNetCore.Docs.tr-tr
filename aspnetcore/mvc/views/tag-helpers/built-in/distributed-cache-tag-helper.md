---
title: Dağıtılmış önbellek etiketi Yardımcısı ASP.NET core'da
author: pkellner
description: Dağıtılmış önbellek etiketi Yardımcısı'nı kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a5b33451a763c297c6d7885855a321c43435abb4
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902627"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="27e5a-103">Dağıtılmış önbellek etiketi Yardımcısı ASP.NET core'da</span><span class="sxs-lookup"><span data-stu-id="27e5a-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="27e5a-104">Tarafından [Peter Kellner](http://peterkellner.net) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="27e5a-104">By [Peter Kellner](http://peterkellner.net) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="27e5a-105">Dağıtılmış önbellek etiketi Yardımcısı, dağıtılmış önbellek kaynağına içeriği önbelleğe alarak ASP.NET Core uygulamanızı performansını önemli ölçüde artırmak olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="27e5a-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="27e5a-106">Etiket Yardımcıları genel bakış için bkz. <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="27e5a-106">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="27e5a-107">Dağıtılmış önbellek etiketi Yardımcısı, önbellek etiketi Yardımcısı olarak aynı temel sınıfından devralır.</span><span class="sxs-lookup"><span data-stu-id="27e5a-107">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="27e5a-108">Tüm [önbellek etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) öznitelikleri dağıtılmış etiketi Yardımcısı için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="27e5a-108">All of the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) attributes are available to the Distributed Tag Helper.</span></span>

<span data-ttu-id="27e5a-109">Dağıtılmış önbellek etiketi Yardımcısı kullanan [Oluşturucu ekleme](xref:fundamentals/dependency-injection#constructor-injection-behavior).</span><span class="sxs-lookup"><span data-stu-id="27e5a-109">The Distributed Cache Tag Helper uses [constructor injection](xref:fundamentals/dependency-injection#constructor-injection-behavior).</span></span> <span data-ttu-id="27e5a-110"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> Arabirimi, dağıtılmış önbellek etiketi Yardımcısı'nın oluşturucuya geçirilir.</span><span class="sxs-lookup"><span data-stu-id="27e5a-110">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="27e5a-111">Hiçbir somut uygulaması varsa `IDistributedCache` oluşturulur `Startup.ConfigureServices` (*Startup.cs*), dağıtılmış önbellek etiketi Yardımcısı olarak önbelleğe alınmış verileri depolamak için aynı bellek içi sağlayıcısı kullanan [önbellek etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="27e5a-111">If no concrete implementation of `IDistributedCache` is created in `Startup.ConfigureServices` (*Startup.cs*), the Distributed Cache Tag Helper uses the same in-memory provider for storing cached data as the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="27e5a-112">Dağıtılmış önbellek etiketi Yardımcısı öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="27e5a-112">Distributed Cache Tag Helper Attributes</span></span>

### <a name="attributes-shared-with-the-cache-tag-helper"></a><span data-ttu-id="27e5a-113">Önbellek etiketi Yardımcısı ile paylaşılan öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="27e5a-113">Attributes shared with the Cache Tag Helper</span></span>

* `enabled`
* `expires-on`
* `expires-after`
* `expires-sliding`
* `vary-by-header`
* `vary-by-query`
* `vary-by-route`
* `vary-by-cookie`
* `vary-by-user`
* `vary-by priority`

<span data-ttu-id="27e5a-114">Önbellek etiketi Yardımcısı aynı sınıfta dağıtılmış önbellek etiketi Yardımcısı devralır.</span><span class="sxs-lookup"><span data-stu-id="27e5a-114">The Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper.</span></span> <span data-ttu-id="27e5a-115">Bu öznitelikler açıklaması için bkz. [önbellek etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="27e5a-115">For descriptions of these attributes, see the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="name"></a><span data-ttu-id="27e5a-116">name</span><span class="sxs-lookup"><span data-stu-id="27e5a-116">name</span></span>

| <span data-ttu-id="27e5a-117">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="27e5a-117">Attribute Type</span></span> | <span data-ttu-id="27e5a-118">Örnek</span><span class="sxs-lookup"><span data-stu-id="27e5a-118">Example</span></span>                               |
| -------------- | ------------------------------------- |
| <span data-ttu-id="27e5a-119">Dize</span><span class="sxs-lookup"><span data-stu-id="27e5a-119">String</span></span>         | `my-distributed-cache-unique-key-101` |

<span data-ttu-id="27e5a-120">`name` gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="27e5a-120">`name` is required.</span></span> <span data-ttu-id="27e5a-121">`name` Özniteliği, her depolanan önbellek örneği için bir anahtar olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="27e5a-121">The `name` attribute is used as a key for each stored cache instance.</span></span> <span data-ttu-id="27e5a-122">Önbellek etiketi Razor sayfası adı ve Razor sayfası konuma göre her bir örneği için bir önbellek anahtarı atayan Yardımcısı, dağıtılmış önbellek etiketi Yardımcısı yalnızca anahtarıyla özniteliğini tabanları `name`.</span><span class="sxs-lookup"><span data-stu-id="27e5a-122">Unlike the Cache Tag Helper that assigns a cache key to each instance based on the Razor page name and location in the Razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`.</span></span>

<span data-ttu-id="27e5a-123">Örnek:</span><span class="sxs-lookup"><span data-stu-id="27e5a-123">Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="27e5a-124">Dağıtılmış önbellek etiketi Yardımcısı IDistributedCache uygulamaları</span><span class="sxs-lookup"><span data-stu-id="27e5a-124">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="27e5a-125">İki uygulamaları vardır <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ASP.NET Core için yerleşik olarak bulunur.</span><span class="sxs-lookup"><span data-stu-id="27e5a-125">There are two implementations of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> built in to ASP.NET Core.</span></span> <span data-ttu-id="27e5a-126">Bir SQL Sunucusu'nu temel alır ve diğer Redis dayanır.</span><span class="sxs-lookup"><span data-stu-id="27e5a-126">One is based on SQL Server, and the other is based on Redis.</span></span> <span data-ttu-id="27e5a-127">Bu uygulamalar ayrıntıları bulunabilir <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="27e5a-127">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="27e5a-128">Bir örneği her iki uygulamaları içeren `IDistributedCache` içinde `Startup`.</span><span class="sxs-lookup"><span data-stu-id="27e5a-128">Both implementations involve setting an instance of `IDistributedCache` in `Startup`.</span></span>

<span data-ttu-id="27e5a-129">Tüm özel uygulanışı kullanmaya özellikle ilişkili hiçbir etiket öznitelik `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="27e5a-129">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="27e5a-130">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="27e5a-130">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
