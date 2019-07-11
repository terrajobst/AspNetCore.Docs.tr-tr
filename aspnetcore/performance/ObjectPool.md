---
title: ASP.NET core'da TransactionAffinity ile nesne yeniden kullanma
author: rick-anderson
description: TransactionAffinity kullanarak ASP.NET Core uygulamalarında performans artırmaya yönelik ipuçları.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 771f19e54a908b8b2cd85ff72f368f16e94a2310
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815520"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a><span data-ttu-id="e39e1-103">ASP.NET core'da TransactionAffinity ile nesne yeniden kullanma</span><span class="sxs-lookup"><span data-stu-id="e39e1-103">Object reuse with ObjectPool in ASP.NET Core</span></span>

<span data-ttu-id="e39e1-104">Tarafından [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e39e1-104">By [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e39e1-105"><xref:Microsoft.Extensions.ObjectPool> toplanabilir nesneler çöp yerine nesne grubu yeniden kullanım için bellekte tutulması destekler ASP.NET Core altyapısının bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="e39e1-105"><xref:Microsoft.Extensions.ObjectPool> is part of the ASP.NET Core infrastructure that supports keeping a group of objects in memory for reuse rather than allowing the objects to be garbage collected.</span></span>

<span data-ttu-id="e39e1-106">Yönetilen nesneleri nesne havuzu kullanmak isteyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e39e1-106">You might want to use the object pool if the objects that are being managed are:</span></span>

- <span data-ttu-id="e39e1-107">Pahalı tahsis başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="e39e1-107">Expensive to allocate/initialize.</span></span>
- <span data-ttu-id="e39e1-108">Bazı sınırlı kaynak temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e39e1-108">Represent some limited resource.</span></span>
- <span data-ttu-id="e39e1-109">Tahmin edilebilir bir biçimde ve sık sık kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e39e1-109">Used predictably and frequently.</span></span>

<span data-ttu-id="e39e1-110">Örneğin, ASP.NET Core framework yeniden kullanmak için bazı yerlerde nesne havuzu kullanır <xref:System.Text.StringBuilder> örnekleri.</span><span class="sxs-lookup"><span data-stu-id="e39e1-110">For example, the ASP.NET Core framework uses the object pool in some places to reuse <xref:System.Text.StringBuilder> instances.</span></span> <span data-ttu-id="e39e1-111">`StringBuilder` ayırır ve karakter verileri tutmak için kendi duyulan arabellekleri yöneten.</span><span class="sxs-lookup"><span data-stu-id="e39e1-111">`StringBuilder` allocates and manages its own buffers to hold character data.</span></span> <span data-ttu-id="e39e1-112">ASP.NET Core düzenli olarak kullandığı `StringBuilder` uygulamak için özellikleri ve bunları yeniden bir performans kazancı sağlar.</span><span class="sxs-lookup"><span data-stu-id="e39e1-112">ASP.NET Core regularly uses `StringBuilder` to implement features, and reusing them provides a performance benefit.</span></span>

<span data-ttu-id="e39e1-113">Nesne havuzu her zaman performansı değil:</span><span class="sxs-lookup"><span data-stu-id="e39e1-113">Object pooling doesn't always improve performance:</span></span>

- <span data-ttu-id="e39e1-114">Bir nesnenin başlangıç maliyeti yüksek olmadığı sürece, nesne havuzdan almak genellikle daha yavaştır.</span><span class="sxs-lookup"><span data-stu-id="e39e1-114">Unless the initialization cost of an object is high, it's usually slower to get the object from the pool.</span></span>
- <span data-ttu-id="e39e1-115">Havuz paylaştırılmamış olana kadar havuzu tarafından yönetilen nesnelere paylaştırılmamış değildir.</span><span class="sxs-lookup"><span data-stu-id="e39e1-115">Objects managed by the pool aren't de-allocated until the pool is de-allocated.</span></span>

<span data-ttu-id="e39e1-116">Yalnızca uygulama veya kitaplık için gerçekçi senaryoları kullanarak performans verilerini topladıktan sonra nesne havuzu kullanın.</span><span class="sxs-lookup"><span data-stu-id="e39e1-116">Use object pooling only after collecting performance data using realistic scenarios for your app or library.</span></span>

<span data-ttu-id="e39e1-117">**UYARI: `ObjectPool` Uygulamayan `IDisposable`. Çıkarma gereken türleri ile kullanımı önerilmemektedir.**</span><span class="sxs-lookup"><span data-stu-id="e39e1-117">**WARNING: The `ObjectPool` doesn't implement `IDisposable`. We don't recommend using it with types that need disposal.**</span></span>

<span data-ttu-id="e39e1-118">**NOT: TransactionAffinity, tahsis nesneleri sayısına bir sınır yerleştirmez, nesneleri, korur sayısına bir sınır yerleştirir.**</span><span class="sxs-lookup"><span data-stu-id="e39e1-118">**NOTE: The ObjectPool doesn't place a limit on the number of objects that it will allocate, it places a limit on the number of objects it will retain.**</span></span>

## <a name="concepts"></a><span data-ttu-id="e39e1-119">Kavramlar</span><span class="sxs-lookup"><span data-stu-id="e39e1-119">Concepts</span></span>

<span data-ttu-id="e39e1-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> -temel nesne havuzu Özet.</span><span class="sxs-lookup"><span data-stu-id="e39e1-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> - the basic object pool abstraction.</span></span> <span data-ttu-id="e39e1-121">Alma ve nesneleri döndürmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e39e1-121">Used to get and return objects.</span></span>

<span data-ttu-id="e39e1-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> -Bu nesneyi nasıl oluşturulduğunu ve nasıl özelleştirileceği uygulamak *sıfırlama* havuza geri döndürüldüğünde.</span><span class="sxs-lookup"><span data-stu-id="e39e1-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> - implement this to customize how an object is created and how it is *reset* when returned to the pool.</span></span> <span data-ttu-id="e39e1-123">Bu, doğrudan oluşturmak bir nesne havuzu geçirilebilir... VEYA</span><span class="sxs-lookup"><span data-stu-id="e39e1-123">This can be passed into an object pool that you construct directly.... OR</span></span>

<span data-ttu-id="e39e1-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> Nesne havuzu oluşturmak için bir üreteci olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="e39e1-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> acts as a factory for creating object pools.</span></span>
<!-- REview, there is no ObjectPoolProvider<T> -->

<span data-ttu-id="e39e1-125">Bir uygulamada birden çok yolla TransactionAffinity kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="e39e1-125">The ObjectPool can be used in an app in multiple ways:</span></span>

* <span data-ttu-id="e39e1-126">Bir havuz örnekleme.</span><span class="sxs-lookup"><span data-stu-id="e39e1-126">Instantiating a pool.</span></span>
* <span data-ttu-id="e39e1-127">Bir havuzda kaydetme [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) bir örneği olarak.</span><span class="sxs-lookup"><span data-stu-id="e39e1-127">Registering a pool in [Dependency injection](xref:fundamentals/dependency-injection) (DI) as an instance.</span></span>
* <span data-ttu-id="e39e1-128">Kaydetme `ObjectPoolProvider<>` DI ve bir Fabrika kullanarak.</span><span class="sxs-lookup"><span data-stu-id="e39e1-128">Registering the `ObjectPoolProvider<>` in DI and using it as a factory.</span></span>

## <a name="how-to-use-objectpool"></a><span data-ttu-id="e39e1-129">TransactionAffinity kullanma</span><span class="sxs-lookup"><span data-stu-id="e39e1-129">How to use ObjectPool</span></span>

<span data-ttu-id="e39e1-130">Çağrı <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> bir nesne almak için ve <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> nesneyi döndürmek için.</span><span class="sxs-lookup"><span data-stu-id="e39e1-130">Call <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> to get an object and <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> to return the object.</span></span>  <span data-ttu-id="e39e1-131">Her nesnenin dönüş gereksinimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="e39e1-131">There's no requirement that you return every object.</span></span> <span data-ttu-id="e39e1-132">Bir nesne döndürmeyin, çöp olarak toplanacak olacaktır.</span><span class="sxs-lookup"><span data-stu-id="e39e1-132">If you don't return an object, it will be garbage collected.</span></span>

## <a name="objectpool-sample"></a><span data-ttu-id="e39e1-133">TransactionAffinity örnek</span><span class="sxs-lookup"><span data-stu-id="e39e1-133">ObjectPool sample</span></span>

<span data-ttu-id="e39e1-134">Aşağıdaki kodu:</span><span class="sxs-lookup"><span data-stu-id="e39e1-134">The following code:</span></span>

* <span data-ttu-id="e39e1-135">Ekler `ObjectPoolProvider` için [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="e39e1-135">Adds `ObjectPoolProvider` to the [Dependency injection](xref:fundamentals/dependency-injection) (DI) container.</span></span>
* <span data-ttu-id="e39e1-136">Ekler ve yapılandırır `ObjectPool<StringBuilder>` DI kapsayıcısı.</span><span class="sxs-lookup"><span data-stu-id="e39e1-136">Adds and configures `ObjectPool<StringBuilder>` to the DI container.</span></span>
* <span data-ttu-id="e39e1-137">Ekler `BirthdayMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="e39e1-137">Adds the `BirthdayMiddleware`.</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

<span data-ttu-id="e39e1-138">Aşağıdaki kod uygular `BirthdayMiddleware`</span><span class="sxs-lookup"><span data-stu-id="e39e1-138">The following code implements `BirthdayMiddleware`</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
