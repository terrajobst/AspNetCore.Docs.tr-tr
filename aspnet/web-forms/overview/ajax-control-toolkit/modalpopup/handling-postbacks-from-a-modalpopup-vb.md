---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Geri göndermeler ModalPopup (VB) gelen işleme | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Özellikle dikkatli bir pos olduğunda gerçekleştirilecek gerekir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 1aa2c42deb67015dd0b35edf4ba72d8d667ec88c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a>Geri göndermeler ModalPopup (VB) gelen işleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Geri gönderimin içinde açılan oluşturulduğunda, özellikle dikkatli olunmalıdır.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Geri gönderimin içinde açılan oluşturulduğunda, özellikle dikkatli olunmalıdır.

## <a name="steps"></a>Adımlar

ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

Ardından, kalıcı açılan hizmet veren bir panel ekleyin. Burada, kullanıcı bir ad ve e-posta adresi girebilirsiniz. Bir düğme açılan kapatın ve bilgileri kaydetmek için kullanılır. Unutmayın `OnClick` özniteliği, böylece bu düğmeye tıklandığında geri gönderimin oluştuğu ayarlanır:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

İki etiketlerini tam olarak aynı bilgiler için sayfa oluşur: ad ve e-posta adresi. Bir düğme kalıcı açılan tetiklemek için kullanılır:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

Açılan pencere görünür yapmak için add `ModalPopupExtender` denetim. Ayarlama `PopupControlID` özniteliği bölmenin Kimliğine ve `TargetControlID` düğmenin kimliği:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

Artık her `Save` kalıcı açılır içindeki düğmesine tıklandığında, sunucu tarafı `SaveData()` yöntemi yürütüldüğünde. Burada, bir veri deposunda girilen veriler kaydedebilir. Basitleştirmek amacıyla, yeni verileri yalnızca etiketinde çıktı:

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

Ayrıca, textbox denetimleri kalıcı açılır içindeki geçerli bir ad ve e-posta ile doldurulmalıdır. Hiçbir geri gönderme oluştuğunda ancak bu yalnızca gereklidir. Geri gönderimin ise, ASP.NET viewstate özelliği otomatik olarak metin kutuları uygun değerlerle doldurun.

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


[![Kalıcı açılan geri gönderimin neden olur.](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

Kalıcı açılan geri gönderimin neden olur ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](using-modalpopup-with-a-repeater-control-vb.md)
> [sonraki](positioning-a-modalpopup-vb.md)
