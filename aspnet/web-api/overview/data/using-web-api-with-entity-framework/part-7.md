---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Görünümü (UI) oluşturma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: e9ebe60f88ecbf65a6f8d04de9a23d72a72fda83
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364987"
---
<a name="create-the-view-ui"></a>Görünümü (UI) oluşturma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](https://github.com/MikeWasson/BookService)

Bu bölümde, uygulama için HTML tanımlayın ve HTML ve görünüm modeli arasında veri bağlaması Ekle başlar.

Views/Home/Index.cshtml dosyasını açın. Bu dosyanın tüm içeriğini aşağıdakiyle değiştirin.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

Çoğu `div` öğeleri bulunduğundan için [önyükleme](http://getbootstrap.com/) stil oluşturma. En önemli öğeler `data-bind` öznitelikleri. Bu öznitelik HTML görünümü modeline bağlar.

Örneğin:

[!code-html[Main](part-7/samples/sample2.html)]

Bu örnekte, &quot; `text` &quot; nedenler bağlamayı `<p>` değerini gösterilecek öğe `error` özelliğinden görünüm modeli. Bu geri çağırma `error` olarak bildirilen bir `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Her yeni bir değer atanır `error`, Knockout güncelleştirmeleri metinde `<p>` öğesi.

`foreach` Bağlama içeriğini döngü Knockout söyler `books` dizisi. Knockout dizideki her öğe için yeni bir oluşturur &lt;li&gt; öğesi. Bağlama bağlamı içinde `foreach` dizideki özelliklere bakın. Örneğin:

[!code-html[Main](part-7/samples/sample4.html)]

Burada `text` bağlama her kitabın Yazar özelliği okur.

Uygulamayı şimdi çalıştırırsanız, şu şekilde görünmelidir:

![](part-7/_static/image1.png)

Kitap listesi, Sayfa yüklendikten sonra zaman uyumsuz olarak yükler. Şu anda &quot;ayrıntıları&quot; Bağlantılar işlevsel değildir. Sonraki bölümde bu işlevselliğini ekleyeceğiz.

> [!div class="step-by-step"]
> [Önceki](part-6.md)
> [İleri](part-8.md)
