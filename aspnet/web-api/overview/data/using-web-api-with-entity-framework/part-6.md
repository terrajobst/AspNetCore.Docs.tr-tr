---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: JavaScript istemcisini oluşturma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 4967e21190c34f698e9c28fd9b921f07bef2ffaf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755535"
---
<a name="create-the-javascript-client"></a>JavaScript istemcisini oluşturma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](https://github.com/MikeWasson/BookService)

Bu bölümde, HTML, JavaScript kullanarak istemci uygulaması oluşturacak ve [Knockout.js](http://knockoutjs.com/) kitaplığı. Aşamalar halinde istemci uygulaması oluşturacağız:

- Kitap listesi gösteriliyor.
- Bir kitap ayrıntı gösteriliyor.
- Yeni bir kitap ekleniyor.

Knockout kitaplığı, Model-View-ViewModel (MVVM) desenini kullanır:

- **Modeli** iş etki alanında (bizim çalışması, kitaplar ve yazarlar) verileri sunucu tarafı gösterimidir.
- **Görünümü** sunu katmanı (HTML).
- **Görünüm modeli** modelleri tutan bir JavaScript nesnesi. Görünüm modeli, kullanıcı arabiriminin bir kod soyutlamadır. HTML gösteriminin bilgisi var. Bunun yerine, görünümün soyut özellikler gibi temsil ettiği &quot;kitap listesi&quot;.

Görünüm veri görünüm modeline bağlı. Görünüm modeli güncelleştirmeler Görünümü'nde otomatik olarak yansıtılır. Düğmesine tıklar gibi görünüm modeli olayları da görünümden alır.

![](part-6/_static/image1.png)

Herhangi bir kodu yeniden yazma olmadan bağlamaları değiştirebilirsiniz çünkü bu yaklaşım, uygulamanızın kullanıcı Arabirimi ve düzeni değiştirmek kolaylaştırır. Örneğin, bir liste öğesi gösterebilir bir `<ul>`, daha sonra tabloya değiştirebilirsiniz.

## <a name="add-the-knockout-library"></a>Knockout kitaplığı Ekle

Visual Studio'da gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**. Ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:

[!code-console[Main](part-6/samples/sample1.cmd)]

Bu komut, Knockout dosyaları Scripts klasörü olarak ekler.

## <a name="create-the-view-model"></a>Görünüm modeli oluşturun

Betikler klasörüne App.js adlı bir JavaScript dosyası ekleyin. (Çözüm Gezgini'nde betikler klasörüne sağ tıklayın, **Ekle**, ardından **JavaScript dosyası**.) Aşağıdaki kodu yapıştırın:

[!code-javascript[Main](part-6/samples/sample2.js)]

Knockout içinde `observable` sınıfı, veri bağlama sağlar. Observable içeriğini değiştirdiğinizde, kendisini güncelleştirmek için gözlemlenebilir tüm verilere bağlı denetimleri bildirir. ( `observableArray` Sınıfı, dizisi sürümü *observable*.) İle başlamak görünümü modelimizi, iki gözlemlenenler sahiptir:

- `books` kitap listesi içerir.
- `error` AJAX çağrısı başarısız olursa bir hata iletisi içerir.

`getAllBooks` Yöntemi books listesini almak için bir AJAX çağrısı yapar. Sonucu üzerine gönderim sonra `books` dizisi.

`ko.applyBindings` Yöntemi Knockout Kitaplığı'nın bir parçasıdır. Bu görünüm modeli parametre olarak alır ve veri bağlamasını ayarlamak ayarlar.

## <a name="add-a-script-bundle"></a>Betik paketi ekleme

Paketleme, birleştirme veya birden çok dosyayı tek bir dosyada paket kolaylaştırır ASP.NET 4.5 özelliğidir. Paketleme sayfa yükleme süresi iyileştirebilen sunucuya istek sayısını azaltır.

Uygulama dosyasını açın\_Start/BundleConfig.cs. RegisterBundles yöntemine aşağıdaki kodu ekleyin.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Önceki](part-5.md)
> [İleri](part-7.md)
