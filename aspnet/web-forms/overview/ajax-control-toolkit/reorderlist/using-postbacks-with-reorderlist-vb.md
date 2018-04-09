---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Geri göndermeler ReorderList (VB) ile kullanma | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti, ReorderList denetimi sürükle ve bırak aracılığıyla kullanıcı tarafından sıralanabilir bir liste sağlar. Listenin kaldırılmasında her bir SAS...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: ef43471f7d8cc94c1a82a368e27acef05f474f81
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="using-postbacks-with-reorderlist-vb"></a>Geri göndermeler ReorderList (VB) ile kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> AJAX Denetim Araç Seti, ReorderList denetimi sürükle ve bırak aracılığıyla kullanıcı tarafından sıralanabilir bir liste sağlar. Listenin kaldırılmasında her geri gönderimin sunucunun değişikliği bildirin.


## <a name="overview"></a>Genel Bakış

`ReorderList` AJAX Denetim Araç Seti denetiminde sürükle ve bırak aracılığıyla kullanıcı tarafından sıralanabilir bir liste sağlar. Listenin kaldırılmasında her geri gönderimin sunucunun değişikliği bildirin.

## <a name="steps"></a>Adımlar

Birkaç olası veri kaynakları için vardır `ReorderList` denetim. Biridir kullanmak için bir `XmlDataSource` denetimi:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Bu XML bağlamak amacıyla bir `ReorderList` denetimi ve etkinleştir Geri göndermeler, aşağıdaki öznitelikler ayarlanmalıdır:

- `DataSourceID`: Veri kaynağı kimliği
- `SortOrderField`: Sıralama ölçütü özelliği
- `AllowReorder`: Liste öğelerini yeniden sıralamak kullanıcı izin verilip verilmeyeceğini
- `PostBackOnReorder`: Liste düzenlenmeyecek her geri gönderimin oluşturulup oluşturulmayacağını

Denetim için uygun biçimlendirme aşağıdaki gibidir:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

İçinde `ReorderList` denetimi, veri kaynağındaki belirli veri bağlı kullanarak `Eval()` yöntemi:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

Son yeniden sıralama oluştuğunda sayfasında rasgele konumunda bir etiket bilgileri tutun:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Bu etiket metin geri gönderme işleme sunucu tarafı kodu olarak girilir:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Son olarak, ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` sayfada denetim koyun:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


[![Her yeniden sıralama geri gönderimin tetikler](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Her yeniden sıralama geri gönderimin tetikler ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-postbacks-with-reorderlist-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](drag-and-drop-via-reorderlist-cs.md)
> [sonraki](drag-and-drop-via-reorderlist-vb.md)
