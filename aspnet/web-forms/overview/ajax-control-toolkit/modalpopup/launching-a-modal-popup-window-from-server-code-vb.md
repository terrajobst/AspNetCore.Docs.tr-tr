---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: (VB) sunucu kodundan kalıcı açılan pencere başlatma | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Ancak bazı senaryolarda bu t gerektirir...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 52eec262a9241aa7b11cbb892bdf2142c7a2b83a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822774"
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a>(VB) sunucu kodundan kalıcı açılan pencere başlatma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Ancak bazı senaryolar kalıcı açılan açılmasına sunucu tarafında tetiklenir gerektirir.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Ancak bazı senaryolar kalıcı açılan açılmasına sunucu tarafında tetiklenir gerektirir.

## <a name="steps"></a>Adımlar

İlk olarak bir ASP.NET düğme web denetimi ModalPopup denetiminin nasıl çalıştığını göstermek için gereklidir. Böyle bir düğme içinde ekleyin &lt;form&gt; öğe yeni bir sayfa üzerinde:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

Ardından, oluşturmak istediğiniz popup için işaretleme gerekir. Olarak tanımlayan bir `<asp:Panel>` denetlemek ve bir düğme denetimi içerdiğinden emin olun. ModalPopup denetim açılan böyle bir düğme yakın hale getirmek için işlevsellik sunar. Aksi takdirde, kaybolur izin vermek için kolay bir yolu yoktur.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

ASP.NET AJAX araç setinin sonraki sayfasına ModalPopup denetim ekleme. Denetim yükleyen düğmesi, kayboluyor getiren düğmesi ve gerçek açılan kimliği özelliklerini ayarlayın.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

ASP.NET AJAX üzerinde göre tüm web sayfalarıyla olarak; Betik Manager, farklı tarayıcılar için gerekli JavaScript kitaplıklarını yüklemek için gereklidir:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

Örneğin, tarayıcıda çalıştırın. Düğmeye tıkladığınızda, kalıcı açılan pencere görünür. Sunucu tarafı kod kullanarak aynı etkiyi elde etmek için yeni bir düğme gereklidir:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

Gördüğünüz gibi bir düğmeye tıklayarak bir geri gönderme oluşturur ve yürütür `ServerButton_Click()` sunucuda yöntemi. Bu yöntemde bir JavaScript işlevi olarak adlandırılan `launchModal()` yürütülür sayfası yüklendikten sonra tam olması için JavaScript işlevinin yürütülecek:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

İşin `launchModal()` ModalPopup görüntülemektir. `launchModal()` İşlevi tam HTML sayfası yüklendikten sonra yürütülür. Bu arada, Bununla birlikte, ASP.NET AJAX framework henüz tam olarak yüklenmemiş. Bu nedenle, `launchModal()` işlevi yalnızca ModalPopup denetim daha sonra gösterilecek bir değişken ayarlar:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

`pageLoad()` JavaScript işlevidir ASP.NET AJAX tamamen yüklendikten sonra yürütülen özel bir işlev. Kod ModalPopup denetimidir, ancak yalnızca göstermek için bu işlevi bu nedenle eklediğimiz `launchModal()` önce çağrıldı:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

`$find()` İşlevi adlandırılmış bir öğeyi sayfada arayışındadır ve sunucu tarafı kimliği bir parametre bekliyor. Bu nedenle, `$find("mpe")` ModalPopup denetimi istemci gösterimini döndürür; `show()` görünen açılan yöntemi sağlar.


[![Kalıcı açılan görüntülenir ya da düğme tıklandığında](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

Kalıcı açılan görüntülenir ya da düğme tıklandığında ([tam boyutlu görüntüyü görmek için tıklatın](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](positioning-a-modalpopup-cs.md)
> [İleri](using-modalpopup-with-a-repeater-control-vb.md)
