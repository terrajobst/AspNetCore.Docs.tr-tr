---
title: ASP.NET Core içinde ObjectPool ile nesne yeniden kullanımı
author: rick-anderson
description: ObjectPool kullanan ASP.NET Core uygulamalarında performansı artırmaya yönelik ipuçları.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 771f19e54a908b8b2cd85ff72f368f16e94a2310
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666112"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a>ASP.NET Core içinde ObjectPool ile nesne yeniden kullanımı

, [Steve Gordon](https://twitter.com/stevejgordon), [Ryan şimdi ak](https://github.com/rynowak)ve [Rick Anderson](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.ObjectPool>, nesnelerin çöp toplanmasına izin vermek yerine, bir nesne grubunu yeniden kullanım için bellekte tutmayı destekleyen ASP.NET Core altyapısının bir parçasıdır.

Yönetilebilecek nesneler şunları içeriyorsa, nesne havuzunu kullanmak isteyebilirsiniz:

- Ayırma/başlatma pahalıdır.
- Sınırlı bir kaynağı temsil eder.
- Tahmin edilebilir ve sık kullanılır.

Örneğin, ASP.NET Core Framework <xref:System.Text.StringBuilder> örnekleri yeniden kullanmak için bazı yerlerde nesne havuzunu kullanır. `StringBuilder` karakter verilerini tutmak için kendi arabelleğini ayırır ve yönetir. ASP.NET Core düzenli olarak özellik uygulamak için `StringBuilder` kullanır ve bunları yeniden kullanmak bir performans avantajı sağlar.

Nesne havuzu her zaman performansı iyileştirmez:

- Bir nesnenin başlatma maliyeti yüksek değilse, havuzdan nesneyi almak genellikle daha yavaştır.
- Havuz tarafından yönetilen nesneler, havuz serbest olarak ayrılana kadar ayrılmış değildir.

Yalnızca uygulamanız veya kitaplığınız için gerçekçi senaryolar kullanarak performans verilerini topladıktan sonra nesne havuzunu kullanın.

**Uyarı: `ObjectPool` `IDisposable`uygulamıyor. Bunu, aktiften çıkarma gerektiren türlerle kullanmanızı önermiyoruz.**

**UNUTMAYıN: ObjectPool, ayırabilecek nesne sayısına bir sınır yerleştirmez, saklanacak nesne sayısına bir sınır koyar.**

## <a name="concepts"></a>Kavramlar

<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1>-temel nesne havuzu soyutlama. Nesneleri almak ve döndürmek için kullanılır.

<xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601>, bir nesnenin nasıl oluşturulduğunu ve havuza döndürüldüğünde nasıl *sıfırlandığını* özelleştirmek için bunu uygulayın. Bu, doğrudan oluşturduğunuz bir nesne havuzuna geçirilebilir.... VEYA

<xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*>, nesne havuzları oluşturmak için bir fabrika işlevi görür.
<!-- REview, there is no ObjectPoolProvider<T> -->

ObjectPool bir uygulamada birden çok şekilde kullanılabilir:

* Havuz örneği oluşturuluyor.
* Bir havuzu bir örnek olarak [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) içinde kaydetme.
* `ObjectPoolProvider<>`, DI 'ye kaydediliyor ve fabrika olarak kullanılıyor.

## <a name="how-to-use-objectpool"></a>ObjectPool kullanma

Nesne almak için <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> çağırın ve nesneyi döndürmek için <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*>.  Her nesneyi döndürmenizde gereksinim yoktur. Bir nesne döndürmezseniz atık olarak toplanacaktır.

## <a name="objectpool-sample"></a>ObjectPool örneği

Aşağıdaki kod:

* [Bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) kapsayıcısına `ObjectPoolProvider` ekler.
* Dı kapsayıcısına `ObjectPool<StringBuilder>` ekler ve yapılandırır.
* `BirthdayMiddleware`ekler.

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

Aşağıdaki kod `BirthdayMiddleware` uygular

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
