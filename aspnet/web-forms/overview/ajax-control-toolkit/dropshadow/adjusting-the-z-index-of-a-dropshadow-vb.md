---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: DropShadow (VB) Z-Index ayarlama | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Ancak bu gölge bazen yükleme konumu için diğer denetimleri çakışıyor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: b484dc6bfa6f67bd6b70f7c36c2eb2ec7143edaf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a>DropShadow (VB) Z-Index ayarlama
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)

> AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Ancak bu gölge, diğer denetimler örneği için ASP.NET menü denetimi çakışıyor. Ne zaman bir menü girişi, gölge göründüğü açılır.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Ancak bu gölge, diğer denetimler örneği için ASP.NET menü denetimi çakışıyor. Ne zaman bir menü girişi, gölge göründüğü açılır.

## <a name="steps"></a>Adımlar

Bölmenin görünür olmasını etkisi için yeterince metin içeren yeterli metni içeren paneli kendisi ile kod gönderir:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

Başka bir panel hemen öncesine yerleştirilir `panelShadow` paneli. Üzerinden menü girişlerini görünmesini sağlayacak şekilde yatay yönlendirme menüsüyle içerir (veya bunun yerine: altında) `dropShadow` Masası):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

Ardından, `DropShadowExtender` genişletmek için eklenen `panelShadow` gölge efekti paneliyle:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

Son olarak, ASP.NET AJAX `ScriptManager` denetimi çalışmak Denetim Araç Seti sağlar:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

Bu komut dosyasını çalıştırdığınızda menü girişlerini paneli altında görünür. Ancak menü CSS sınıfı kullanır `panel` yalnızca olduğu öğelerin önünde diğer paneli görünür yapmak için iki şey tanımlamak:

- Göreli konumlandırma
- Pozitif z-index

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

Ardından, `DropShadowExtender` denetim çakışıp artık menü denetimi ile.


[![Önce: Menü girişi görünür değil](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)

Önce: Menü girişi görünür değil ([tam boyutlu görüntüyü görüntülemek için tıklatın](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))


[![Sonra: Menü giriş görüntülenir.](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)

Sonra: Menü girişi görünür ([tam boyutlu görüntüyü görüntülemek için tıklatın](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Önceki](manipulating-dropshadow-properties-from-client-code-cs.md)
> [sonraki](manipulating-dropshadow-properties-from-client-code-vb.md)
