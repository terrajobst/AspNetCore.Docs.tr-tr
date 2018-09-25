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
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Dağıtılmış önbellek etiketi Yardımcısı ASP.NET core'da

Tarafından [Peter Kellner](http://peterkellner.net) 

Dağıtılmış önbellek etiketi Yardımcısı, dağıtılmış önbellek kaynağına içeriği önbelleğe alarak ASP.NET Core uygulamanızı performansını önemli ölçüde artırmak olanağı sağlar.

Dağıtılmış önbellek etiketi Yardımcısı, önbellek etiketi Yardımcısı olarak aynı temel sınıfından devralır. Önbellek etiketi Yardımcısı ile ilişkili tüm öznitelikler, üzerinde dağıtılmış etiketi Yardımcısı olarak da çalışır.

Dağıtılmış önbellek etiketi Yardımcısı izleyen **açık bağımlılıkları İlkesi** olarak bilinen **Oluşturucu ekleme**. Özellikle, `IDistributedCache` arabirimi kapsayıcı, dağıtılmış önbellek etiketi Yardımcısı'nın oluşturucuya geçirilir. Belirli bir somut uygulamasına varsa `IDistributedCache` içinde oluşturulan `ConfigureServices`, genellikle startup.cs içinde bulunan ve ardından dağıtılmış önbellek etiketi Yardımcısı temel önbellek etiketi Yardımcısı önbelleğe alınmış verileri depolamak için aynı bellek içi sağlayıcısı kullanır.

## <a name="distributed-cache-tag-helper-attributes"></a>Dağıtılmış önbellek etiketi Yardımcısı öznitelikleri

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>süresi dolmadan açma süresi dolduktan sonra süresi dolar-kayan etkin farklılık-tarafından-header farklılık-tarafından-sorgu değişir-tarafından-route farklılık-tarafından-tanımlama bilgisi değişiklik kullanıcı tarafından farklılık-önceliğe göre

Önbellek etiketi Yardımcısı tanımları için bkz. Bu öznitelikler önbellek etiketi Yardımcısı ' yaygın şekilde dağıtılmış önbellek etiketi Yardımcısı önbellek etiketi Yardımcısı olarak aynı sınıfından devralır.

- - -

### <a name="name-required"></a>ad (gerekli)

| Öznitelik türü    | Örnek değer     |
|----------------   |----------------   |
| dize    | "my-distributed-cache-unique-key-101"     |

Gerekli `name` özniteliği, bu önbelleğe dağıtılmış önbellek etiketi Yardımcısı her örneği için depolanan bir anahtar olarak kullanılır. Temel önbellek etiketi Razor sayfası adı ve konumu sayfasındaki razor etiket Yardımcısı'nın temel alarak her önbellek etiketi Yardımcısı örneği için bir anahtar atayan Yardımcısı, dağıtılmış önbellek etiketi Yardımcısı yalnızca anahtarıyla özniteliğini alır `name`

Kullanım örneği:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Dağıtılmış önbellek etiketi Yardımcısı IDistributedCache uygulamaları

İki uygulamaları vardır `IDistributedCache` ASP.NET Core için yerleşik olarak bulunur. Bir SQL Sunucusu'nu temel alır ve diğer Redis temel alır. Bu uygulamalar ayrıntıları bulunabilir <xref:performance/caching/distributed>. Bir örneği her iki uygulamaları içeren `IDistributedCache` ASP.NET core'da'nın *Startup.cs*.

Tüm özel uygulanışı kullanmaya özellikle ilişkili hiçbir etiket öznitelik `IDistributedCache`.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
