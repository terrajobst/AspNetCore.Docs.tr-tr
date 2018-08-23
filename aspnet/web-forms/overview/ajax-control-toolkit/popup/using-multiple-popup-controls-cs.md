---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Birden çok açılan denetim (C#) kullanarak | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti, PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde, popup tetiklemek için kolay bir yolunu sunar. M kullanmak da mümkündür...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8cf7f929b696240e6805a74fb576ad56738491fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753372"
---
<a name="using-multiple-popup-controls-c"></a><span data-ttu-id="130cc-104">Birden çok açılan denetim (C#) kullanma</span><span class="sxs-lookup"><span data-stu-id="130cc-104">Using Multiple Popup Controls (C#)</span></span>
====================
<span data-ttu-id="130cc-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="130cc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="130cc-106">[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="130cc-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span></span>

> <span data-ttu-id="130cc-107">AJAX Denetim Araç Seti, PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde, popup tetiklemek için kolay bir yolunu sunar.</span><span class="sxs-lookup"><span data-stu-id="130cc-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="130cc-108">Tek bir sayfada birden çok açılan denetim kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="130cc-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="130cc-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="130cc-109">Overview</span></span>

<span data-ttu-id="130cc-110">AJAX Denetim Araç Seti, PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde, popup tetiklemek için kolay bir yolunu sunar.</span><span class="sxs-lookup"><span data-stu-id="130cc-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="130cc-111">Tek bir sayfada birden çok açılan denetim kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="130cc-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="130cc-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="130cc-112">Steps</span></span>

<span data-ttu-id="130cc-113">ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="130cc-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="130cc-114">Ardından, açılan menüsü hizmet veren bir panel ekleme.</span><span class="sxs-lookup"><span data-stu-id="130cc-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="130cc-115">Geçerli senaryosunda, panele içeren bir `Calendar` denetimi.</span><span class="sxs-lookup"><span data-stu-id="130cc-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="130cc-116">Takvimin Geri göndermeler tarafından neden sayfa yenilenir önlemek için panel içinde konur bir `UpdatePanel` denetimi:</span><span class="sxs-lookup"><span data-stu-id="130cc-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="130cc-117">Sayfa, iki metin kutuları da içerir.</span><span class="sxs-lookup"><span data-stu-id="130cc-117">The page also contains two text boxes.</span></span> <span data-ttu-id="130cc-118">Metin kutusu etkinleştirildikten sonra her bir metin kutusu için Takvim açılır görünmesi gereken.</span><span class="sxs-lookup"><span data-stu-id="130cc-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="130cc-119">Her iki metin kutuları artık genişleten bir `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="130cc-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="130cc-120">`TargetControlID` Öznitelik kimliği için genişletici bağlı denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="130cc-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="130cc-121">`PopupControlID` Özniteliği açılır paneli Kimliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="130cc-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="130cc-122">Bu durumda, her iki Genişleticileri aynı paneli gösterir, ancak farklı panellerin yanı olasılığı vardır.</span><span class="sxs-lookup"><span data-stu-id="130cc-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="130cc-123">Artık bir metin alanı içinde tıklattığınızda, bir takvim tarihi seçmenize olanak sağlar alanının altında görünür.</span><span class="sxs-lookup"><span data-stu-id="130cc-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="130cc-124">(Seçilen tarihten metin kutularına geri alamazsınız. farklı bir öğreticide ele alınacaktır.)</span><span class="sxs-lookup"><span data-stu-id="130cc-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="130cc-125">[![Takvim kullanıcı TextBox'a tıkladığında görünür](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="130cc-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="130cc-126">Takvim kullanıcı TextBox'a tıkladığında görünür ([tam boyutlu görüntüyü görmek için tıklatın](using-multiple-popup-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="130cc-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="130cc-127">Next</span><span class="sxs-lookup"><span data-stu-id="130cc-127">Next</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
