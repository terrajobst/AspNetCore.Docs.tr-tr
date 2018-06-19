---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: JavaScript (VB) panelinden genişletme ve daraltma | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti CollapsiblePanel denetiminde bir panel genişletir ve içeriğini daraltmak ve genişletmek yeteneği sağlar bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 5cf61cd0d8204a5405ba62cd3884d66ccb21968b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873124"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a><span data-ttu-id="3301b-103">JavaScript (VB) panelinden genişletme ve daraltma</span><span class="sxs-lookup"><span data-stu-id="3301b-103">Collapsing and Expanding a Panel from JavaScript (VB)</span></span>
====================
<span data-ttu-id="3301b-104">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3301b-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3301b-105">[Kodu indirme](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3301b-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span></span>

> <span data-ttu-id="3301b-106">ASP.NET AJAX Denetim Araç Seti CollapsiblePanel denetiminde bir panel genişletir ve içeriğini daraltın ve tekrar genişletme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="3301b-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="3301b-107">Bu iki eylem ayrıca özel JavaScript kodundan tetiklenebilir.</span><span class="sxs-lookup"><span data-stu-id="3301b-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="3301b-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="3301b-108">Overview</span></span>

<span data-ttu-id="3301b-109">ASP.NET AJAX Denetim Araç Seti CollapsiblePanel denetiminde bir panel genişletir ve içeriğini daraltın ve tekrar genişletme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="3301b-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="3301b-110">Bu iki eylem ayrıca özel JavaScript kodundan tetiklenebilir.</span><span class="sxs-lookup"><span data-stu-id="3301b-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="3301b-111">Adımlar</span><span class="sxs-lookup"><span data-stu-id="3301b-111">Steps</span></span>

<span data-ttu-id="3301b-112">İlk olarak, yeni bir ASP.NET sayfası oluşturmak ve dahil etmek `ScriptManager` bir içinde `<form>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="3301b-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="3301b-113">Bu Denetim Araç Seti tarafından gerekli olan ASP.NET AJAX kitaplığı yükler:</span><span class="sxs-lookup"><span data-stu-id="3301b-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="3301b-114">Ardından, bir panel bazı metinle oluşturun, böylece daraltma ve genişletme etkisi görülebilir:</span><span class="sxs-lookup"><span data-stu-id="3301b-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="3301b-115">Gördüğünüz gibi bölmenin burada gösterilen (ve temel bir arka plan rengi ve bölmenin genişliğini tanımlar) bir CSS sınıfı başvuruyor:</span><span class="sxs-lookup"><span data-stu-id="3301b-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

<span data-ttu-id="3301b-116">`CollapsiblePanelExtender` Denetimi gerektirir `TargetControlID` daraltın veya istek üzerine genişletmek için hangi Masası Araç Seti bilir böylece özniteliği:</span><span class="sxs-lookup"><span data-stu-id="3301b-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

<span data-ttu-id="3301b-117">Ne yazık ki, genişletici şu anda belirli bir API daraltma veya paneli genişletme için kullanıma sunmuyor, ancak bazı belgelenmemiş yöntemleri yapar.</span><span class="sxs-lookup"><span data-stu-id="3301b-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="3301b-118">Öncelikle, üç HTML düğmesi, ardından daraltmak veya bölmenin içeriği genişletmek için istemci tarafı JavaScript tetikleyecek sayfasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="3301b-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="3301b-119">İstemci tarafı JavaScript kodu (kullanmaya `<script type="text/javascript">`), `$find()` yönteminin gerekir kullanılacak erişimi `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="3301b-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="3301b-120">`$find("cpe")` bir başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="3301b-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="3301b-121">Üzerinde buradan belirli yöntemler elinizdeki çözüm.</span><span class="sxs-lookup"><span data-stu-id="3301b-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="3301b-122">Yöntemi (genişletme) açmak için bölmenin adlı `_doOpen()`; aşağıdaki kod uygulayan `doOpen()` işlevini çağırdı ilk düğme tıklatıldığında:</span><span class="sxs-lookup"><span data-stu-id="3301b-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

<span data-ttu-id="3301b-123">Kapatma veya paneli daraltma için `_doClose()` yöntemi yürütülmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="3301b-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="3301b-124">Bu nedenle kullanıcı ikinci düğmesine tıkladığında, şu JavaScript kodunu çağrılır:</span><span class="sxs-lookup"><span data-stu-id="3301b-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

<span data-ttu-id="3301b-125">Üçüncü düğme paneli durumunu değiştirir: gelen için genişletilmiş, daraltılmış ve tersi.</span><span class="sxs-lookup"><span data-stu-id="3301b-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="3301b-126">`CollapsiblePanelExtender` Sunan `toggle()` tam olarak, yapan yöntemi: paneli durumunu tersine çevirir.</span><span class="sxs-lookup"><span data-stu-id="3301b-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="3301b-127">Ancak bulunmaktadır başka bir yaklaşım (dahili olarak kullanılan tarafından `toggle()` yöntemi): `get_Collapsed()` yöntemi `CollapsiblePanelExtender()` bize panel daraltılmış durumda değil söyler.</span><span class="sxs-lookup"><span data-stu-id="3301b-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="3301b-128">Ya da Genişletilmiş sonra bu işlevin dönüş değerini bağlı olarak, panosudur (`_doOpen()` yöntemi) veya daraltılmış (`_doClose()`) yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3301b-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


<span data-ttu-id="3301b-129">[![Üçüncü düğme paneli durumunu değiştirir: gelen genişletilmiş ve geri daraltılmış](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3301b-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="3301b-130">Üçüncü düğme paneli durumunu değiştirir: gelen genişletilmiş ve geri daraltılmış ([tam boyutlu görüntüyü görüntülemek için tıklatın](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3301b-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3301b-131">Önceki</span><span class="sxs-lookup"><span data-stu-id="3301b-131">Previous</span></span>](collapsing-and-expanding-a-panel-from-javascript-cs.md)
