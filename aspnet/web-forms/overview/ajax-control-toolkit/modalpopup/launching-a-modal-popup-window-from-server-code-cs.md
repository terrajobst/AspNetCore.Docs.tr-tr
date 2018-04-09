---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Sunucu kod (C#) kalıcı açılır penceresinden başlatma | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Ancak bazı senaryolar bu t iste...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 04bb056ee29065a472a70d480568b897a789ae59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a>Sunucu kod (C#) kalıcı açılır penceresinden başlatma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)

> AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Ancak bazı senaryolar kalıcı açılan açılmasını sunucu tarafında tetiklenir gerektirir.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Ancak bazı senaryolar kalıcı açılan açılmasını sunucu tarafında tetiklenir gerektirir.

## <a name="steps"></a>Adımlar

Öncelikle, bir ASP.NET düğme web denetimi ModalPopup denetiminin nasıl çalıştığını göstermek için gereklidir. İçinde bu tür bir düğme ekleme &lt;form&gt; öğe yeni bir sayfa üzerinde:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

Ardından, oluşturmak istediğiniz açılan için işaretleme gerekir. Olarak tanımlayan bir `<asp:Panel>` denetlemek ve düğme denetimi içerdiğinden emin olun. ModalPopup denetim böyle bir düğmeyi Kapat açılan hale getirmek için işlevselliği sunar; Aksi takdirde, kaybolur izin vermek için kolay bir yolu yoktur.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

Sonraki ModalPopup denetimi ASP.NET AJAX araç setinin sayfasına ekleyin. Denetim yükleyen düğmesi, kayboluyor kılan düğmesi ve gerçek açılan Kimliğini özelliklerini ayarlayın.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

Gibi ASP.NET AJAX tabanlı tüm web sayfalarıyla; komut dosyası Yöneticisi'ni, farklı bir hedef tarayıcılar için gerekli JavaScript kitaplıklarını yüklemek için gereklidir:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

Örneğin tarayıcıda çalıştırın. Düğmeyi tıkladığınızda, kalıcı açılan görüntülenir. Sunucu tarafı kodu kullanarak aynı sonucu elde etmek için yeni bir düğme gereklidir:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

Gördüğünüz gibi bir düğmeye tıklayın geri gönderimin oluşturur ve çalıştırır `ServerButton_Click()` sunucuda yöntemi. Bu yöntemde, JavaScript işlevini çağırdı `launchModal()` yürütülür Sayfa yüklendikten sonra tam olması için JavaScript işlevinin yürütülecek:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

İş `launchModal()` ModalPopup görüntülemektir. `launchModal()` İşlevi tam HTML sayfası yüklendikten sonra yürütülür. O anda ancak, ASP.NET AJAX framework henüz tam olarak yüklendi değil. Bu nedenle, `launchModal()` işlevi yalnızca ModalPopup denetim daha sonra gösterilecek bir değişken ayarlar:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

`pageLoad()` JavaScript işlevi, ASP.NET AJAX tam yüklenmeden silindikten sonra yürütülen özel bir işlev değil. Kod ModalPopup denetimi, ancak yalnızca göstermek için bu işleve bu nedenle eklediğimiz `launchModal()` önce çağrılır:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

`$find()` İşlevi bir adlandırılmış öğeyi sayfada aranıyor ve bir parametre olarak sunucu tarafı kodu bekliyor. Bu nedenle, `$find("mpe")` ModalPopup denetim istemci gösterimini döndürür; kendi `show()` yöntemi görünür açılan olanak sağlar.


[![Kalıcı açılan ne zaman görünür düğmelerin ya da tıklandığında](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)

Kalıcı açılan ne zaman görünür düğmelerin ya da tıklandığında ([tam boyutlu görüntüyü görüntülemek için tıklatın](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-modalpopup-with-a-repeater-control-cs.md)
