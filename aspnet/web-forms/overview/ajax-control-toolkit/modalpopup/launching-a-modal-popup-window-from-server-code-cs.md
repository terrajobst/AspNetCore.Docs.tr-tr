---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: (C#) sunucu kodundan kalıcı açılan pencere başlatma | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Ancak bazı senaryolarda bu t gerektirir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: a13915f5748f8ad9b79fb787cc5e0682faa19388
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374097"
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a>(C#) sunucu kodundan kalıcı açılan pencere başlatma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)

> AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Ancak bazı senaryolar kalıcı açılan açılmasına sunucu tarafında tetiklenir gerektirir.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Ancak bazı senaryolar kalıcı açılan açılmasına sunucu tarafında tetiklenir gerektirir.

## <a name="steps"></a>Adımlar

İlk olarak bir ASP.NET düğme web denetimi ModalPopup denetiminin nasıl çalıştığını göstermek için gereklidir. Böyle bir düğme içinde ekleyin &lt;form&gt; öğe yeni bir sayfa üzerinde:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

Ardından, oluşturmak istediğiniz popup için işaretleme gerekir. Olarak tanımlayan bir `<asp:Panel>` denetlemek ve bir düğme denetimi içerdiğinden emin olun. ModalPopup denetim açılan böyle bir düğme yakın hale getirmek için işlevsellik sunar. Aksi takdirde, kaybolur izin vermek için kolay bir yolu yoktur.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

ASP.NET AJAX araç setinin sonraki sayfasına ModalPopup denetim ekleme. Denetim yükleyen düğmesi, kayboluyor getiren düğmesi ve gerçek açılan kimliği özelliklerini ayarlayın.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

ASP.NET AJAX üzerinde göre tüm web sayfalarıyla olarak; Betik Manager, farklı tarayıcılar için gerekli JavaScript kitaplıklarını yüklemek için gereklidir:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

Örneğin, tarayıcıda çalıştırın. Düğmeye tıkladığınızda, kalıcı açılan pencere görünür. Sunucu tarafı kod kullanarak aynı etkiyi elde etmek için yeni bir düğme gereklidir:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

Gördüğünüz gibi bir düğmeye tıklayarak bir geri gönderme oluşturur ve yürütür `ServerButton_Click()` sunucuda yöntemi. Bu yöntemde bir JavaScript işlevi olarak adlandırılan `launchModal()` yürütülür sayfası yüklendikten sonra tam olması için JavaScript işlevinin yürütülecek:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

İşin `launchModal()` ModalPopup görüntülemektir. `launchModal()` İşlevi tam HTML sayfası yüklendikten sonra yürütülür. Bu arada, Bununla birlikte, ASP.NET AJAX framework henüz tam olarak yüklenmemiş. Bu nedenle, `launchModal()` işlevi yalnızca ModalPopup denetim daha sonra gösterilecek bir değişken ayarlar:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

`pageLoad()` JavaScript işlevidir ASP.NET AJAX tamamen yüklendikten sonra yürütülen özel bir işlev. Kod ModalPopup denetimidir, ancak yalnızca göstermek için bu işlevi bu nedenle eklediğimiz `launchModal()` önce çağrıldı:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

`$find()` İşlevi adlandırılmış bir öğeyi sayfada arayışındadır ve sunucu tarafı kimliği bir parametre bekliyor. Bu nedenle, `$find("mpe")` ModalPopup denetimi istemci gösterimini döndürür; `show()` görünen açılan yöntemi sağlar.


[![Kalıcı açılan görüntülenir ya da düğme tıklandığında](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)

Kalıcı açılan görüntülenir ya da düğme tıklandığında ([tam boyutlu görüntüyü görmek için tıklatın](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-modalpopup-with-a-repeater-control-cs.md)
