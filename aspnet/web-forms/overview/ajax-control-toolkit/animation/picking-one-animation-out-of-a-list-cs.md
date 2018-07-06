---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Bir liste (C#) animasyon seçme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Framework ayrıca ver...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 7ef2c5d37c32150d17b798e22290f33b5619a14c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834678"
---
<a name="picking-one-animation-out-of-a-list-c"></a><span data-ttu-id="2ca81-104">Bir liste (C#) animasyon seçme</span><span class="sxs-lookup"><span data-stu-id="2ca81-104">Picking One Animation Out Of a List (C#)</span></span>
====================
<span data-ttu-id="2ca81-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2ca81-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2ca81-106">[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2ca81-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span></span>

> <span data-ttu-id="2ca81-107">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="2ca81-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2ca81-108">Framework, bir listeden animasyon animasyon, bazı JavaScript kodları değerlendirmesine bağlı olarak çekmek Programcı de sağlar.</span><span class="sxs-lookup"><span data-stu-id="2ca81-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="2ca81-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="2ca81-109">Overview</span></span>

<span data-ttu-id="2ca81-110">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="2ca81-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2ca81-111">Framework, bir listeden animasyon animasyon, bazı JavaScript kodları değerlendirmesine bağlı olarak çekmek Programcı de sağlar.</span><span class="sxs-lookup"><span data-stu-id="2ca81-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="2ca81-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="2ca81-112">Steps</span></span>

<span data-ttu-id="2ca81-113">İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:</span><span class="sxs-lookup"><span data-stu-id="2ca81-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

<span data-ttu-id="2ca81-114">Animasyonun bir panel şuna benzer metin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="2ca81-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

<span data-ttu-id="2ca81-115">İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="2ca81-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

<span data-ttu-id="2ca81-116">Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="2ca81-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

<span data-ttu-id="2ca81-117">İçinde `<Animations>` düğümü, kullanım `<OnLoad>` animasyonları sayfanın tam yüklü silindikten sonra çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="2ca81-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="2ca81-118">Normal animasyonları birini yerine `<Case>` öğesi oyuna gelir.</span><span class="sxs-lookup"><span data-stu-id="2ca81-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="2ca81-119">Kendi SelectScript özniteliğinin değerini değerlendirilir; dönüş değeri sayısal olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2ca81-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="2ca81-120">Bu sayı, içinde subanimations birine bağlı olarak &lt;çalışması&gt; yürütülür.</span><span class="sxs-lookup"><span data-stu-id="2ca81-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="2ca81-121">Denetim Araç Seti SelectScript 2 olarak değerlendirilirse, üçüncü animasyon içinde örneği için çalışan &lt;çalışması&gt; (sayım 0 başlar).</span><span class="sxs-lookup"><span data-stu-id="2ca81-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="2ca81-122">Aşağıdaki biçimlendirmede üç subanimations tanımlar: genişliğini yeniden boyutlandırma, yükseklik yeniden boyutlandırma ve yavaş çıkış. JavaScript kodu (`Math.floor(3 * Math.random())`) üç animasyonları birini çalıştırın, ardından 0. ve 2 arasında bir sayı seçer:</span><span class="sxs-lookup"><span data-stu-id="2ca81-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]


<span data-ttu-id="2ca81-123">[![Olası üç animasyonları birini: panel geniş alır](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2ca81-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span></span>

<span data-ttu-id="2ca81-124">Olası üç animasyonları birini: panel geniş alır ([tam boyutlu görüntüyü görmek için tıklatın](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2ca81-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2ca81-125">[Önceki](animation-depending-on-a-condition-cs.md)
> [İleri](animating-in-response-to-user-interaction-cs.md)</span><span class="sxs-lookup"><span data-stu-id="2ca81-125">[Previous](animation-depending-on-a-condition-cs.md)
[Next](animating-in-response-to-user-interaction-cs.md)</span></span>
