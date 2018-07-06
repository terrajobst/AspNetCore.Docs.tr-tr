---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Dinamik olarak Accordion bölmesi bir (C#) ekleme | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti Accordion denetimi birden fazla bölme sağlar ve bunlardan biri aynı anda görüntüleme izin verir. Paneller genellikle w bildirilen...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 60bff8dd2d06356a1f2cc771cf5b7fa9c4e4eac5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803262"
---
<a name="dynamically-adding-an-accordion-pane-c"></a>Dinamik olarak Accordion bölmesi bir (C#) ekleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> AJAX Denetim Araç Seti Accordion denetimi birden fazla bölme sağlar ve bunlardan biri aynı anda görüntüleme izin verir. Paneller genellikle sayfa içinde bildirilen, ancak sunucu tarafı kod aynı sonucu elde etmek için kullanılabilir.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti Accordion denetimi birden fazla bölme sağlar ve bunlardan biri aynı anda görüntüleme izin verir. Paneller genellikle sayfa içinde bildirilen, ancak sunucu tarafı kod aynı sonucu elde etmek için kullanılabilir.

## <a name="steps"></a>Adımlar

Accordion denetimi, sunucu tarafı kodu tüm önemli özellikleri sunar. Başka şeylerin yanında `Panes` özelliği Accordion olun bölmelerin koleksiyonunu erişim verir. Türü her bölmesinde `AccordionPane`. Bu nedenle, böyle bir bölme oluşturmak için Önemsiz:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

`HeaderContainer` Özelliği `AccordionPane` ; bölmesinin üst bilgisi bölümü içinde ASP.NET denetimleri için erişim sağlayan `ContentContainer` özelliği `AccordionPane` bölmesinin içerik bölümü için aynı yapar. Bu içeriği bölmelerini eklemek ASP.NET kodu sağlar:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Son olarak, pane(s) eklenmeli `Panes` Accordion koleksiyonu:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

İki bölme için bir Accordion denetimi ekler tam bir sunucu tarafı kod şu şekildedir:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

Tek eksik öğe ASP.NET varlığını temel bağımlı Accordion kendisi ise `ScriptManager` denetimi:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Örneği tamamlamak için tarayıcı için stil bilgilerini Accordion denetimi başvurulan iki CSS sınıfları sağlar:

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


[![Sunucu tarafı kodu tarafından dinamik olarak accordion verileri eklendikten sonra](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

Sunucu tarafı kodu tarafından dinamik olarak accordion verileri eklendikten sonra ([tam boyutlu görüntüyü görmek için tıklatın](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](databinding-to-an-accordion-cs.md)
> [İleri](databinding-to-an-accordion-vb.md)
