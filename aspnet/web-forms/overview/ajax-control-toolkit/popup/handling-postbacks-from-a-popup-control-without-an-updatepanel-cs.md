---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: "Geri göndermeler bir UpdatePanel (C#) olmadan açılan kontrolü işleme | Microsoft Docs"
author: wenz
description: "AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar. Su içinde geri gönderimin meydana geldiğinde..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: df43052950b6186908fe1baf04808f40cb926f69
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>Geri göndermeler bir UpdatePanel (C#) olmadan açılan kontrolü işleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar. Bu tür bir panelinde geri gönderimin oluştuğu ve sayfada birkaç paneller yoksa hangi panele tıklattınız belirlemek zordur.


## <a name="overview"></a>Genel Bakış

AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar. Bu tür bir panelinde geri gönderimin oluştuğu ve sayfada birkaç paneller yoksa hangi panele tıklattınız belirlemek zordur.

## <a name="steps"></a>Adımlar

Kullanırken bir `PopupControl` geri gönderimin olan, ancak sahip olmayan bir `UpdatePanel` sayfasında Denetim Araç Seti, hangi istemci öğesi sırayla geri gönderme neden açılan başlatmış olabilir belirlemek için bir yol sağlamaz. Ancak küçük eli bu senaryo için geçici bir çözüm sağlar.

Öncelikle, temel kurulum şöyledir: hangi hem aynı açılan, takvimi tetikleyen iki metin kutusu. İki `PopupControlExtenders` metin kutuları ve açılan bir araya getirme.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

Gizli form alanına eklemek için temel fikirdir &lt; `form` &gt; popup başlattı metin kutusunu tutan öğe:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

Sayfa yüklendiğinde, JavaScript kodu olay işleyicisi iki metin kutusuna ekler.: kullanıcı bir metin kutusunda tıkladığında adını gizli form alanına yazılır:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

Sunucu tarafı kodu gizli alanın değerini okumalısınız. Gizli form alanlarını yönetmek için Önemsiz olduğundan, gizli değeri doğrulamak için bir beyaz liste yaklaşım gereklidir. Doğru metin kutusunda belirlendikten sonra takvimden tarihi üzerine yazılır.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


[![Takvim kullanıcı metin kutusuna tıkladığında görüntülenir](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

Takvim kullanıcı metin kutusuna tıklattığında görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))


[![Bir tarihte tıklatarak metin kutusuna yerleştirir](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

Bir tarihte tıklatarak koyar, metin kutusuna ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

>[!div class="step-by-step"]
[Önceki](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[sonraki](using-multiple-popup-controls-vb.md)
