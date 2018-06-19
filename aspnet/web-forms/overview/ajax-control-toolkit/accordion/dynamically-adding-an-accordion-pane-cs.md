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
ms.locfileid: "30879471"
---
<a name="dynamically-adding-an-accordion-pane-c"></a><span data-ttu-id="2e70d-104">Bir Accordion bölmesi (C#) dinamik olarak ekleme</span><span class="sxs-lookup"><span data-stu-id="2e70d-104">Dynamically Adding An Accordion Pane (C#)</span></span>
====================
<span data-ttu-id="2e70d-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2e70d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2e70d-106">[Kodu indirme](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2e70d-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span></span>

> <span data-ttu-id="2e70d-107">AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e70d-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="2e70d-108">Paneller genellikle sayfa içinde bildirilen, ancak sunucu tarafı kodu aynı sonucu elde etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2e70d-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="2e70d-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="2e70d-109">Overview</span></span>

<span data-ttu-id="2e70d-110">AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e70d-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="2e70d-111">Paneller genellikle sayfa içinde bildirilen, ancak sunucu tarafı kodu aynı sonucu elde etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2e70d-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="2e70d-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="2e70d-112">Steps</span></span>

<span data-ttu-id="2e70d-113">Accordion denetim sunucu tarafı kodu tüm önemli özellikleri sunar.</span><span class="sxs-lookup"><span data-stu-id="2e70d-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="2e70d-114">Başka şeylerin `Panes` özelliği Accordion olun bölmeleri koleksiyonuna erişim verir.</span><span class="sxs-lookup"><span data-stu-id="2e70d-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="2e70d-115">Tür her bölmesinde `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="2e70d-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="2e70d-116">Bu nedenle, bu tür bir bölme oluşturmak için Önemsiz şöyledir:</span><span class="sxs-lookup"><span data-stu-id="2e70d-116">It is therefore trivial to create such a pane:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

<span data-ttu-id="2e70d-117">`HeaderContainer` Özelliği `AccordionPane` ASP.NET denetimleri bölmesi; üstbilgi bölümünün içinde erişmenizi sağlar `ContentContainer` özelliği `AccordionPane` bölmesinin içerik bölümü için aynı değil.</span><span class="sxs-lookup"><span data-stu-id="2e70d-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="2e70d-118">Bu içerik için bölmeleri eklemek ASP.NET kodu sağlar:</span><span class="sxs-lookup"><span data-stu-id="2e70d-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

<span data-ttu-id="2e70d-119">Son olarak, pane(s) eklenmeli `Panes` Accordion koleksiyonu:</span><span class="sxs-lookup"><span data-stu-id="2e70d-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

<span data-ttu-id="2e70d-120">İki bölme bir Accordion denetimini ekler tam bir sunucu tarafı kodu şöyledir:</span><span class="sxs-lookup"><span data-stu-id="2e70d-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

<span data-ttu-id="2e70d-121">Yalnızca eksik, ASP.NET varlığına bağlı Accordion kendisini öğedir `ScriptManager` denetimi:</span><span class="sxs-lookup"><span data-stu-id="2e70d-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

<span data-ttu-id="2e70d-122">Örnek tamamlamak için tarayıcı için stil bilgilerini Accordion denetiminde başvurulan iki CSS sınıfları sağlar.</span><span class="sxs-lookup"><span data-stu-id="2e70d-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


<span data-ttu-id="2e70d-123">[![Accordion verilerde sunucu tarafı kodu tarafından dinamik olarak eklendi](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2e70d-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span></span>

<span data-ttu-id="2e70d-124">Accordion verilerde sunucu tarafı kodu tarafından dinamik olarak eklendi ([tam boyutlu görüntüyü görüntülemek için tıklatın](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2e70d-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2e70d-125">[Önceki](databinding-to-an-accordion-cs.md)
> [sonraki](databinding-to-an-accordion-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2e70d-125">[Previous](databinding-to-an-accordion-cs.md)
[Next](databinding-to-an-accordion-vb.md)</span></span>
