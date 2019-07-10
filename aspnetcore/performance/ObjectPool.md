---
title: ASP.NET core'da TransactionAffinity ile nesne yeniden kullanma
author: rick-anderson
description: TransactionAffinity kullanarak ASP.NET Core uygulamalarında performans artırmaya yönelik ipuçları.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 4/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 92106d5add7dbda36e451614429baa0db420f0e8
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724858"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a>ASP.NET core'da TransactionAffinity ile nesne yeniden kullanma

Tarafından [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.ObjectPool> toplanabilir nesneler çöp yerine nesne grubu yeniden kullanım için bellekte tutulması destekler ASP.NET Core altyapısının bir parçasıdır.

Yönetilen nesneleri nesne havuzu kullanmak isteyebilirsiniz:

- Pahalı tahsis başlatılamadı.
- Bazı sınırlı kaynak temsil eder.
- Tahmin edilebilir bir biçimde ve sık sık kullanılır.

Örneğin, ASP.NET Core framework yeniden kullanmak için bazı yerlerde nesne havuzu kullanır <xref:System.Text.StringBuilder> örnekleri. `StringBuilder` ayırır ve karakter verileri tutmak için kendi duyulan arabellekleri yöneten. ASP.NET Core düzenli olarak kullandığı `StringBuilder` uygulamak için özellikleri ve bunları yeniden bir performans kazancı sağlar.

Nesne havuzu her zaman performansı değil:

- Bir nesnenin başlangıç maliyeti yüksek olmadığı sürece, nesne havuzdan almak genellikle daha yavaştır.
- Havuz paylaştırılmamış olana kadar havuzu tarafından yönetilen nesnelere paylaştırılmamış değildir.

Yalnızca uygulama veya kitaplık için gerçekçi senaryoları kullanarak performans verilerini topladıktan sonra nesne havuzu kullanın.

**UYARI: `ObjectPool` Uygulamayan `IDisposable`. Çıkarma gereken türleri ile kullanımı önerilmemektedir.**

**NOT: TransactionAffinity, tahsis nesneleri sayısına bir sınır yerleştirmez, nesneleri, korur sayısına bir sınır yerleştirir.**

## <a name="concepts"></a>Kavramlar

<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> -temel nesne havuzu Özet. Alma ve nesneleri döndürmek için kullanılır.

<xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> -Bu nesneyi nasıl oluşturulduğunu ve nasıl özelleştirileceği uygulamak *sıfırlama* havuza geri döndürüldüğünde. Bu, doğrudan oluşturmak bir nesne havuzu geçirilebilir... VEYA

<xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> Nesne havuzu oluşturmak için bir üreteci olarak görev yapar.
<!-- REview, there is no ObjectPoolProvider<T> -->

Bir uygulamada birden çok yolla TransactionAffinity kullanılabilir:

* Bir havuz örnekleme.
* Bir havuzda kaydetme [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) bir örneği olarak.
* Kaydetme `ObjectPoolProvider<>` DI ve bir Fabrika kullanarak.

## <a name="how-to-use-objectpool"></a>TransactionAffinity kullanma

Çağrı <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> bir nesne almak için ve <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> nesneyi döndürmek için.  Her nesnenin dönüş gereksinimi yoktur. Bir nesne döndürmeyin, çöp olarak toplanacak olacaktır.

## <a name="objectpool-sample"></a>TransactionAffinity örnek

Aşağıdaki kodu:

* Ekler `ObjectPoolProvider` için [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) kapsayıcı.
* Ekler ve yapılandırır `ObjectPool<StringBuilder>` DI kapsayıcısı.
* Ekler `BirthdayMiddleware`.

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

Aşağıdaki kod uygular `BirthdayMiddleware`

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
