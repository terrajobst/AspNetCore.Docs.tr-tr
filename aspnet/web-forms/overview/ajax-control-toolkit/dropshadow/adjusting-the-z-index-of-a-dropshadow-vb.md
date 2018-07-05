---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: (VB) bir dropshadow'un Z dizinini | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Ancak bu gölge bazen diğer denetimlerle yükleme konumu için çakışan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: afc51c6e52a08f46ffc44cc462bdf9a9d5c8ef43
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397548"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a>(VB) bir dropshadow'un Z dizinini ayarlama
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)

> AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Ancak bu gölge, örneğin ASP.NET menü denetimi diğer denetimlerle çakışıyor. Ne zaman menüsü girişi, gölge görünür açılır.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Ancak bu gölge, örneğin ASP.NET menü denetimi diğer denetimlerle çakışıyor. Ne zaman menüsü girişi, gölge görünür açılır.

## <a name="steps"></a>Adımlar

Kod paneli görünür olmasını etkisi için yeterli metni içeren yeterli metin içeren Panel'i kendisi ile başlar:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

Başka bir panel hemen öncesine yerleştirilir `panelShadow` paneli. Yatay yönlendirme sahip bir menüyü menü girişlerini üzerinde görünmesini sağlayacak şekilde içerdiği (veya bunun yerine: altında) `dropShadow` paneli):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

Ardından, `DropShadowExtender` genişletmek için eklenen `panelShadow` panel gölge etkisi ile:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

Son olarak, ASP.NET AJAX `ScriptManager` denetimi çalışmak Denetim Araç Seti sağlar:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

Bu komut dosyasını çalıştırdığınızda, menü girişlerini bölmenin altında görünür. Ancak menü CSS sınıfı kullanır `panel` yalnızca sahip olduğunuz öğeleri diğer panel önünde görünür yapmak için iki şey tanımlamak:

- Göreli konum
- Bir pozitif z dizinini ayarlama

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

Ardından, `DropShadowExtender` denetimi değil çakışan artık ile menü denetimi.


[![Önce: Menüsü girişi görünür değil](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)

Önce: Menüsü girişi görünür değil ([tam boyutlu görüntüyü görmek için tıklatın](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))


[![Sonra: Menü giriş görüntülenir.](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)

Sonra: Menüsü girişi görünür ([tam boyutlu görüntüyü görmek için tıklatın](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Önceki](manipulating-dropshadow-properties-from-client-code-cs.md)
> [İleri](manipulating-dropshadow-properties-from-client-code-vb.md)
