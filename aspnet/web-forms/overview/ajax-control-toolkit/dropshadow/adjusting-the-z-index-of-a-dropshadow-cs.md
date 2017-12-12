---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: DropShadow (C#) Z-Index ayarlama | Microsoft Docs
author: wenz
description: "AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Ancak bu gölge bazen yükleme konumu için diğer denetimleri çakışıyor..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 161f02daa5dd1f0e21853c1b7c1a65c1a9aa5d03
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a>DropShadow (C#) Z-Index ayarlama
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Ancak bu gölge, diğer denetimler örneği için ASP.NET menü denetimi çakışıyor. Ne zaman bir menü girişi, gölge göründüğü açılır.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Ancak bu gölge, diğer denetimler örneği için ASP.NET menü denetimi çakışıyor. Ne zaman bir menü girişi, gölge göründüğü açılır.

## <a name="steps"></a>Adımlar

Bölmenin görünür olmasını etkisi için yeterince metin içeren yeterli metni içeren paneli kendisi ile kod gönderir:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Başka bir panel hemen öncesine yerleştirilir `panelShadow` paneli. Üzerinden menü girişlerini görünmesini sağlayacak şekilde yatay yönlendirme menüsüyle içerir (veya bunun yerine: altında) `dropShadow` Masası):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

Ardından, `DropShadowExtender` genişletmek için eklenen `panelShadow` gölge efekti paneliyle:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Son olarak, ASP.NET AJAX `ScriptManager` denetimi çalışmak Denetim Araç Seti sağlar:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

Bu komut dosyasını çalıştırdığınızda menü girişlerini paneli altında görünür. Ancak menü CSS sınıfı kullanır `panel` yalnızca olduğu öğelerin önünde diğer paneli görünür yapmak için iki şey tanımlamak:

- Göreli konumlandırma
- Pozitif z-index

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

Ardından, `DropShadowExtender` denetim çakışıp artık menü denetimi ile.


[![Önce: Menü girişi görünür değil](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Önce: Menü girişi görünür değil ([tam boyutlu görüntüyü görüntülemek için tıklatın](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))


[![Sonra: Menü giriş görüntülenir.](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

Sonra: Menü girişi görünür ([tam boyutlu görüntüyü görüntülemek için tıklatın](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

>[!div class="step-by-step"]
[Sonraki](manipulating-dropshadow-properties-from-client-code-cs.md)
