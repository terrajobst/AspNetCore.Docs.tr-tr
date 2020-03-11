---
title: ASP.NET Core dağıtılmış önbellek etiketi Yardımcısı
author: pkellner
description: Dağıtılmış önbellek etiketi yardımcısını nasıl kullanacağınızı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 01/24/2020
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: f5957adf3cef8966812a1bf0cbc6b2627d19d026
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664019"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>ASP.NET Core dağıtılmış önbellek etiketi Yardımcısı

By [Peter Kellner](https://peterkellner.net)

Dağıtılmış önbellek etiketi Yardımcısı, içeriğini dağıtılmış bir önbellek kaynağına önbelleğe alarak ASP.NET Core uygulamanızın performansını önemli ölçüde iyileştirebilme olanağı sağlar.

Etiket Yardımcıları hakkında genel bilgi için bkz. <xref:mvc/views/tag-helpers/intro>.

Dağıtılmış önbellek etiketi Yardımcısı, önbellek etiketi Yardımcısı ile aynı temel sınıftan devralınır. Tüm [önbellek etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) öznitelikleri, dağıtılmış etiket Yardımcısı tarafından kullanılabilir.

Dağıtılmış önbellek etiketi Yardımcısı, [Oluşturucu Ekleme](xref:fundamentals/dependency-injection#constructor-injection-behavior)işlemini kullanır. <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> arabirimi, dağıtılmış önbellek etiketi Yardımcısı 'nın oluşturucusuna geçirilir. `Startup.ConfigureServices` (*Startup.cs*) içinde `IDistributedCache` somut bir uygulama oluşturulmadıysa, dağıtılmış önbellek etiketi Yardımcısı önbelleğe alınmış verileri [önbellek etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)olarak depolamak için aynı bellek içi sağlayıcıyı kullanır.

## <a name="distributed-cache-tag-helper-attributes"></a>Dağıtılmış önbellek etiketi Yardımcısı öznitelikleri

### <a name="attributes-shared-with-the-cache-tag-helper"></a>Önbellek etiketi Yardımcısı ile paylaşılan öznitelikler

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

Dağıtılmış önbellek etiketi Yardımcısı, önbellek etiketi Yardımcısı ile aynı sınıftan devralınır. Bu özniteliklerin açıklamaları için [önbellek etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)' na bakın.

### <a name="name"></a>ad

| Öznitelik türü | Örnek                               |
| -------------- | ------------------------------------- |
| String         | `my-distributed-cache-unique-key-101` |

`name` gereklidir. `name` özniteliği, depolanan her önbellek örneği için bir anahtar olarak kullanılır. Razor sayfasındaki Razor sayfası adı ve konumuna göre her örneğe bir önbellek anahtarı atayan önbellek etiketi Yardımcısı 'nın aksine, dağıtılmış önbellek etiketi Yardımcısı yalnızca anahtarını öznitelik `name`temel alır.

Örnek:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Dağıtılmış önbellek etiketi Yardımcısı ıdistributedönbellek uygulamaları

ASP.NET Core için yerleşik <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> iki uygulaması vardır. Biri SQL Server tabanlıdır ve diğeri redin tabanlıdır. [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)gibi üçüncü taraf uygulamalar da mevcuttur. Bu uygulamaların ayrıntıları <xref:performance/caching/distributed>' de bulunabilir. Her iki uygulama da `Startup`bir `IDistributedCache` örneğini ayarlamayı içerir.

`IDistributedCache`belirli bir uygulamasını kullanmayla özellikle ilişkili etiket özniteliği yok.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
