---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: (VB) ReorderList ile geri gönderme kullanma | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ReorderList denetimi sürükle ve bırak kullanıcı tarafından sıralanabilir bir liste sağlar. Listeyi yeniden her bir SAS...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 20214db1c038f417944bd374bdaa8ce55d6d44aa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839005"
---
<a name="using-postbacks-with-reorderlist-vb"></a>(VB) ReorderList ile geri gönderme kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> AJAX Denetim Araç Seti ReorderList denetimi sürükle ve bırak kullanıcı tarafından sıralanabilir bir liste sağlar. Listeyi yeniden olduğunda, bir geri gönderme sunucunun değişikliği bildirmek.


## <a name="overview"></a>Genel Bakış

`ReorderList` AJAX Denetim Araç Seti denetimi sürükle ve bırak kullanıcı tarafından sıralanabilir bir liste sağlar. Listeyi yeniden olduğunda, bir geri gönderme sunucunun değişikliği bildirmek.

## <a name="steps"></a>Adımlar

Birkaç olası veri kaynağı için `ReorderList` denetimi. Biridir kullanmak için bir `XmlDataSource` denetimi:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Bu XML bağlamak amacıyla bir `ReorderList` denetimi ve etkinleştirme Geri göndermeler, aşağıdaki öznitelikleri ayarlanmalıdır:

- `DataSourceID`: Veri kaynağı kimliği
- `SortOrderField`: Sıralama ölçütü özelliği
- `AllowReorder`: Kullanıcının, liste öğelerini yeniden sıralamak izin verilip verilmeyeceğini
- `PostBackOnReorder`: Listeyi yeniden düzenlenir her bir geri gönderme oluşturulup oluşturulmayacağını

Denetim için uygun biçimlendirmesi şöyledir:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

İçinde `ReorderList` denetimi, veri kaynağındaki belirli veri bağımlı kullanarak `Eval()` yöntemi:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

Son yeniden sıralama oluştuğunda rastgele bir konumda sayfasında, bilgileri bir etiket tutar:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Bu etiket, metin işleme geri gönderme sunucu tarafı kodu ile doldurulur:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Son olarak, ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` sayfada denetim yerleştirin:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


[![Bir geri gönderme tetikleyen her yeniden sıralama](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Bir geri gönderme her sipariş tetikler ([tam boyutlu görüntüyü görmek için tıklatın](using-postbacks-with-reorderlist-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](drag-and-drop-via-reorderlist-cs.md)
> [İleri](drag-and-drop-via-reorderlist-vb.md)
