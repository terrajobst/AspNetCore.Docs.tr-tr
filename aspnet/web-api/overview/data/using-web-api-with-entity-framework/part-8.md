---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Öğe ayrıntıları görüntülemek | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 94863e94f2a8b3f1ce8a8fb85d877bc0768f3d8a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868093"
---
<a name="display-item-details"></a>Görüntü öğe ayrıntıları
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](https://github.com/MikeWasson/BookService)

Bu bölümde, her kitap ayrıntılarını görüntülemek için özelliğini ekleyecektir. App.js içinde görünüm modeli aşağıdaki kodu ekleyin:

[!code-javascript[Main](part-8/samples/sample1.js)]

Views/Home/Index.cshtml içinde veri bağlama öğesi ayrıntıları bağlantısını ekleyin:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Bu click işleyicisi bağlar &lt;bir&gt; öğesine `getBookDetail` görünüm modeli üzerinde işlevi.

Aynı dosyada aşağıdaki işaretleme değiştirin:

[!code-html[Main](part-8/samples/sample3.html)]

şununla değiştirin:

[!code-html[Main](part-8/samples/sample4.html)]

Bu biçimlendirme verilere özelliklerine bağlı bir tablo oluşturur `detail` observable görünüm modeli.

"&lt;!--Ko--&gt; &quot; sözdizimi Boşaltılan bağlamasını DOM öğesi dışında dahil olanak sağlar. Bu durumda, `if` bağlama neden olur, bu bölümde görüntülenecek biçimlendirme yalnızca `details` null olmayan bir değer.

[!code-html[Main](part-8/samples/sample5.html)]

Uygulamayı çalıştırın ve birine tıklayın, şimdi &quot;ayrıntı&quot; bağlantılar, uygulama rehberi ayrıntılarını görüntüler.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Önceki](part-7.md)
> [sonraki](part-9.md)
