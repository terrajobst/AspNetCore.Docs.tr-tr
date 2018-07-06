---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: (C#) UpdatePanel olmadan bir açılan denetimden gelen geri göndermeleri işleme | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti, PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde, popup tetiklemek için kolay bir yolunu sunar. Su içinde bir geri gönderme gerçekleştiğinde...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 108595a37d6c15a4bd6ddee365ca718969ca9c8c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804041"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a><span data-ttu-id="ae47c-104">(C#) UpdatePanel olmadan bir açılan denetimden gelen geri göndermeleri işleme</span><span class="sxs-lookup"><span data-stu-id="ae47c-104">Handling Postbacks from A Popup Control Without an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="ae47c-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ae47c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ae47c-106">[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ae47c-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span></span>

> <span data-ttu-id="ae47c-107">AJAX Denetim Araç Seti, PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde, popup tetiklemek için kolay bir yolunu sunar.</span><span class="sxs-lookup"><span data-stu-id="ae47c-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="ae47c-108">Bu panelde bir geri gönderme gerçekleşir ve sayfada birkaç panelleri olduğunda hangi panele tıkladı belirlemek zordur.</span><span class="sxs-lookup"><span data-stu-id="ae47c-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="ae47c-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="ae47c-109">Overview</span></span>

<span data-ttu-id="ae47c-110">AJAX Denetim Araç Seti, PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde, popup tetiklemek için kolay bir yolunu sunar.</span><span class="sxs-lookup"><span data-stu-id="ae47c-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="ae47c-111">Bu panelde bir geri gönderme gerçekleşir ve sayfada birkaç panelleri olduğunda hangi panele tıkladı belirlemek zordur.</span><span class="sxs-lookup"><span data-stu-id="ae47c-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="ae47c-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="ae47c-112">Steps</span></span>

<span data-ttu-id="ae47c-113">Kullanırken bir `PopupControl` bir geri gönderme, ancak sahip olmayan bir `UpdatePanel` sayfasında Denetim Araç Seti, hangi istemci öğesi sırayla geri göndermenin neden açılan tetiklediğini belirlemek için bir yol sunmaz.</span><span class="sxs-lookup"><span data-stu-id="ae47c-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="ae47c-114">Ancak küçük ipucunu bu senaryo için geçici bir çözüm sağlar.</span><span class="sxs-lookup"><span data-stu-id="ae47c-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="ae47c-115">İlk olarak, temel kurulum şöyledir: her iki aynı açılan, bir Takvim tetikleyicisi iki metin kutuları.</span><span class="sxs-lookup"><span data-stu-id="ae47c-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="ae47c-116">İki `PopupControlExtenders` metin kutularını ve açılan bir araya getirin.</span><span class="sxs-lookup"><span data-stu-id="ae47c-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="ae47c-117">Gizli form alanına eklemek için temel fikirdir &lt; `form` &gt; başlatılan açılan metin kutusuna tutan öğesi:</span><span class="sxs-lookup"><span data-stu-id="ae47c-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="ae47c-118">Sayfa yüklendiğinde, JavaScript kodu her iki metin kutuları için bir olay işleyicisi ekler: kullanıcı bir metin kutusunda tıkladığında, adını gizli form alanına yazılır:</span><span class="sxs-lookup"><span data-stu-id="ae47c-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

<span data-ttu-id="ae47c-119">Sunucu tarafı kodunda gizli alan değerini okunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae47c-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="ae47c-120">Gizli form alanları değiştirmek için Önemsiz olduğundan, gizli değeri doğrulamak için bir beyaz liste yaklaşım gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ae47c-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="ae47c-121">Doğru metin kutusunu belirlendikten sonra takvimden tarihi üzerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="ae47c-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


<span data-ttu-id="ae47c-122">[![Takvim kullanıcı TextBox'a tıkladığında görünür](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ae47c-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="ae47c-123">Takvim kullanıcı TextBox'a tıkladığında görünür ([tam boyutlu görüntüyü görmek için tıklatın](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ae47c-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="ae47c-124">[![Bir tarihte tıklayarak metin kutusunda yerleştirir](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="ae47c-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="ae47c-125">Bir tarihte tıklayarak onu koyar metin kutusu içinde ([tam boyutlu görüntüyü görmek için tıklatın](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ae47c-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ae47c-126">[Önceki](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [İleri](using-multiple-popup-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ae47c-126">[Previous](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Next](using-multiple-popup-controls-vb.md)</span></span>
