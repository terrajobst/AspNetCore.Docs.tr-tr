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
<a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="884ce-104">Bir Accordion bölmesi (VB) dinamik olarak ekleme</span><span class="sxs-lookup"><span data-stu-id="884ce-104">Dynamically Adding An Accordion Pane (VB)</span></span>
====================
<span data-ttu-id="884ce-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="884ce-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="884ce-106">[Kodu indirme](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="884ce-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="884ce-107">AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="884ce-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="884ce-108">Paneller genellikle sayfa içinde bildirilen, ancak sunucu tarafı kodu aynı sonucu elde etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="884ce-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="884ce-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="884ce-109">Overview</span></span>

<span data-ttu-id="884ce-110">AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="884ce-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="884ce-111">Paneller genellikle sayfa içinde bildirilen, ancak sunucu tarafı kodu aynı sonucu elde etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="884ce-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="884ce-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="884ce-112">Steps</span></span>

<span data-ttu-id="884ce-113">Accordion denetim sunucu tarafı kodu tüm önemli özellikleri sunar.</span><span class="sxs-lookup"><span data-stu-id="884ce-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="884ce-114">Başka şeylerin `Panes` özelliği Accordion olun bölmeleri koleksiyonuna erişim verir.</span><span class="sxs-lookup"><span data-stu-id="884ce-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="884ce-115">Tür her bölmesinde `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="884ce-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="884ce-116">Bu nedenle, bu tür bir bölme oluşturmak için Önemsiz şöyledir:</span><span class="sxs-lookup"><span data-stu-id="884ce-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="884ce-117">`HeaderContainer` Özelliği `AccordionPane` ASP.NET denetimleri bölmesi; üstbilgi bölümünün içinde erişmenizi sağlar `ContentContainer` özelliği `AccordionPane` bölmesinin içerik bölümü için aynı değil.</span><span class="sxs-lookup"><span data-stu-id="884ce-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="884ce-118">Bu içerik için bölmeleri eklemek ASP.NET kodu sağlar:</span><span class="sxs-lookup"><span data-stu-id="884ce-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="884ce-119">Son olarak, pane(s) eklenmeli `Panes` Accordion koleksiyonu:</span><span class="sxs-lookup"><span data-stu-id="884ce-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="884ce-120">İki bölme bir Accordion denetimini ekler tam bir sunucu tarafı kodu şöyledir:</span><span class="sxs-lookup"><span data-stu-id="884ce-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="884ce-121">Yalnızca eksik, ASP.NET varlığına bağlı Accordion kendisini öğedir `ScriptManager` denetimi:</span><span class="sxs-lookup"><span data-stu-id="884ce-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="884ce-122">Örnek tamamlamak için tarayıcı için stil bilgilerini Accordion denetiminde başvurulan iki CSS sınıfları sağlar.</span><span class="sxs-lookup"><span data-stu-id="884ce-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


<span data-ttu-id="884ce-123">[![Accordion verilerde sunucu tarafı kodu tarafından dinamik olarak eklendi](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="884ce-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="884ce-124">Accordion verilerde sunucu tarafı kodu tarafından dinamik olarak eklendi ([tam boyutlu görüntüyü görüntülemek için tıklatın](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="884ce-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="884ce-125">Önceki</span><span class="sxs-lookup"><span data-stu-id="884ce-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
