---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: İstemci kodu (C#) DropShadow özelliklerinden düzenleme | Microsoft Docs
author: wenz
description: DataList'ın düzenleme arabirimi özelleştirme
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 37a7784e1d42477e31938e1d15495993ac86fc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870342"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a>İstemci kodu (C#) DropShadow özelliklerini düzenleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Bu genişletici özelliklerini kullanarak istemci JavaScript kodu da değiştirilebilir.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Bu genişletici özelliklerini kullanarak istemci JavaScript kodu da değiştirilebilir.

## <a name="steps"></a>Adımlar

Kod bazı satırlık metin içeren bir panel ile başlar:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

İlişkili CSS sınıf Masası iyi arka plan rengi sağlar:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

`DropShadowExtender` Bir gölge efekti, % 50 olarak belirlenmiş opaklık paneliyle genişletmek için eklendi:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

Ardından, ASP.NET AJAX `ScriptManager` denetimi çalışmak Denetim Araç Seti sağlar:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Başka bir panel gölge geçirgenliğini ayarlamak için iki JavaScript bağlantılar içerir: eksi bağlantı gölgenin opaklık azaltır, artı bağlantı da artırır.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

JavaScript işlevinin `changeOpacity()` sonra ilk bulmalıdır `DropShadowExtender` sayfasında denetimi. ASP.NET AJAX tanımlar `$find()` tam olarak bu görev için yöntem. Ardından, `get_Opacity()` yöntemi alır Geçerli Opaklık `set_Opacity()` yöntemini ayarlar. JavaScript kodu ardından geçerli geçirgenlik değeri geçirir `<label>` öğe:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


[![Opaklık istemci tarafında değiştirilir](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

Opaklık istemci tarafında değiştirilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [sonraki](adjusting-the-z-index-of-a-dropshadow-vb.md)
