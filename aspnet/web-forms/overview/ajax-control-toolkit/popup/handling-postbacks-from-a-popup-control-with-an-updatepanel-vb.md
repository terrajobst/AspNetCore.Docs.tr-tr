---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: UpdatePanel (VB) ile bir açılan denetimden gelen geri göndermeleri işleme | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti, PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde, popup tetiklemek için kolay bir yolunu sunar. Özel dikkat edilmelidir...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 2cd6f03b805ce209b736ab9c105fadd3ba6ac26b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826150"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a>UpdatePanel (VB) ile bir açılan denetimden gelen geri göndermeleri işleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)

> AJAX Denetim Araç Seti, PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde, popup tetiklemek için kolay bir yolunu sunar. Böyle bir açılan pencere içinde bir geri gönderme gerçekleştiğinde yapılacak önemi sahiptir.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti, PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde, popup tetiklemek için kolay bir yolunu sunar. Böyle bir açılan pencere içinde bir geri gönderme gerçekleştiğinde yapılacak önemi sahiptir.

## <a name="steps"></a>Adımlar

Kullanırken bir `PopupControl` bir geri gönderme bir `UpdatePanel` göndermenin neden sayfa yenileme engelleyebilirsiniz. Aşağıdaki biçimlendirmede birkaç önemli öğe tanımlar:

- A `ScriptManager` ASP.NET AJAX Denetim Araç Seti çalışmasını kontrol
- İki `TextBox` hem popup tetikleyecek denetimleri
- A `Panel` açılan menüsü hizmet verecek denetimi
- Panel içinde bir `Calendar` denetimi içine gömüldüğü bir `UpdatePanel` denetimi
- İki `PopupControlExtender` metin kutuları paneli atama denetimleri

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

Unutmayın `OnSelectionChanged` özniteliği `Calendar` denetim ayarlanır. Kullanıcının Takvim içinde bir tarih seçtiğinde, bir geri gönderme oluşmaz ve sunucu tarafı yöntemi `c1_SelectionChanged()` yürütülür. Bu yöntem içinde geçerli tarihi alınır ve TextBox'a geri yazılır.

Bu sözdizimi aşağıdaki gibidir: öncelikle, bir proxy nesnesi `PopupControlExtender` sayfada oluşturulmuş olması gerekir. ASP.NET AJAX Denetim Araç Seti sunar `GetProxyForCurrentPopup()` yöntemi. Bu yöntem döndürür nesne destekleyen `Commit()` yöntemi bir değer (yöntem çağrısı tetikleyen denetimi değil!) açılan tetiklenen denetim geri gönderir. Aşağıdaki kod, seçilen tarihten bağımsız değişkeni olarak sağlar `Commit()` yöntemi, seçilen tarihten metin kutusuna geri yazmak için kodu neden:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

Bir takvim tarihi, ilişkili metin kutusuna, seçilen tarihten görünür tıklattığınızda artık bir tarih seçici denetimi oluşturma şu anda birçok Web sitesi üzerinde bulunabilir.


[![Takvim kullanıcı TextBox'a tıkladığında görünür](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)

Takvim kullanıcı TextBox'a tıkladığında görünür ([tam boyutlu görüntüyü görmek için tıklatın](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))


[![Bir tarihte tıklayarak metin kutusunda yerleştirir](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)

Bir tarihte tıklayarak onu koyar metin kutusu içinde ([tam boyutlu görüntüyü görmek için tıklatın](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Önceki](using-multiple-popup-controls-vb.md)
> [İleri](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)
