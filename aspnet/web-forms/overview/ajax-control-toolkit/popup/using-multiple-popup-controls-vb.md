---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: "Birden çok açılan denetimler (VB) kullanarak | Microsoft Docs"
author: wenz
description: "AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar. M kullanmak da mümkündür..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 32e170ebd78a6f849004e789f53c03d9cd40be01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="c9e11-104">Birden çok açılan denetimler (VB) kullanma</span><span class="sxs-lookup"><span data-stu-id="c9e11-104">Using Multiple Popup Controls (VB)</span></span>
====================
<span data-ttu-id="c9e11-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c9e11-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c9e11-106">[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c9e11-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="c9e11-107">AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="c9e11-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="c9e11-108">Bir sayfa birden fazla açılan denetiminde kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="c9e11-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="c9e11-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="c9e11-109">Overview</span></span>

<span data-ttu-id="c9e11-110">AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="c9e11-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="c9e11-111">Bir sayfa birden fazla açılan denetiminde kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="c9e11-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="c9e11-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="c9e11-112">Steps</span></span>

<span data-ttu-id="c9e11-113">ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="c9e11-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="c9e11-114">Ardından, açılan hizmet veren bir panel ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c9e11-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="c9e11-115">Geçerli senaryoda paneli içeren bir `Calendar` denetim.</span><span class="sxs-lookup"><span data-stu-id="c9e11-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="c9e11-116">Takvim Geri göndermeler tarafından neden sayfa yenilenir önlemek için bölmenin içinde yerleştirileceği bir `UpdatePanel` denetimi:</span><span class="sxs-lookup"><span data-stu-id="c9e11-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="c9e11-117">Sayfa iki metin kutusu da içerir.</span><span class="sxs-lookup"><span data-stu-id="c9e11-117">The page also contains two text boxes.</span></span> <span data-ttu-id="c9e11-118">Metin kutusu etkinleştirildikten sonra her metin kutusu için Takvim açılan görünür.</span><span class="sxs-lookup"><span data-stu-id="c9e11-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="c9e11-119">Artık her iki metin kutuları ile genişletmek bir `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="c9e11-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="c9e11-120">`TargetControlID` Özniteliği için genişletici bağlı denetiminin Kimliğini sağlar.</span><span class="sxs-lookup"><span data-stu-id="c9e11-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="c9e11-121">`PopupControlID` Özniteliği açılan paneli Kimliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="c9e11-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="c9e11-122">Bu durumda, her iki Extender'larının aynı panelini Göster, ancak farklı paneller de olasılığı vardır.</span><span class="sxs-lookup"><span data-stu-id="c9e11-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="c9e11-123">Şimdi bir metin alanı içinde tıklattığınızda, bir takvim bir tarih seçin olanak tanıyan alanın altında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c9e11-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="c9e11-124">(Seçilen tarih metin kutularına geri alamazsınız. farklı bir öğreticide ele alınacaktır.)</span><span class="sxs-lookup"><span data-stu-id="c9e11-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="c9e11-125">[![Takvim kullanıcı metin kutusuna tıkladığında görüntülenir](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c9e11-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="c9e11-126">Takvim kullanıcı metin kutusuna tıklattığında görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c9e11-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c9e11-127">[Önceki](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[sonraki](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c9e11-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
