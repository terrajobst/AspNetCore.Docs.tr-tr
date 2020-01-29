---
title: ASP.NET Core dağıtılmış önbellek etiketi Yardımcısı
author: pkellner
description: Dağıtılmış önbellek etiketi yardımcısını nasıl kullanacağınızı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 01/24/2020
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: e5100d7244600358186b653073990985f48434a7
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76809061"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="dd2b4-103">ASP.NET Core dağıtılmış önbellek etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="dd2b4-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="dd2b4-104">By [Peter Kellner](https://peterkellner.net) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="dd2b4-104">By [Peter Kellner](https://peterkellner.net) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="dd2b4-105">Dağıtılmış önbellek etiketi Yardımcısı, içeriğini dağıtılmış bir önbellek kaynağına önbelleğe alarak ASP.NET Core uygulamanızın performansını önemli ölçüde iyileştirebilme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="dd2b4-106">Etiket Yardımcıları hakkında genel bilgi için bkz. <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-106">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="dd2b4-107">Dağıtılmış önbellek etiketi Yardımcısı, önbellek etiketi Yardımcısı ile aynı temel sınıftan devralınır.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-107">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="dd2b4-108">Tüm [önbellek etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) öznitelikleri, dağıtılmış etiket Yardımcısı tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-108">All of the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) attributes are available to the Distributed Tag Helper.</span></span>

<span data-ttu-id="dd2b4-109">Dağıtılmış önbellek etiketi Yardımcısı, [Oluşturucu Ekleme](xref:fundamentals/dependency-injection#constructor-injection-behavior)işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-109">The Distributed Cache Tag Helper uses [constructor injection](xref:fundamentals/dependency-injection#constructor-injection-behavior).</span></span> <span data-ttu-id="dd2b4-110"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> arabirimi, dağıtılmış önbellek etiketi Yardımcısı 'nın oluşturucusuna geçirilir.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-110">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="dd2b4-111">`Startup.ConfigureServices` (*Startup.cs*) içinde `IDistributedCache` somut bir uygulama oluşturulmadıysa, dağıtılmış önbellek etiketi Yardımcısı önbelleğe alınmış verileri [önbellek etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)olarak depolamak için aynı bellek içi sağlayıcıyı kullanır.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-111">If no concrete implementation of `IDistributedCache` is created in `Startup.ConfigureServices` (*Startup.cs*), the Distributed Cache Tag Helper uses the same in-memory provider for storing cached data as the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="dd2b4-112">Dağıtılmış önbellek etiketi Yardımcısı öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="dd2b4-112">Distributed Cache Tag Helper Attributes</span></span>

### <a name="attributes-shared-with-the-cache-tag-helper"></a><span data-ttu-id="dd2b4-113">Önbellek etiketi Yardımcısı ile paylaşılan öznitelikler</span><span class="sxs-lookup"><span data-stu-id="dd2b4-113">Attributes shared with the Cache Tag Helper</span></span>

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

<span data-ttu-id="dd2b4-114">Dağıtılmış önbellek etiketi Yardımcısı, önbellek etiketi Yardımcısı ile aynı sınıftan devralınır.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-114">The Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper.</span></span> <span data-ttu-id="dd2b4-115">Bu özniteliklerin açıklamaları için [önbellek etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)' na bakın.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-115">For descriptions of these attributes, see the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="name"></a><span data-ttu-id="dd2b4-116">{1&gt;name&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd2b4-116">name</span></span>

| <span data-ttu-id="dd2b4-117">Öznitelik türü</span><span class="sxs-lookup"><span data-stu-id="dd2b4-117">Attribute Type</span></span> | <span data-ttu-id="dd2b4-118">Örnek</span><span class="sxs-lookup"><span data-stu-id="dd2b4-118">Example</span></span>                               |
| -------------- | ------------------------------------- |
| <span data-ttu-id="dd2b4-119">Dize</span><span class="sxs-lookup"><span data-stu-id="dd2b4-119">String</span></span>         | `my-distributed-cache-unique-key-101` |

<span data-ttu-id="dd2b4-120">`name` gereklidir.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-120">`name` is required.</span></span> <span data-ttu-id="dd2b4-121">`name` özniteliği, depolanan her önbellek örneği için bir anahtar olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-121">The `name` attribute is used as a key for each stored cache instance.</span></span> <span data-ttu-id="dd2b4-122">Razor sayfasındaki Razor sayfası adı ve konumuna göre her örneğe bir önbellek anahtarı atayan önbellek etiketi Yardımcısı 'nın aksine, dağıtılmış önbellek etiketi Yardımcısı yalnızca anahtarını öznitelik `name`temel alır.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-122">Unlike the Cache Tag Helper that assigns a cache key to each instance based on the Razor page name and location in the Razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`.</span></span>

<span data-ttu-id="dd2b4-123">Örnek:</span><span class="sxs-lookup"><span data-stu-id="dd2b4-123">Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="dd2b4-124">Dağıtılmış önbellek etiketi Yardımcısı ıdistributedönbellek uygulamaları</span><span class="sxs-lookup"><span data-stu-id="dd2b4-124">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="dd2b4-125">ASP.NET Core için yerleşik <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> iki uygulaması vardır.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-125">There are two implementations of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> built in to ASP.NET Core.</span></span> <span data-ttu-id="dd2b4-126">Biri SQL Server tabanlıdır ve diğeri redin tabanlıdır.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-126">One is based on SQL Server, and the other is based on Redis.</span></span> <span data-ttu-id="dd2b4-127">[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)gibi üçüncü taraf uygulamalar da mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-127">Third-party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html).</span></span> <span data-ttu-id="dd2b4-128">Bu uygulamaların ayrıntıları <xref:performance/caching/distributed>' de bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-128">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="dd2b4-129">Her iki uygulama da `Startup`bir `IDistributedCache` örneğini ayarlamayı içerir.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-129">Both implementations involve setting an instance of `IDistributedCache` in `Startup`.</span></span>

<span data-ttu-id="dd2b4-130">`IDistributedCache`belirli bir uygulamasını kullanmayla özellikle ilişkili etiket özniteliği yok.</span><span class="sxs-lookup"><span data-stu-id="dd2b4-130">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dd2b4-131">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="dd2b4-131">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
