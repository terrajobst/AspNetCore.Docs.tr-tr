---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Bir Accordion bölmesi (C#) dinamik olarak ekleme | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar. Paneller genellikle w bildirilir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: ad2fc6ea3d527215c0226f3f594d781163d538b5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-adding-an-accordion-pane-c"></a>Bir Accordion bölmesi (C#) dinamik olarak ekleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar. Paneller genellikle sayfa içinde bildirilen, ancak sunucu tarafı kodu aynı sonucu elde etmek için kullanılabilir.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar. Paneller genellikle sayfa içinde bildirilen, ancak sunucu tarafı kodu aynı sonucu elde etmek için kullanılabilir.

## <a name="steps"></a>Adımlar

Accordion denetim sunucu tarafı kodu tüm önemli özellikleri sunar. Başka şeylerin `Panes` özelliği Accordion olun bölmeleri koleksiyonuna erişim verir. Tür her bölmesinde `AccordionPane`. Bu nedenle, bu tür bir bölme oluşturmak için Önemsiz şöyledir:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

`HeaderContainer` Özelliği `AccordionPane` ASP.NET denetimleri bölmesi; üstbilgi bölümünün içinde erişmenizi sağlar `ContentContainer` özelliği `AccordionPane` bölmesinin içerik bölümü için aynı değil. Bu içerik için bölmeleri eklemek ASP.NET kodu sağlar:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Son olarak, pane(s) eklenmeli `Panes` Accordion koleksiyonu:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

İki bölme bir Accordion denetimini ekler tam bir sunucu tarafı kodu şöyledir:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

Yalnızca eksik, ASP.NET varlığına bağlı Accordion kendisini öğedir `ScriptManager` denetimi:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Örnek tamamlamak için tarayıcı için stil bilgilerini Accordion denetiminde başvurulan iki CSS sınıfları sağlar.

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


[![Accordion verilerde sunucu tarafı kodu tarafından dinamik olarak eklendi](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

Accordion verilerde sunucu tarafı kodu tarafından dinamik olarak eklendi ([tam boyutlu görüntüyü görüntülemek için tıklatın](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](databinding-to-an-accordion-cs.md)
> [sonraki](databinding-to-an-accordion-vb.md)
