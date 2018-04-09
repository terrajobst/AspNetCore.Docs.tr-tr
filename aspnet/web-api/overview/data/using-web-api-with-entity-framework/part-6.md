---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: JavaScript istemci oluşturma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 29d50e448e6d282c7db06b9d1946ac221347e1ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="create-the-javascript-client"></a>JavaScript istemci oluşturma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](https://github.com/MikeWasson/BookService)

Bu bölümde, HTML, JavaScript kullanılarak istemci uygulaması oluşturacak ve [Knockout.js](http://knockoutjs.com/) kitaplığı. Biz aşamada istemci uygulaması oluşturma:

- Kitap listesi gösteriliyor.
- Defteri ayrıntısı gösteriliyor.
- Yeni bir rehberi ekleniyor.

Boşaltılan kitaplığı Model View ViewModel (MVVM) deseni kullanır:

- **Modeli** iş etki alanında (bizim çalışması, books ve yazarlar) verileri sunucu tarafı gösterimidir.
- **Görünüm** sunu (HTML) katmanıdır.
- **Görünüm modeli** modelleri tutan bir JavaScript nesnesidir. Görünüm modeli UI kod soyutlamadır. HTML temsilini olanağıyla sahiptir. Bunun yerine, görünümün soyut özellikler gibi temsil ettiği &quot;books listesini&quot;.

Görünüm verilere görünüm modeline bağlı. Görünüm modeli güncelleştirmeleri otomatik olarak görünümünde yansıtılır. Düğmesine tıklar gibi görünüm modeli olayları da görünümden alır.

![](part-6/_static/image1.png)

Herhangi bir kod yeniden yazma işlemi olmadan bağlamaları değişebileceği için bu yaklaşım, uygulamanızın kullanıcı Arabirimi ve düzenini değiştirmek kolaylaştırır. Örneğin, bir öğe listesi gösterebilir. bir `<ul>`, tabloya daha sonra değiştirin.

## <a name="add-the-knockout-library"></a>Boşaltılan kitaplığı Ekle

Visual Studio'da gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**. Ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:

[!code-console[Main](part-6/samples/sample1.cmd)]

Bu komut Boşaltılan dosyaları betikler klasörüne ekler.

## <a name="create-the-view-model"></a>Görünüm modeli oluşturma

Betikler klasörüne App.js adlı bir JavaScript dosyası ekleyin. (Çözüm Gezgini'nde betikler klasörüne sağ tıklayın, **Ekle**seçeneğini belirleyip **JavaScript dosyası**.) Aşağıdaki kodu yapıştırın:

[!code-javascript[Main](part-6/samples/sample2.js)]

Boşaltılan içinde `observable` veri bağlama sınıfı sağlar. Observable içeriğini değiştirdiğinizde, kendilerini güncelleştirecek şekilde observable tüm verilere bağlı denetimler bildirir. ( `observableArray` Sınıftır dizi sürümü *observable*.) İle başlamak iki tane observable bizim görünüm modeli vardır:

- `books` books listesini tutar.
- `error` AJAX çağrısı başarısız olursa bir hata iletisi içerir.

`getAllBooks` Yöntemi books listesini almak için bir AJAX çağrısı yapar. Sonucun üzerine iter sonra `books` dizi.

`ko.applyBindings` Yöntemi Boşaltılan kitaplığı bir parçasıdır. Görünüm modeli parametre olarak alır ve veri bağlamanın ayarlar.

## <a name="add-a-script-bundle"></a>Bir komut dosyası paket ekleme

Paketleme, birleştirebilir veya tek bir dosyaya birden çok dosya paketini daha kolay hale getirir ASP.NET 4.5 özelliğidir. Paketleme sayfa yükleme süresini artırabilir sunucu isteklerinin sayısını azaltır.

Uygulama dosyasını açın\_Start/BundleConfig.cs. RegisterBundles yöntemine aşağıdaki kodu ekleyin.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Önceki](part-5.md)
> [sonraki](part-7.md)
