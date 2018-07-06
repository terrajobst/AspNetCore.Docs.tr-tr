---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: (VB) JavaScript'ten bir paneli daraltma ve genişletme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti CollapsiblePanel denetiminde bir panel genişletir ve içeriğini daraltmak ve genişletmek için olanağı sunan bir...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: da1bc6958dba99ffc5ef54fbfbc003bb26050495
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812511"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a><span data-ttu-id="08e53-103">(VB) JavaScript'ten bir paneli daraltma ve genişletme</span><span class="sxs-lookup"><span data-stu-id="08e53-103">Collapsing and Expanding a Panel from JavaScript (VB)</span></span>
====================
<span data-ttu-id="08e53-104">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="08e53-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="08e53-105">[Kodu indir](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="08e53-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span></span>

> <span data-ttu-id="08e53-106">ASP.NET AJAX Denetim Araç Seti CollapsiblePanel denetiminde bir panel genişletir ve içeriğini Daralt ve yeniden genişletin yeteneği sağlar.</span><span class="sxs-lookup"><span data-stu-id="08e53-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="08e53-107">Bu iki eylem, özel bir JavaScript kodundan da tetiklenebilir.</span><span class="sxs-lookup"><span data-stu-id="08e53-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="08e53-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="08e53-108">Overview</span></span>

<span data-ttu-id="08e53-109">ASP.NET AJAX Denetim Araç Seti CollapsiblePanel denetiminde bir panel genişletir ve içeriğini Daralt ve yeniden genişletin yeteneği sağlar.</span><span class="sxs-lookup"><span data-stu-id="08e53-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="08e53-110">Bu iki eylem, özel bir JavaScript kodundan da tetiklenebilir.</span><span class="sxs-lookup"><span data-stu-id="08e53-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="08e53-111">Adımlar</span><span class="sxs-lookup"><span data-stu-id="08e53-111">Steps</span></span>

<span data-ttu-id="08e53-112">İlk olarak yeni bir ASP.NET sayfasında oluşturduğunuz ve `ScriptManager` bir içinde `<form>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="08e53-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="08e53-113">Bu Denetim Araç Seti tarafından gerekli olan ASP.NET AJAX kitaplığı yükler:</span><span class="sxs-lookup"><span data-stu-id="08e53-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="08e53-114">Ardından, bir panel bazı metinleri olan oluşturun, böylece daraltma/genişletme etkisi görülebilir:</span><span class="sxs-lookup"><span data-stu-id="08e53-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="08e53-115">Gördüğünüz gibi burada gösterilen (ve temel bir arka plan rengi ve panelin genişliğini tanımlar) bir CSS sınıfı paneli başvuruyor:</span><span class="sxs-lookup"><span data-stu-id="08e53-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

<span data-ttu-id="08e53-116">`CollapsiblePanelExtender` Denetimi gerektirir `TargetControlID` Araç Seti Daralt veya istek üzerine genişletmek için hangi panel bilebilmesi özniteliği:</span><span class="sxs-lookup"><span data-stu-id="08e53-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

<span data-ttu-id="08e53-117">Ne yazık ki, genişletici şu anda belirli bir API'yi daraltma veya genişletme panel için kullanıma sunmuyor, ancak bazı belgelenmemiş yöntemleri yapar.</span><span class="sxs-lookup"><span data-stu-id="08e53-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="08e53-118">İlk olarak üç HTML düğmesi daraltma veya genişletme bölmenin içeriği için istemci tarafı JavaScript ardından tetikleyecek sayfaya ekleyin:</span><span class="sxs-lookup"><span data-stu-id="08e53-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="08e53-119">İstemci tarafı JavaScript kodunda (kullanmaya `<script type="text/javascript">`), `$find()` gereken yöntemini kullanılacak erişimi `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="08e53-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="08e53-120">`$find("cpe")` Buna bir başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="08e53-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="08e53-121">Üzerinde buradan belirli yöntemler elinizdeki çözüm.</span><span class="sxs-lookup"><span data-stu-id="08e53-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="08e53-122">Yöntem (genişletme) açmak için panel adı verilir `_doOpen()`; aşağıdaki kod uygulayan `doOpen()` işlevi ilk düğmesine tıklandığında çağırılır:</span><span class="sxs-lookup"><span data-stu-id="08e53-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

<span data-ttu-id="08e53-123">Kapatma ya da bir paneli daraltma `_doClose()` yöntemi yürütülmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="08e53-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="08e53-124">Bu nedenle kullanıcı ikinci düğmeye tıkladığında, aşağıdaki JavaScript kodunu çağrılır:</span><span class="sxs-lookup"><span data-stu-id="08e53-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

<span data-ttu-id="08e53-125">Alan üçüncü düğmeye paneli durumunu değiştirir: gelen için genişletilmiş, daraltılmış ve bunun tersi de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="08e53-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="08e53-126">`CollapsiblePanelExtender` Sunan `toggle()` tam olarak yapar yöntemi: panel durumunu tersine çevirir.</span><span class="sxs-lookup"><span data-stu-id="08e53-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="08e53-127">Ancak de mevcuttur başka bir yaklaşım (dahili olarak kullanılan tarafından `toggle()` yöntemi): `get_Collapsed()` yöntemi `CollapsiblePanelExtender()` bize paneli veya daraltılmış durumda gösterir.</span><span class="sxs-lookup"><span data-stu-id="08e53-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="08e53-128">Ya da Genişletilmiş sonra bağlı olarak bu işlev dönüş değeri, masasıdır (`_doOpen()` yöntemi) veya daraltılmış (`_doClose()`) yöntemi:</span><span class="sxs-lookup"><span data-stu-id="08e53-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


<span data-ttu-id="08e53-129">[![Alan üçüncü düğmeye paneli durumunu değiştirir: genişletilmiş ve geri gelen daraltılmış](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="08e53-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="08e53-130">Alan üçüncü düğmeye paneli durumunu değiştirir: genişletilmiş ve geri gelen daraltılmış ([tam boyutlu görüntüyü görmek için tıklatın](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="08e53-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="08e53-131">Önceki</span><span class="sxs-lookup"><span data-stu-id="08e53-131">Previous</span></span>](collapsing-and-expanding-a-panel-from-javascript-cs.md)
