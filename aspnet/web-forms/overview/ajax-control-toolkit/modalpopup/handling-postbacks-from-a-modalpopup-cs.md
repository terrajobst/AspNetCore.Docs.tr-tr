---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: "Geri göndermeler bir ModalPopup (C#) gelen işleme | Microsoft Docs"
author: wenz
description: "AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Özellikle dikkatli bir pos olduğunda gerçekleştirilecek gerekir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 096c32d6004474d3d5952ce12d7349b9ebda6003
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-modalpopup-c"></a>İşleme Geri göndermeler gelen ModalPopup (C#)
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Geri gönderimin içinde açılan oluşturulduğunda, özellikle dikkatli olunmalıdır.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Geri gönderimin içinde açılan oluşturulduğunda, özellikle dikkatli olunmalıdır.

## <a name="steps"></a>Adımlar

ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

Ardından, kalıcı açılan hizmet veren bir panel ekleyin. Burada, kullanıcı bir ad ve e-posta adresi girebilirsiniz. Bir düğme açılan kapatın ve bilgileri kaydetmek için kullanılır. Unutmayın `OnClick` özniteliği, böylece bu düğmeye tıklandığında geri gönderimin oluştuğu ayarlanır:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

İki etiketlerini tam olarak aynı bilgiler için sayfa oluşur: ad ve e-posta adresi. Bir düğme kalıcı açılan tetiklemek için kullanılır:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

Açılan pencere görünür yapmak için add `ModalPopupExtender` denetim. Ayarlama `PopupControlID` özniteliği bölmenin Kimliğine ve `TargetControlID` düğmenin kimliği:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

Artık her `Save` kalıcı açılır içindeki düğmesine tıklandığında, sunucu tarafı `SaveData()` yöntemi yürütüldüğünde. Burada, bir veri deposunda girilen veriler kaydedebilir. Basitleştirmek amacıyla, yeni verileri yalnızca etiketinde çıktı:

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

Ayrıca, textbox denetimleri kalıcı açılır içindeki geçerli bir ad ve e-posta ile doldurulmalıdır. Hiçbir geri gönderme oluştuğunda ancak bu yalnızca gereklidir. Geri gönderimin ise, ASP.NET viewstate özelliği otomatik olarak metin kutuları uygun değerlerle doldurun.

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


[![Kalıcı açılan geri gönderimin neden olur.](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

Kalıcı açılan geri gönderimin neden olur ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

>[!div class="step-by-step"]
[Önceki](using-modalpopup-with-a-repeater-control-cs.md)
[sonraki](positioning-a-modalpopup-cs.md)
