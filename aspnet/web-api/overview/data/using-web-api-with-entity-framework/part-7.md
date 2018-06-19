---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Görünüm (UI) oluşturma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 5052d7cca4a5c12a9ea56eb929d4794b19e82603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878808"
---
<a name="create-the-view-ui"></a>Görünüm (UI) oluşturun
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](https://github.com/MikeWasson/BookService)

Bu bölümde, uygulama için HTML tanımlamak ve HTML ve görünüm modeli arasında veri bağlama eklemek başlar.

Views/Home/Index.cshtml dosyasını açın. Bu dosyanın tüm içeriğini aşağıdakiyle değiştirin.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

Çoğu `div` öğeleri bulunduğundan için [önyükleme](http://getbootstrap.com/) stil oluşturma. Raporlarla önemli öğeleridir `data-bind` öznitelikleri. Bu öznitelik HTML görünüm modeli bağlar.

Örneğin:

[!code-html[Main](part-7/samples/sample2.html)]

Bu örnekte, &quot; `text` &quot; nedenler bağlama `<p>` değerini göstermek için öğesi `error` görünüm modeli özelliğinden. Sözcüğünün `error` olarak bildirilen bir `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Her yeni bir değer atanması `error`, çakıştırma güncelleştirmeleri metinde `<p>` öğesi.

`foreach` Bağlama içeriğini döngü Boşaltılan söyler `books` dizi. Dizideki her öğe için çakıştırma yeni bir oluşturur &lt;li&gt; öğesi. Bağlamaları bağlamı içinde `foreach` dizi öğenin özelliklerini bakın. Örneğin:

[!code-html[Main](part-7/samples/sample4.html)]

Burada `text` bağlama her rehberi Yazar özelliği okur.

Uygulamayı şimdi çalıştırırsanız, aşağıdaki gibi görünmelidir:

![](part-7/_static/image1.png)

Sayfa yüklendikten sonra defterleri listesi zaman uyumsuz olarak yükler. Şimdi &quot;ayrıntıları&quot; Bağlantılar işlevsel değildir. Bu işlevsellik sonraki bölümde ekleyeceğiz.

> [!div class="step-by-step"]
> [Önceki](part-6.md)
> [sonraki](part-8.md)
