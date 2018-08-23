---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: '7. Bölüm: ana oluşturma sayfası | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: bb4704e7f4f13fab04acdbdd642174884517e18a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753367"
---
<a name="part-7-creating-the-main-page"></a>7. Bölüm: ana oluşturma sayfası
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Ana oluşturma sayfası

Bu bölümde, uygulama ana sayfası oluşturur. Bu sayfa, biz bunu birkaç adımda yaklaşımını şekilde yönetici sayfadan daha karmaşık olacaktır. Bu doğrultuda, bazı daha gelişmiş Knockout.js teknikleri görürsünüz. Temel düzen sayfasının şu şekildedir:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Ürün", bir dizi ürün tutar.
- "Sepeti" miktarlar ürünleriyle dizisi içerir. "Sepete Ekle"'ı tıklatarak, sepet güncelleştirir.
- "Siparişler", bir sipariş kimlikleri dizisi içerir.
- "Details" (miktarlar ürünleriyle) öğelerin bir dizisi olan bir sipariş ayrıntısı tutar.

Veri bağlama ya da betik HTML, bazı temel düzeni tanımlayarak başlayacağız. Views/Home/Index.cshtml dosyasını açın ve tüm içeriğini aşağıdakiyle değiştirin:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Ardından, betikleri bölüm ekleme ve boş bir görünüm modeli oluşturun:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Daha önce ince ince tasarımı görünümü modelimizi gözlemlenenler ürünleri, sepet, siparişler ve Ayrıntılar için gerekiyor. Aşağıdaki değişkenleri ekleyip `AppViewModel` nesnesi:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Kullanıcılar, öğeleri ürünleri listeden Sepete Ekle ve sepetinden öğeleri kaldırın. Bu işlevler yalıtılacak bir ürünü temsil eden başka bir görünüm modeli sınıf oluşturacağız. Aşağıdaki kodu ekleyin `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

`ProductViewModel` Sınıfı sepet gelen ve ürün taşımak için kullanılan iki işlevleri içerir: `addItemToCart` , sepete ürün bir birimi ekler ve `removeAllFromCart` ürün tüm miktarlarını kaldırır.

Kullanıcılar, var olan bir sırayı seçin ve Sipariş ayrıntılarını alın. Biz, başka bir görünüm modeli bu işlevselliği kapsülleyen:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

`OrderDetailsViewModel` Bir sıra ile başlatılır ve sunucuya bir AJAX isteği göndererek sipariş ayrıntılarını getirir.

Ayrıca, fark `total` özellikte `OrderDetailsViewModel`. Bu özellik, gözlemlenebilir adlı özel bir tür bir [observable hesaplanan](http://knockoutjs.com/documentation/computedObservables.html). Hesaplanan observable adından da anlaşılacağı gibi hesaplanan değeri verilerin bağlanacağı sağlar&#8212;toplam sırası bu durumda maliyet.

Ardından, bu işlevler için ekleme `AppViewModel`:

- `resetCart` tüm öğeleri sepetinden kaldırır.
- `getDetails` için bir sipariş ayrıntıları alır (yeni bir pusing tarafından `OrderDetailsViewModel` üzerine `details` listesi).
- `createOrder` Yeni bir sıra oluşturur ve sepet boşaltır.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Son olarak, ürünler ve siparişler için AJAX isteği yaparak görünüm modeli başlatın:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

Tamam, bir sürü kod olan, ancak biz yerleşik yedekleme adım adım şekilde Umarım tasarım işaretlenmemiştir. Şimdi biz HTML bazı Knockout.js bağlamaları ekleyebilirsiniz.

**Ürünler**

Ürün listesi için olan bağlamaları şunlardır:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Bu ürünler dizi içinde yinelenir ve fiyat ve adını görüntüler. "Order Ekle" düğmesi, yalnızca kullanıcı oturum açtığında görünür olur.

"Order Ekle" düğmesi çağrıları `addItemToCart` üzerinde `ProductViewModel` ürün için örneği. Bu güzel bir özelliği Knockout.js gösterir: görünüm modeli diğer görünüm modelleri içeriyorsa, iç modele bağlamalarını uygulayabilirsiniz. Bu örnekte, içinde bağlamaları `foreach` her bir uygulanan `ProductViewModel` örnekleri. Bu yaklaşım, tek bir görünüm-modeline tüm işlevlerin koyarak daha çok daha net olur.

**Cart**

Sepet bağlamalarda şunlardır:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Bu, sepet dizi içinde yinelenir ve ad, fiyat ve miktar görüntüler. İşlevler görünümü-model bağlantıyı "Kaldır" ve "Sipariş oluştur" düğmesine bağlı olduklarını unutmayın.

**Siparişler**

Siparişler listesi için olan bağlamaları şunlardır:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Bu siparişleri yinelenir ve sipariş kimliği gösterir Tıklama olayı bağlantısına bağlı `getDetails` işlevi.

**Sipariş Ayrıntıları**

Sipariş ayrıntılarını bağlamalarda şunlardır:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Bu sırada öğeler üzerinden yinelenir ve ürün, fiyatı ile miktarı görüntüler. Yalnızca ayrıntıları dizi bir veya daha fazla öğe içeriyorsa, çevreleyen div görülebilir.

## <a name="conclusion"></a>Sonuç

Bu öğreticide, veritabanı ve veri katmanı üzerinde bir genel kullanıma yönelik arabirim sağlamak üzere ASP.NET Web API ile iletişim kurmak için Entity Framework kullanan bir uygulama oluşturdunuz. ASP.NET MVC 4 Sayfa yeniden yükler olmadan dinamik etkileşim sağlamak için HTML sayfalarında ve Knockout.js artı jQuery işlemek için kullanırız.

Ek kaynaklar:

- [ASP.NET Data Access içerik haritası](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Entity Framework Geliştirici Merkezi](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Önceki](using-web-api-with-entity-framework-part-6.md)
