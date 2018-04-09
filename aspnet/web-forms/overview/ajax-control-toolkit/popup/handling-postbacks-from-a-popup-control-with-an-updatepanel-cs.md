---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Geri göndermeler bir UpdatePanel (C#) ile açılan kontrolü işleme | Microsoft Docs
author: wenz
description: AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar. Özellikle dikkat edilmelidir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: abedb5247f710b02752651a7bfb011ab63d32844
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a>Geri göndermeler bir UpdatePanel (C#) ile açılan kontrolü işleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)

> AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar. Bu tür bir açılan içinde geri gönderimin oluştuğu olduğunda gerçekleştirilecek özellikle dikkat edilmelidir.


## <a name="overview"></a>Genel Bakış

AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar. Bu tür bir açılan içinde geri gönderimin oluştuğu olduğunda gerçekleştirilecek özellikle dikkat edilmelidir.

## <a name="steps"></a>Adımlar

Kullanırken bir `PopupControl` bir geri gönderme ile bir `UpdatePanel` tarafından geri gönderme neden sayfa yenileme engelleyebilirsiniz. Aşağıdaki biçimlendirmede birkaç önemli öğe tanımlar:

- A `ScriptManager` ASP.NET AJAX Denetim Araç Seti çalışmasını denetleme
- İki `TextBox` her ikisi de popup tetikleyecek denetimleri
- A `Panel` açılan hizmet verecektir denetimi
- Paneli içindeki bir `Calendar` içinde denetim katıştırılmış bir `UpdatePanel` denetimi
- İki `PopupControlExtender` paneli metin kutuları atamak denetimleri

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

Unutmayın `OnSelectionChanged` özniteliği `Calendar` denetim ayarlanır. Kullanıcının Takvim içindeki bir tarih seçtiğinde geri gönderimin oluşmaz ve sunucu tarafı yöntemi `c1_SelectionChanged()` yürütülür. Bu yöntem içinde geçerli tarih alınan ve metin kutusuna geri yazabilirsiniz.

Söz dizimi aşağıdaki gibidir: öncelikle, bir proxy nesnesi `PopupControlExtender` sayfada oluşturulmuş olması gerekir. ASP.NET AJAX Denetim Araç Seti sunar `GetProxyForCurrentPopup()` yöntemi. Bu yöntem nesnesi destekler `Commit()` yöntemi bir değer (yöntem çağrısının tetikleyen denetimin değil!) açılan tetiklenen denetim geri gönderir. Seçilen tarih için bağımsız değişken olarak aşağıdaki kod sağlar `Commit()` yöntemi, seçilen tarihten geri metin kutusuna yazmak için kodu neden:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

Seçilen tarih ilişkili metin kutusunda görünür bir takvim tarihi tıklattığınızda şimdi bir tarih seçici denetimi oluşturma şu anda birçok Web sitelerinde bulunabilir.


[![Takvim kullanıcı metin kutusuna tıkladığında görüntülenir](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

Takvim kullanıcı metin kutusuna tıklattığında görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))


[![Bir tarihte tıklatarak metin kutusuna yerleştirir](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

Bir tarihte tıklatarak koyar, metin kutusuna ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Önceki](using-multiple-popup-controls-cs.md)
> [sonraki](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
