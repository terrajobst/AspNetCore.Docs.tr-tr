---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: (C#) JavaScript'ten bir paneli daraltma ve genişletme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti CollapsiblePanel denetiminde bir panel genişletir ve içeriğini daraltmak ve genişletmek için olanağı sunan bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 85a9c6e6cc56cc563eefea6bc863950ba71149ba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402762"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>(C#) JavaScript'ten bir paneli daraltma ve genişletme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> ASP.NET AJAX Denetim Araç Seti CollapsiblePanel denetiminde bir panel genişletir ve içeriğini Daralt ve yeniden genişletin yeteneği sağlar. Bu iki eylem, özel bir JavaScript kodundan da tetiklenebilir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti CollapsiblePanel denetiminde bir panel genişletir ve içeriğini Daralt ve yeniden genişletin yeteneği sağlar. Bu iki eylem, özel bir JavaScript kodundan da tetiklenebilir.

## <a name="steps"></a>Adımlar

İlk olarak yeni bir ASP.NET sayfasında oluşturduğunuz ve `ScriptManager` bir içinde `<form>` öğesi. Bu Denetim Araç Seti tarafından gerekli olan ASP.NET AJAX kitaplığı yükler:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

Ardından, bir panel bazı metinleri olan oluşturun, böylece daraltma/genişletme etkisi görülebilir:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Gördüğünüz gibi burada gösterilen (ve temel bir arka plan rengi ve panelin genişliğini tanımlar) bir CSS sınıfı paneli başvuruyor:

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

`CollapsiblePanelExtender` Denetimi gerektirir `TargetControlID` Araç Seti Daralt veya istek üzerine genişletmek için hangi panel bilebilmesi özniteliği:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

Ne yazık ki, genişletici şu anda belirli bir API'yi daraltma veya genişletme panel için kullanıma sunmuyor, ancak bazı belgelenmemiş yöntemleri yapar. İlk olarak üç HTML düğmesi daraltma veya genişletme bölmenin içeriği için istemci tarafı JavaScript ardından tetikleyecek sayfaya ekleyin:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

İstemci tarafı JavaScript kodunda (kullanmaya `<script type="text/javascript">`), `$find()` gereken yöntemini kullanılacak erişimi `CollapsiblePanelExtender`. `$find("cpe")` Buna bir başvuru döndürür. Üzerinde buradan belirli yöntemler elinizdeki çözüm.

Yöntem (genişletme) açmak için panel adı verilir `_doOpen()`; aşağıdaki kod uygulayan `doOpen()` işlevi ilk düğmesine tıklandığında çağırılır:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Kapatma ya da bir paneli daraltma `_doClose()` yöntemi yürütülmesi gerekir. Bu nedenle kullanıcı ikinci düğmeye tıkladığında, aşağıdaki JavaScript kodunu çağrılır:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

Alan üçüncü düğmeye paneli durumunu değiştirir: gelen için genişletilmiş, daraltılmış ve bunun tersi de geçerlidir. `CollapsiblePanelExtender` Sunan `toggle()` tam olarak yapar yöntemi: panel durumunu tersine çevirir. Ancak de mevcuttur başka bir yaklaşım (dahili olarak kullanılan tarafından `toggle()` yöntemi): `get_Collapsed()` yöntemi `CollapsiblePanelExtender()` bize paneli veya daraltılmış durumda gösterir. Ya da Genişletilmiş sonra bağlı olarak bu işlev dönüş değeri, masasıdır (`_doOpen()` yöntemi) veya daraltılmış (`_doClose()`) yöntemi:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


[![Alan üçüncü düğmeye paneli durumunu değiştirir: genişletilmiş ve geri gelen daraltılmış](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

Alan üçüncü düğmeye paneli durumunu değiştirir: genişletilmiş ve geri gelen daraltılmış ([tam boyutlu görüntüyü görmek için tıklatın](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](collapsing-and-expanding-a-panel-from-javascript-vb.md)
