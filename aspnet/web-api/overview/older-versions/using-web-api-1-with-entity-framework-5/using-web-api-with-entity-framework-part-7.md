---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: "7. Kısım: ana oluşturma sayfası | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: a17b9f26ec48b5410211d6dad6e4deec971642d7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="part-7-creating-the-main-page"></a>7. Kısım: ana oluşturma sayfası
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Ana oluşturma sayfası

Bu bölümde, ana uygulama sayfası oluşturur. Biz, birkaç adımda yaklaşımını böylece bu sayfayı yönetici sayfadan daha karmaşık olacaktır. Yol boyunca daha gelişmiş bazı Knockout.js teknikleri görürsünüz. Sayfa temel düzenini şöyledir:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Ürünler" ürünleri dizisi içerir.
- "Sepeti" miktarları ürünleriyle dizisi içerir. "Sepete Ekle" tıklatarak Sepeti güncelleştirir.
- "Siparişler" sipariş kimlikleri bir dizi tutar.
- Bir dizi öğelerinin (miktarları ürünleriyle) bir Sipariş Ayrıntısı "Ayrıntılar" tutar

Veri bağlama ya da komut dosyası HTML'de, bazı temel düzeni tanımlayarak başlayacağız. Views/Home/Index.cshtml dosyasını açın ve tüm içeriğini aşağıdakiyle değiştirin:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Ardından, komut dosyaları Bölüm Ekle ve boş bir görünüm model oluşturun:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Daha önce ince ince tasarımı bizim görünüm modeli gözlemlenenler ürünler, Sepeti, siparişler ve Ayrıntılar için gerekiyor. Aşağıdaki değişkenleri eklemek `AppViewModel` nesnesi:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Kullanıcıların Sepeti ürünleri listesinden öğe eklemek ve Alışveriş sepetinden öğeleri kaldırın. Bu işlevler kapsüllemek için bir ürün temsil eden başka bir görünüm modeli sınıf oluşturacağız. Aşağıdaki kodu ekleyin `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

`ProductViewModel` Sınıfı Sepeti gelen ve giden ürün taşımak için kullanılan iki işlevler içerir: `addItemToCart` sepete ürün bir birimi ekler ve `removeAllFromCart` ürün tüm miktarlarını kaldırır.

Kullanıcılar, var olan bir sırayı seçin ve Sipariş ayrıntılarını alır. Biz bu işlevi başka bir görünüm modelini kapsülleyeceğiz:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

`OrderDetailsViewModel` Bir sıra ile başlatıldı ve sunucuya bir AJAX isteği göndererek sipariş ayrıntılarını getirir.

Ayrıca, fark `total` özelliği `OrderDetailsViewModel`. Bu özellik observable adlı özel bir tür olan bir [observable hesaplanan](http://knockoutjs.com/documentation/computedObservables.html). Hesaplanan observable adından da anlaşılacağı gibi hesaplanan değeri &#8212;veri bağlama sağlar; bu durumda, sırasını toplam maliyeti.

Ardından, bu işlevler eklemek `AppViewModel`:

- `resetCart`Alışveriş sepetinden tüm öğeleri kaldırır.
- `getDetails`Ayrıntılar için bir sıra alır (yeni bir pusing tarafından `OrderDetailsViewModel` üzerine `details` listesi).
- `createOrder`Yeni bir sipariş oluşturur ve sepeti boşaltır.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Son olarak, ürün ve siparişleri AJAX istekleri yaparak görünüm modeli başlatın:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

Tamam, çok fazla kod olan, ancak bunu oluşturduğumuz yukarı adım adım, bu nedenle umarız tasarım işaretlenmemiştir. Şimdi biz HTML bazı Knockout.js bağlamaları ekleyebilirsiniz.

**Ürünler**

Ürün listesi için olan bağlamaları şunlardır:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Bu ürünler dizi tekrarlanan ve fiyat ve adını görüntüler. Yalnızca kullanıcı oturum açtığında "Sırada Ekle" düğmesi görünür olur.

"Sırada Ekle" düğmesi çağrıları `addItemToCart` üzerinde `ProductViewModel` ürün için örneği. Bu iyi bir özelliği olan Knockout.js gösterir: görünüm modeli diğer görünüm modelleri içerdiğinde, iç model bağlamaları uygulayabilirsiniz. Bu örnekte, içinde bağlamaları `foreach` her biri için uygulanan `ProductViewModel` örnekleri. Bu yaklaşım tüm işlevleri tek bir görünüm-modeline koyma daha çok temizleyici olur.

**Sepeti**

Alışveriş için olan bağlamaları şunlardır:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Bu Sepeti dizi tekrarlanan ve adı, fiyat ve miktar görüntüler. Bağlantıyı "Kaldır" ve "Sipariş oluştur" düğmesine görünüm modeli işlevlere bağlı unutmayın.

**Siparişleri**

Siparişleri listesi için olan bağlamaları şunlardır:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Bu sipariş tekrarlanan ve sipariş kimliği gösterir Bağlantıyı click olayını bağlı `getDetails` işlevi.

**Sipariş ayrıntılarını**

Sipariş ayrıntılarını bağlantılarında şunlardır:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Bu sırada öğeler üzerinde yinelenir ve ürün, fiyat ve miktarı görüntüler. Çevresindeki div yalnızca ayrıntıları dizi bir veya daha fazla öğe içeriyorsa görünür olur.

## <a name="conclusion"></a>Sonuç

Bu öğreticide, veritabanı ve veri katmanı üzerinde genel kullanıma yönelik arabirimi sağlamak için ASP.NET Web API ile iletişim kurmak için Entity Framework kullanan bir uygulama oluşturdunuz. HTML sayfaları ve Knockout.js ve jQuery sayfa yeniden yükler olmadan dinamik etkileşimlerini sağlamak üzere işlemek için ASP.NET MVC 4 kullanırız.

Ek kaynaklar:

- [ASP.NET Data Access içerik haritası](https://msdn.microsoft.com/en-us/library/6759sth4.aspx)
- [Entity Framework Geliştirici Merkezi](https://msdn.microsoft.com/en-US/data/ef)

>[!div class="step-by-step"]
[Önceki](using-web-api-with-entity-framework-part-6.md)
