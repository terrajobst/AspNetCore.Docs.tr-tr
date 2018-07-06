---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: (C#) kullanıcı etkileşimine karşılık olarak animasyon ekleme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyonları yıldız...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e8ebf5ec7fc0875e0eb43923321513bf0a08899
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826137"
---
<a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="3a28d-104">(C#) kullanıcı etkileşimine karşılık olarak animasyon ekleme</span><span class="sxs-lookup"><span data-stu-id="3a28d-104">Animating in Response To User Interaction (C#)</span></span>
====================
<span data-ttu-id="3a28d-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3a28d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3a28d-106">[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="3a28d-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="3a28d-107">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="3a28d-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3a28d-108">Animasyonların otomatik olarak başlayabilir veya kullanıcı etkileşimi tarafından örneğin fareyle tıklatarak tetiklenebilir.</span><span class="sxs-lookup"><span data-stu-id="3a28d-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="3a28d-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="3a28d-109">Overview</span></span>

<span data-ttu-id="3a28d-110">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="3a28d-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3a28d-111">Animasyonların otomatik olarak başlayabilir veya kullanıcı etkileşimi tarafından örneğin fareyle tıklatarak tetiklenebilir.</span><span class="sxs-lookup"><span data-stu-id="3a28d-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="3a28d-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="3a28d-112">Steps</span></span>

<span data-ttu-id="3a28d-113">İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:</span><span class="sxs-lookup"><span data-stu-id="3a28d-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="3a28d-114">Animasyonun bir panel şuna benzer metin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="3a28d-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="3a28d-115">İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="3a28d-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="3a28d-116">Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="3a28d-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="3a28d-117">İçinde `<Animations>` düğümü, kullanıcı etkileşimi aracılığıyla animasyonu başlatmak için beş yol vardır (eksik öğe `<OnLoad>` sayfanın tamamını tam yüklü silindikten sonra yürütülen):</span><span class="sxs-lookup"><span data-stu-id="3a28d-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="3a28d-118">`<OnClick>` (Denetim fare tıklatın)</span><span class="sxs-lookup"><span data-stu-id="3a28d-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="3a28d-119">`<OnHoverOut>` (fare denetimi terk)</span><span class="sxs-lookup"><span data-stu-id="3a28d-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="3a28d-120">`<OnHoverOver>` (fare durdurulurken bir denetimin üzerine geldiğinde `<OnHoverOut>` animasyon)</span><span class="sxs-lookup"><span data-stu-id="3a28d-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="3a28d-121">`<OnMouseOut>` (fare, denetimin bırakır)</span><span class="sxs-lookup"><span data-stu-id="3a28d-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="3a28d-122">`<OnMouseOver>` (fare değil durdurulurken bir denetimin üzerine geldiğinde `<OnMouseOut>` animasyon)</span><span class="sxs-lookup"><span data-stu-id="3a28d-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="3a28d-123">Bu senaryoda, `<OnClick>` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3a28d-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="3a28d-124">Kullanıcı panelinde tıkladığında boyutlandırılır ve aynı anda silinerek çıkar.</span><span class="sxs-lookup"><span data-stu-id="3a28d-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


<span data-ttu-id="3a28d-125">[![Fare tıklatın animasyonu başlatır](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3a28d-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="3a28d-126">Animasyonun bir fare tıklaması başlar ([tam boyutlu görüntüyü görmek için tıklatın](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3a28d-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3a28d-127">[Önceki](picking-one-animation-out-of-a-list-cs.md)
> [İleri](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="3a28d-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
