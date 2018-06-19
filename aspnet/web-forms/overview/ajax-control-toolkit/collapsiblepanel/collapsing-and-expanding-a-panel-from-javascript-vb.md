---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: JavaScript (VB) panelinden genişletme ve daraltma | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti CollapsiblePanel denetiminde bir panel genişletir ve içeriğini daraltmak ve genişletmek yeteneği sağlar bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 5cf61cd0d8204a5405ba62cd3884d66ccb21968b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873124"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a>JavaScript (VB) panelinden genişletme ve daraltma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)

> ASP.NET AJAX Denetim Araç Seti CollapsiblePanel denetiminde bir panel genişletir ve içeriğini daraltın ve tekrar genişletme olanağı sağlar. Bu iki eylem ayrıca özel JavaScript kodundan tetiklenebilir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti CollapsiblePanel denetiminde bir panel genişletir ve içeriğini daraltın ve tekrar genişletme olanağı sağlar. Bu iki eylem ayrıca özel JavaScript kodundan tetiklenebilir.

## <a name="steps"></a>Adımlar

İlk olarak, yeni bir ASP.NET sayfası oluşturmak ve dahil etmek `ScriptManager` bir içinde `<form>` öğesi. Bu Denetim Araç Seti tarafından gerekli olan ASP.NET AJAX kitaplığı yükler:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

Ardından, bir panel bazı metinle oluşturun, böylece daraltma ve genişletme etkisi görülebilir:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

Gördüğünüz gibi bölmenin burada gösterilen (ve temel bir arka plan rengi ve bölmenin genişliğini tanımlar) bir CSS sınıfı başvuruyor:

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

`CollapsiblePanelExtender` Denetimi gerektirir `TargetControlID` daraltın veya istek üzerine genişletmek için hangi Masası Araç Seti bilir böylece özniteliği:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

Ne yazık ki, genişletici şu anda belirli bir API daraltma veya paneli genişletme için kullanıma sunmuyor, ancak bazı belgelenmemiş yöntemleri yapar. Öncelikle, üç HTML düğmesi, ardından daraltmak veya bölmenin içeriği genişletmek için istemci tarafı JavaScript tetikleyecek sayfasına ekleyin:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

İstemci tarafı JavaScript kodu (kullanmaya `<script type="text/javascript">`), `$find()` yönteminin gerekir kullanılacak erişimi `CollapsiblePanelExtender`. `$find("cpe")` bir başvuru döndürür. Üzerinde buradan belirli yöntemler elinizdeki çözüm.

Yöntemi (genişletme) açmak için bölmenin adlı `_doOpen()`; aşağıdaki kod uygulayan `doOpen()` işlevini çağırdı ilk düğme tıklatıldığında:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

Kapatma veya paneli daraltma için `_doClose()` yöntemi yürütülmesi gerekiyor. Bu nedenle kullanıcı ikinci düğmesine tıkladığında, şu JavaScript kodunu çağrılır:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

Üçüncü düğme paneli durumunu değiştirir: gelen için genişletilmiş, daraltılmış ve tersi. `CollapsiblePanelExtender` Sunan `toggle()` tam olarak, yapan yöntemi: paneli durumunu tersine çevirir. Ancak bulunmaktadır başka bir yaklaşım (dahili olarak kullanılan tarafından `toggle()` yöntemi): `get_Collapsed()` yöntemi `CollapsiblePanelExtender()` bize panel daraltılmış durumda değil söyler. Ya da Genişletilmiş sonra bu işlevin dönüş değerini bağlı olarak, panosudur (`_doOpen()` yöntemi) veya daraltılmış (`_doClose()`) yöntemi:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


[![Üçüncü düğme paneli durumunu değiştirir: gelen genişletilmiş ve geri daraltılmış](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)

Üçüncü düğme paneli durumunu değiştirir: gelen genişletilmiş ve geri daraltılmış ([tam boyutlu görüntüyü görüntülemek için tıklatın](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](collapsing-and-expanding-a-panel-from-javascript-cs.md)
