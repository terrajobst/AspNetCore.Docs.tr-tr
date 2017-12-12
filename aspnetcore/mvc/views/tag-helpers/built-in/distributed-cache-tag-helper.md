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
# <a name="distributed-cache-tag-helper"></a>Dağıtılmış önbellek etiket Yardımcısı

Tarafından [Peter Kellner](http://peterkellner.net) 


Dağıtılmış önbellek etiket Yardımcısı dağıtılmış önbellek kaynağı için içeriği önbelleğe alarak ASP.NET Core uygulamanızın performansını önemli ölçüde artırmak için yeteneği sağlar.

Dağıtılmış önbellek etiket Yardımcısı önbellek etiket Yardımcısı ile aynı temel sınıfından devralır.  Önbellek etiketi Yardımcıyla ilişkili tüm öznitelikleri üzerinde dağıtılmış etiket Yardımcısı da çalışır.


Dağıtılmış önbellek etiket Yardımcısı izleyen **açık bağımlılıkları ilkesine** olarak bilinen **Oluşturucu ekleme**.  Özellikle, `IDistributedCache` arabirimi kapsayıcı dağıtılmış önbellek etiketi yardımcının oluşturucuya geçirilir.  Belirli hiçbir somut uygulaması varsa `IDistributedCache` içinde oluşturulan `ConfigureServices`, genellikle haline içinde bulunan ve dağıtılmış önbellek etiket Yardımcısı aynı bellek içi sağlayıcısı temel önbellek etiket Yardımcısı olarak önbelleğe alınmış verileri depolamak için kullanır.

## <a name="distributed-cache-tag-helper-attributes"></a>Dağıtılmış önbellek etiketi yardımcı öznitelik

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>süresi dolmadan üzerinde süresi dolduktan sonra süresi dolar-kayan etkin farklılık tarafından-üstbilgi farklılık-by-sorgusu farklılık tarafından-yönlendirme farklılık-tarafından-cookie farklılık kullanıcıya göre farklılık-önceliğe göre

Önbellek etiket Yardımcısı tanımları için bkz. Bu öznitelikler önbellek etiket Yardımcısı yaygın şekilde dağıtılmış önbellek etiket Yardımcısı önbellek etiket Yardımcısı ile aynı sınıfta devralır.

- - -

### <a name="name-required"></a>adı (gerekli)

| Öznitelik türü    | Örnek değer     |
|----------------   |----------------   |
| dize    | "my-distributed-cache-unique-key-101"     |

Gerekli `name` özniteliği dağıtılmış önbellek etiket Yardımcısı her örneği için depolanan bu önbelleğine anahtarı olarak kullanılır.  Temel önbellek etiketi Razor sayfa adı ve razor sayfasını etiketi yardımcı konumunu temel alarak her önbellek etiket Yardımcısı örneği için bir anahtar atayan yardımcıyı farklı olarak, dağıtılmış önbellek etiket Yardımcısı yalnızca, anahtar özniteliği taban`name`

Kullanım örneği:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Dağıtılmış önbellek etiket Yardımcısı IDistributedCache uygulamaları

İki uygulamaları vardır `IDistributedCache` ASP.NET Core yerleşik olarak bulunur.  Bir temel **Sql Server** ve diğer dayanır **Redis**. Bu uygulamaların ayrıntılarını adlandırılmış "ile çalışmayı dağıtılmış önbellek" aşağıda başvurulan kaynakta bulunamadı. Bir örneği iki uygulamaları içeren `IDistributedCache` ASP.NET Core'nın içinde **haline**.

Özellikle herhangi belirli uyarlamasını kullanımıyla ilişkili hiçbir etiket öznitelik `IDistributedCache`.



- - -



## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
