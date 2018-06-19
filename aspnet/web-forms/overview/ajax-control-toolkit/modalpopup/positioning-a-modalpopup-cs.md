---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Bir ModalPopup (C#) konumlandırma | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Ancak denetim yok satışa bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: bee5be84259231d8cd5efde74b610d72f5e250cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874154"
---
<a name="positioning-a-modalpopup-c"></a>Bir ModalPopup (C#) konumlandırma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)

> AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Ancak denetim açılan konumlandırmak için yerleşik bir işlevselliği sunmaz.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Ancak denetim açılan konumlandırmak için yerleşik bir işlevselliği sunmaz.

## <a name="steps"></a>Adımlar

ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager`. Denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

Ardından, kalıcı açılan hizmet veren bir panel ekleyin. Bir düğme açılan kapatmak için kullanılır:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

Açılan gösterilen her sayfada belirli bir yerde konumlandırılmış. Bu görev için bir istemci tarafı JavaScript işlevi oluşturulur. Paneline erişmek ilk önce çalışır. Başarılı olursa, CSS ve JavaScript (açılan konumunu olacak değiştirin) kullanarak bölmenin konumu ayarlanır. Ancak `ModalPopupExtender` denetimi de dener açılan getirin. Bu nedenle, JavaScript kodu art arda açılan, ikinci bir her onuncu yerleştirir.

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

Gördüğünüz gibi dönüş değerini `setTimeout()` JavaScript yöntemi genel bir değişkende kaydedilir. Bu isteğe bağlı olarak, açılan yinelenen konumlandırma durdurmak için sağlar kullanarak `clearTimeout()` yöntemi:

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

Şimdi yapmak için sol olan uygun olduğunda bu işlev çağrısı tarayıcı yapma. `movePanel()` Masası tetikleyen düğme tıklatıldığında JavaScript işlevi çağrılmalıdır:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

Ve `stopMoving()` işlevi gelen oyuna açılan bu kapatıldığında tetiklenen `ModalPopupExtender` denetimi:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


[![Kalıcı açılan belirlenen konumunda görünür](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)

Kalıcı açılan belirlenen konumunda görünür ([tam boyutlu görüntüyü görüntülemek için tıklatın](positioning-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](handling-postbacks-from-a-modalpopup-cs.md)
> [sonraki](launching-a-modal-popup-window-from-server-code-vb.md)
