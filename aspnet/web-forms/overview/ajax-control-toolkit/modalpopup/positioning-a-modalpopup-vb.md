---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Bir modalpopup (VB)'ı konumlandırma | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Ancak denetim teklif değil bir...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: e36d22b5c2f503ed849f373153e263bdd9546452
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821767"
---
<a name="positioning-a-modalpopup-vb"></a>Bir modalpopup (VB)'ı konumlandırma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)

> AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Ancak denetim açılan konumlandırmak için yerleşik bir işlevsellik sağlamaz.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Ancak denetim açılan konumlandırmak için yerleşik bir işlevsellik sağlamaz.

## <a name="steps"></a>Adımlar

ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager`. Denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

Ardından, kalıcı açılan hizmet veren bir panel ekleme. Bir düğme, açılan kapatmak için kullanılır:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

Açılan gösterilen zaman, belirli bir sayfayı yerde konumlandırılmış. Bu görev için bir istemci tarafı JavaScript işlevi oluşturulur. Paneline erişmek önce çalışır. Başarılı olursa, bölmenin konumu, CSS ve JavaScript (olur açılan pencerenin konumunu değiştir) kullanılarak ayarlanır. Ancak `ModalPopupExtender` denetim açılan konumlandırmak de çalışır. Bu nedenle, JavaScript kodu tekrar tekrar açılan, saniyenin her onda yerleştirir.

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

Gördüğünüz gibi dönüş değeri `setTimeout()` JavaScript yöntemi, bir genel değişken kaydedilir. İsteğe bağlı olarak, açılan yinelenen konumlandırma durdurmak için böylece kullanarak `clearTimeout()` yöntemi:

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

Şimdi yapılması gereken şey bu işlevler, uygun olduğunda çağrı tarayıcısı yapmak için. `movePanel()` Paneli tetikleyen düğmesine tıklandığında, JavaScript işlevi çağrılmalıdır:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

Ve `stopMoving()` işlevi, bu açılır penceresi kapatıldığında oyuna gelir tetiklenen içinde `ModalPopupExtender` denetimi:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


[![Belirtilen konumda kalıcı açılan pencere görünür](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)

Belirtilen konumda kalıcı açılan pencere görünür ([tam boyutlu görüntüyü görmek için tıklatın](positioning-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](handling-postbacks-from-a-modalpopup-vb.md)
