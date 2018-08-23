---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: '5. Bölüm: Knockout.js ile dinamik kullanıcı Arabirimi oluşturma | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: d019941700992e173a5812b11b558b6726dfd809
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752342"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>5. Bölüm: Knockout.js ile dinamik kullanıcı Arabirimi oluşturma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Knockout.js ile dinamik kullanıcı Arabirimi oluşturma

Bu bölümde, yönetici görünümü işlevselliği eklemek için Knockout.js kullanacağız.

[Knockout.js](http://knockoutjs.com/) HTML denetimleri verilere bağlamak için kolaylaştıran bir Javascript kitaplığı. Knockout.js Model-View-ViewModel (MVVM) desenini kullanır.

- *Modeli* iş etki alanında (çalışması, Ürünlerimiz ve siparişler) verileri sunucu tarafı gösterimidir.
- *Görünümü* sunu katmanı (HTML).
- *Görünüm modeli* model verileri tutan bir Javascript nesnesi. Görünüm modeli, kullanıcı arabiriminin bir kod soyutlamadır. HTML gösteriminin bilgisi var. Bunun yerine, "öğe listesi" gibi görünümün soyut özellikler temsil eder.

Görünüm veri görünüm modeline bağlı. Görünüm modeli güncelleştirmeler Görünümü'nde otomatik olarak yansıtılır. Görünüm modeli görünümünden bir düğmeye tıklanması gibi olayları alır ve bir sipariş oluşturma gibi model üzerinde işlemleri gerçekleştirir.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

İlk Görünüm modeli tanımlarız. Bundan sonra biz HTML biçimlendirmesi için Görünüm modeli bağlayacaksınız.

Aşağıdaki Razor bölümü için Admin.cshtml ekleyin:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

Bu bölümde dosyasında herhangi bir yere ekleyebilirsiniz. Görünüm işlendiğinde bölümü HTML sayfasının en altında görünür kapatmadan önce sağ &lt;/body&gt; etiketi.

Tüm bu sayfa için betik açıklama tarafından belirtilen komut dosyası etiketi içinde geçer:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

İlk olarak, bir görünüm modeli sınıf tanımlayın:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** denir Knockout, nesnenin özel bir tür bir *observable*. Gelen [Knockout.js belgeleri](http://knockoutjs.com/documentation/observables.html): observable "aboneleri değişiklikleri bildiren bir JavaScript nesne" dir. Observable içeriğini değiştiğinde görünüm eşleşecek şekilde otomatik olarak güncelleştirilir.

Doldurmak için `products` dizi, web API'sine bir AJAX isteği yapın. Biz API görünüm paketi temel URI'sini depolanan geri çağırma (bkz [bölüm 4](using-web-api-with-entity-framework-part-4.md) öğreticinin).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Ardından, İşlevler görünümü-oluşturma, güncelleştirme ve silme ürünleri için eklersiniz. Bu işlevler, Web API AJAX çağrıları göndermek ve sonuçları görünüm modeli güncelleştirmek için kullanın.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Şimdi en önemli kısmıdır: zaman DOM olan fulled yüklenen, çağrı **ko.applyBindings** işlev ve yeni bir örneğini geçirin `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

**Ko.applyBindings** yöntemi Knockout etkinleştirir ve görünüme bağlayan görünüm modeli.

Görünüm modeli sahibiz, bağlamaları oluşturabiliriz. Knockout.js içinde ekleyerek bunu `data-bind` HTML öğeleri için öznitelikler. Örneğin, bir dizi için bir HTML liste bağlamak için kullanın `foreach` bağlama:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

`foreach` Bağlama dizi aracılığıyla yinelenir ve alt öğeleri her nesne için dizide oluşturur. Alt öğeleri üzerinde bağlamaları dizi nesnelerdeki özelliklerin başvurabilir.

Aşağıdaki bağlamaları "update-ürünleri" listesine ekleyin:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

`<li>` Öğesi oluşur kapsamında **foreach** bağlama. Knockout anlamına gelir her ürün için bir kez öğe işleme `products` dizisi. Tüm bağlamaları içinde `<li>` öğesi, bu ürün örneğe bakın. Örneğin, `$data.Name` başvurduğu `Name` ürün özelliği.

Metin girişleri değerlerini ayarlamak için kullanın `value` bağlama. Model-görünüm üzerinde düğmeleri işlevlere bağlı kullanarak `click` bağlama. Ürün örneği her işlev için parametre olarak geçirilir. Daha fazla bilgi için [Knockout.js belgeleri](http://knockoutjs.com/documentation/observables.html) çeşitli bağlamaları iyi açıklamaları vardır.

Ardından, bağlama için ekleme **gönderme** Ürün Ekle formdaki olay:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Bu bağlama çağrıları `create` işlevi yeni ürün oluşturmak için Görünüm modeli.

Yönetici görünümü için tam kod aşağıdaki gibidir:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Uygulamayı çalıştırın, yönetici hesabıyla oturum açın ve "Yönetici" bağlantısına tıklayın. Ürünlerinin listesini görmek ve oluşturmak, güncelleştirmek veya ürünleri silmek mümkün olmayacaktır.

> [!div class="step-by-step"]
> [Önceki](using-web-api-with-entity-framework-part-4.md)
> [İleri](using-web-api-with-entity-framework-part-6.md)
