---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: '5. Kısım: dinamik bir kullanıcı Arabirimi ile Knockout.js oluşturma | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: b63446d076fbb1143641dead788042967b996bf8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873816"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>5. Kısım: dinamik kullanıcı arabirimini Knockout.js ile oluşturma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Knockout.js ile dinamik kullanıcı Arabirimi oluşturma

Bu bölümde, yönetici görünümüne işlevsellik eklemek için Knockout.js kullanacağız.

[Knockout.js](http://knockoutjs.com/) , HTML denetimlerini verilere bağlama kolaylaştırır bir Javascript kitaplığı. Knockout.js Model View ViewModel (MVVM) desen kullanır.

- *Modeli* iş etki alanında (servis talebi, ürünlerimizi ve siparişleri) verileri sunucu tarafı gösterimidir.
- *Görünüm* sunu (HTML) katmanıdır.
- *Görünüm modeli* modeli verilerini tutan bir Javascript nesnesidir. Görünüm modeli UI kod soyutlamadır. HTML temsilini olanağıyla sahiptir. Bunun yerine, "bir öğe listesi" gibi görünümün soyut özellikleri temsil eder.

Görünüm veri görünüm modeline bağlı. Görünüm modeli güncelleştirmeleri otomatik olarak görünümünde yansıtılır. Görünüm modeli de görünümünden düğme tıklamaları gibi olayları alır ve bir sıra oluşturma gibi bir modeli işlemleri gerçekleştirir.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Öncelikle biz görünüm modeli tanımlamanız. Bundan sonra biz HTML biçimlendirmesi görünüm modeline bağlarsınız.

Aşağıdaki Razor bölümü için Admin.cshtml ekleyin:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

Bu bölümde dosyasında herhangi bir yere ekleyebilirsiniz. Görünüm işlendiğinde bölümü HTML sayfasının en altında görünür kapatmadan önce sağ &lt;/body&gt; etiketi.

Bu sayfa için komut tümünün açıklamaya göre belirtilen komut dosyası etiketinin içine geçer:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

İlk olarak, bir görünüm model sınıfı tanımlayın:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** özel türde bir adlı Boşaltılan nesnesinde bir *observable*. Gelen [Knockout.js belgelerine](http://knockoutjs.com/documentation/observables.html): observable "aboneleri değişiklikler hakkında bilgilendirebilir bir JavaScript." nesnedir Observable içeriğini değiştirdiğinizde, görünümü eşleşecek şekilde otomatik olarak güncelleştirilir.

Doldurmak için `products` dizisindeki, web API AJAX isteği olun. Biz API görünüm paketi taban URI depolanan geri çağırma (bkz [bölümü 4](using-web-api-with-entity-framework-part-4.md) öğreticinin).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Ardından, İşlevler görünümü-oluşturma, güncelleştirme ve silme ürünleri için modeline ekleyin. Bu işlevler AJAX çağrıları Web API'ye gönderin ve sonuçları görünüm modeli güncelleştirmek için kullanın.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Şimdi en önemli bölümü: zaman DOM olduğu fulled yüklenen, çağrı **ko.applyBindings** işlev ve yeni bir örneğini içinde geçirmek `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

**Ko.applyBindings** yöntemi Boşaltılan etkinleştirir ve görünümüne bağlayan görünüm modeli.

Görünüm modeli sahibiz, bağlamaları oluşturabilir. Knockout.js içinde ekleyerek bunu `data-bind` öznitelikleri HTML öğeleri. Örneğin, bir dizi için bir HTML liste bağlamak için kullanın `foreach` bağlama:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

`foreach` Bağlama dizisini yineler ve alt dizideki her nesne için öğeleri oluşturur. Alt öğeler üzerinde bağlamaları dizi nesnelerdeki özellikleri başvurabilirsiniz.

Aşağıdaki bağlamaları "Güncelleştirme ürünleri" listesine ekleyin:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

`<li>` Öğesi oluşur kapsamında **foreach** bağlama. Anlamına gelir Boşaltılan her ürün için bir kez öğe işleme `products` dizi. Tüm bağlamaları içinde `<li>` öğesi bu ürün örneğine bakın. Örneğin, `$data.Name` başvurduğu `Name` ürün özelliği.

Metin girdi değerleri ayarlamak için kullanma `value` bağlama. Düğmeleri model-view üzerinde işlevlere bağlı kullanarak `click` bağlama. Ürün örneği her işlevi için parametre olarak geçirilir. Daha fazla bilgi için [Knockout.js belgelerine](http://knockoutjs.com/documentation/observables.html) çeşitli bağlamaları iyi açıklamaları vardır.

Ardından, için bir bağlama eklemek **gönderme** Ürün Ekle form üzerinde olay:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Bu bağlama çağrıları `create` yeni ürün oluşturmak için Görünüm modeli üzerinde işlevi.

Yönetim görünümü için tam kod aşağıdaki gibidir:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Uygulamayı çalıştırın, yönetici hesabıyla oturum açın ve "Yönetici" bağlantısını tıklatın. Ürünlerinin listesini görmek ve oluşturma, güncelleştirme veya silme ürünleri kurabilmesi gerekir.

> [!div class="step-by-step"]
> [Önceki](using-web-api-with-entity-framework-part-4.md)
> [sonraki](using-web-api-with-entity-framework-part-6.md)
