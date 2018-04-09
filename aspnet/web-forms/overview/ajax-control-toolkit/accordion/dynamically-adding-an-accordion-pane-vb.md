---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Bir Accordion bölmesi (VB) dinamik olarak ekleme | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar. Paneller genellikle w bildirilir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 68c60ba6d4be5eb6709f7558d6be4165f8232a4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>Bir Accordion bölmesi (VB) dinamik olarak ekleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar. Paneller genellikle sayfa içinde bildirilen, ancak sunucu tarafı kodu aynı sonucu elde etmek için kullanılabilir.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar. Paneller genellikle sayfa içinde bildirilen, ancak sunucu tarafı kodu aynı sonucu elde etmek için kullanılabilir.

## <a name="steps"></a>Adımlar

Accordion denetim sunucu tarafı kodu tüm önemli özellikleri sunar. Başka şeylerin `Panes` özelliği Accordion olun bölmeleri koleksiyonuna erişim verir. Tür her bölmesinde `AccordionPane`. Bu nedenle, bu tür bir bölme oluşturmak için Önemsiz şöyledir:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

`HeaderContainer` Özelliği `AccordionPane` ASP.NET denetimleri bölmesi; üstbilgi bölümünün içinde erişmenizi sağlar `ContentContainer` özelliği `AccordionPane` bölmesinin içerik bölümü için aynı değil. Bu içerik için bölmeleri eklemek ASP.NET kodu sağlar:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Son olarak, pane(s) eklenmeli `Panes` Accordion koleksiyonu:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

İki bölme bir Accordion denetimini ekler tam bir sunucu tarafı kodu şöyledir:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

Yalnızca eksik, ASP.NET varlığına bağlı Accordion kendisini öğedir `ScriptManager` denetimi:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Örnek tamamlamak için tarayıcı için stil bilgilerini Accordion denetiminde başvurulan iki CSS sınıfları sağlar.

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![Accordion verilerde sunucu tarafı kodu tarafından dinamik olarak eklendi](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Accordion verilerde sunucu tarafı kodu tarafından dinamik olarak eklendi ([tam boyutlu görüntüyü görüntülemek için tıklatın](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](databinding-to-an-accordion-vb.md)
