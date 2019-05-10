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
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Dağıtılmış önbellek etiketi Yardımcısı ASP.NET core'da

Tarafından [Peter Kellner](http://peterkellner.net) ve [Luke Latham](https://github.com/guardrex)

Dağıtılmış önbellek etiketi Yardımcısı, dağıtılmış önbellek kaynağına içeriği önbelleğe alarak ASP.NET Core uygulamanızı performansını önemli ölçüde artırmak olanağı sağlar.

Etiket Yardımcıları genel bakış için bkz. <xref:mvc/views/tag-helpers/intro>.

Dağıtılmış önbellek etiketi Yardımcısı, önbellek etiketi Yardımcısı olarak aynı temel sınıfından devralır. Tüm [önbellek etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) öznitelikleri dağıtılmış etiketi Yardımcısı için kullanılabilir.

Dağıtılmış önbellek etiketi Yardımcısı kullanan [Oluşturucu ekleme](xref:fundamentals/dependency-injection#constructor-injection-behavior). <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> Arabirimi, dağıtılmış önbellek etiketi Yardımcısı'nın oluşturucuya geçirilir. Hiçbir somut uygulaması varsa `IDistributedCache` oluşturulur `Startup.ConfigureServices` (*Startup.cs*), dağıtılmış önbellek etiketi Yardımcısı olarak önbelleğe alınmış verileri depolamak için aynı bellek içi sağlayıcısı kullanan [önbellek etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

## <a name="distributed-cache-tag-helper-attributes"></a>Dağıtılmış önbellek etiketi Yardımcısı öznitelikleri

### <a name="attributes-shared-with-the-cache-tag-helper"></a>Önbellek etiketi Yardımcısı ile paylaşılan öznitelikleri

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

Önbellek etiketi Yardımcısı aynı sınıfta dağıtılmış önbellek etiketi Yardımcısı devralır. Bu öznitelikler açıklaması için bkz. [önbellek etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="name"></a>name

| Öznitelik türü | Örnek                               |
| -------------- | ------------------------------------- |
| Dize         | `my-distributed-cache-unique-key-101` |

`name` gerekli değildir. `name` Özniteliği, her depolanan önbellek örneği için bir anahtar olarak kullanılır. Önbellek etiketi Razor sayfası adı ve Razor sayfası konuma göre her bir örneği için bir önbellek anahtarı atayan Yardımcısı, dağıtılmış önbellek etiketi Yardımcısı yalnızca anahtarıyla özniteliğini tabanları `name`.

Örnek:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Dağıtılmış önbellek etiketi Yardımcısı IDistributedCache uygulamaları

İki uygulamaları vardır <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ASP.NET Core için yerleşik olarak bulunur. Bir SQL Sunucusu'nu temel alır ve diğer Redis dayanır. Bu uygulamalar ayrıntıları bulunabilir <xref:performance/caching/distributed>. Bir örneği her iki uygulamaları içeren `IDistributedCache` içinde `Startup`.

Tüm özel uygulanışı kullanmaya özellikle ilişkili hiçbir etiket öznitelik `IDistributedCache`.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
