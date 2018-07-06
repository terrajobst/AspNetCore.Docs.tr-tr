---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Birden çok açılan denetim (VB) kullanarak | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti, PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde, popup tetiklemek için kolay bir yolunu sunar. M kullanmak da mümkündür...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 5a2be36ecf17a95d53e5e912ba0a90113f70f2fb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807641"
---
<a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="6aac0-104">Birden çok açılan denetim (VB) kullanma</span><span class="sxs-lookup"><span data-stu-id="6aac0-104">Using Multiple Popup Controls (VB)</span></span>
====================
<span data-ttu-id="6aac0-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6aac0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6aac0-106">[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6aac0-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="6aac0-107">AJAX Denetim Araç Seti, PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde, popup tetiklemek için kolay bir yolunu sunar.</span><span class="sxs-lookup"><span data-stu-id="6aac0-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="6aac0-108">Tek bir sayfada birden çok açılan denetim kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6aac0-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="6aac0-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="6aac0-109">Overview</span></span>

<span data-ttu-id="6aac0-110">AJAX Denetim Araç Seti, PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde, popup tetiklemek için kolay bir yolunu sunar.</span><span class="sxs-lookup"><span data-stu-id="6aac0-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="6aac0-111">Tek bir sayfada birden çok açılan denetim kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6aac0-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="6aac0-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="6aac0-112">Steps</span></span>

<span data-ttu-id="6aac0-113">ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="6aac0-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="6aac0-114">Ardından, açılan menüsü hizmet veren bir panel ekleme.</span><span class="sxs-lookup"><span data-stu-id="6aac0-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="6aac0-115">Geçerli senaryosunda, panele içeren bir `Calendar` denetimi.</span><span class="sxs-lookup"><span data-stu-id="6aac0-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="6aac0-116">Takvimin Geri göndermeler tarafından neden sayfa yenilenir önlemek için panel içinde konur bir `UpdatePanel` denetimi:</span><span class="sxs-lookup"><span data-stu-id="6aac0-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="6aac0-117">Sayfa, iki metin kutuları da içerir.</span><span class="sxs-lookup"><span data-stu-id="6aac0-117">The page also contains two text boxes.</span></span> <span data-ttu-id="6aac0-118">Metin kutusu etkinleştirildikten sonra her bir metin kutusu için Takvim açılır görünmesi gereken.</span><span class="sxs-lookup"><span data-stu-id="6aac0-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="6aac0-119">Her iki metin kutuları artık genişleten bir `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="6aac0-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="6aac0-120">`TargetControlID` Öznitelik kimliği için genişletici bağlı denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="6aac0-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="6aac0-121">`PopupControlID` Özniteliği açılır paneli Kimliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="6aac0-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="6aac0-122">Bu durumda, her iki Genişleticileri aynı paneli gösterir, ancak farklı panellerin yanı olasılığı vardır.</span><span class="sxs-lookup"><span data-stu-id="6aac0-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="6aac0-123">Artık bir metin alanı içinde tıklattığınızda, bir takvim tarihi seçmenize olanak sağlar alanının altında görünür.</span><span class="sxs-lookup"><span data-stu-id="6aac0-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="6aac0-124">(Seçilen tarihten metin kutularına geri alamazsınız. farklı bir öğreticide ele alınacaktır.)</span><span class="sxs-lookup"><span data-stu-id="6aac0-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="6aac0-125">[![Takvim kullanıcı TextBox'a tıkladığında görünür](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6aac0-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="6aac0-126">Takvim kullanıcı TextBox'a tıkladığında görünür ([tam boyutlu görüntüyü görmek için tıklatın](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6aac0-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6aac0-127">[Önceki](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [İleri](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6aac0-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
