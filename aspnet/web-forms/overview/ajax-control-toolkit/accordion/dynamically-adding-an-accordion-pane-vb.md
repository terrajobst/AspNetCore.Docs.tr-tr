---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Dinamik olarak bir Accordion bölmesi (VB) ekleme | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti Accordion denetimi birden fazla bölme sağlar ve bunlardan biri aynı anda görüntüleme izin verir. Paneller genellikle w bildirilen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 3dd82fab03e06aa5dd3baba7dd24734fa964b350
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378968"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>Dinamik olarak bir Accordion bölmesi (VB) ekleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> AJAX Denetim Araç Seti Accordion denetimi birden fazla bölme sağlar ve bunlardan biri aynı anda görüntüleme izin verir. Paneller genellikle sayfa içinde bildirilen, ancak sunucu tarafı kod aynı sonucu elde etmek için kullanılabilir.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti Accordion denetimi birden fazla bölme sağlar ve bunlardan biri aynı anda görüntüleme izin verir. Paneller genellikle sayfa içinde bildirilen, ancak sunucu tarafı kod aynı sonucu elde etmek için kullanılabilir.

## <a name="steps"></a>Adımlar

Accordion denetimi, sunucu tarafı kodu tüm önemli özellikleri sunar. Başka şeylerin yanında `Panes` özelliği Accordion olun bölmelerin koleksiyonunu erişim verir. Türü her bölmesinde `AccordionPane`. Bu nedenle, böyle bir bölme oluşturmak için Önemsiz:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

`HeaderContainer` Özelliği `AccordionPane` ; bölmesinin üst bilgisi bölümü içinde ASP.NET denetimleri için erişim sağlayan `ContentContainer` özelliği `AccordionPane` bölmesinin içerik bölümü için aynı yapar. Bu içeriği bölmelerini eklemek ASP.NET kodu sağlar:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Son olarak, pane(s) eklenmeli `Panes` Accordion koleksiyonu:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

İki bölme için bir Accordion denetimi ekler tam bir sunucu tarafı kod şu şekildedir:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

Tek eksik öğe ASP.NET varlığını temel bağımlı Accordion kendisi ise `ScriptManager` denetimi:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Örneği tamamlamak için tarayıcı için stil bilgilerini Accordion denetimi başvurulan iki CSS sınıfları sağlar:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![Sunucu tarafı kodu tarafından dinamik olarak accordion verileri eklendikten sonra](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Sunucu tarafı kodu tarafından dinamik olarak accordion verileri eklendikten sonra ([tam boyutlu görüntüyü görmek için tıklatın](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](databinding-to-an-accordion-vb.md)
