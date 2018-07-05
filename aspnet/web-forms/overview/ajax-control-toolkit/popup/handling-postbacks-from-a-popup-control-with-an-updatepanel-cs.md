---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: UpdatePanel (C#) ile bir açılan denetimden gelen geri göndermeleri işleme | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti, PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde, popup tetiklemek için kolay bir yolunu sunar. Özel dikkat edilmelidir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 758ed5eef32be0fb596d8b9dbf903729816746bd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377979"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a>UpdatePanel (C#) ile bir açılan denetimden gelen geri göndermeleri işleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)

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

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

Unutmayın `OnSelectionChanged` özniteliği `Calendar` denetim ayarlanır. Kullanıcının Takvim içinde bir tarih seçtiğinde, bir geri gönderme oluşmaz ve sunucu tarafı yöntemi `c1_SelectionChanged()` yürütülür. Bu yöntem içinde geçerli tarihi alınır ve TextBox'a geri yazılır.

Bu sözdizimi aşağıdaki gibidir: öncelikle, bir proxy nesnesi `PopupControlExtender` sayfada oluşturulmuş olması gerekir. ASP.NET AJAX Denetim Araç Seti sunar `GetProxyForCurrentPopup()` yöntemi. Bu yöntem döndürür nesne destekleyen `Commit()` yöntemi bir değer (yöntem çağrısı tetikleyen denetimi değil!) açılan tetiklenen denetim geri gönderir. Aşağıdaki kod, seçilen tarihten bağımsız değişkeni olarak sağlar `Commit()` yöntemi, seçilen tarihten metin kutusuna geri yazmak için kodu neden:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

Bir takvim tarihi, ilişkili metin kutusuna, seçilen tarihten görünür tıklattığınızda artık bir tarih seçici denetimi oluşturma şu anda birçok Web sitesi üzerinde bulunabilir.


[![Takvim kullanıcı TextBox'a tıkladığında görünür](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

Takvim kullanıcı TextBox'a tıkladığında görünür ([tam boyutlu görüntüyü görmek için tıklatın](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))


[![Bir tarihte tıklayarak metin kutusunda yerleştirir](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

Bir tarihte tıklayarak onu koyar metin kutusu içinde ([tam boyutlu görüntüyü görmek için tıklatın](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Önceki](using-multiple-popup-controls-cs.md)
> [İleri](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
