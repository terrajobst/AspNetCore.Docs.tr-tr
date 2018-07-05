---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: (VB) kullanıcı etkileşimine karşılık olarak animasyon ekleme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyonları yıldız...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: d3a118d3dc44dd74780fafd00d139160f8fc3bc5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366883"
---
<a name="animating-in-response-to-user-interaction-vb"></a><span data-ttu-id="133d7-104">(VB) kullanıcı etkileşimine karşılık olarak animasyon ekleme</span><span class="sxs-lookup"><span data-stu-id="133d7-104">Animating in Response To User Interaction (VB)</span></span>
====================
<span data-ttu-id="133d7-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="133d7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="133d7-106">[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="133d7-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span></span>

> <span data-ttu-id="133d7-107">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="133d7-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="133d7-108">Animasyonların otomatik olarak başlayabilir veya kullanıcı etkileşimi tarafından örneğin fareyle tıklatarak tetiklenebilir.</span><span class="sxs-lookup"><span data-stu-id="133d7-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="133d7-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="133d7-109">Overview</span></span>

<span data-ttu-id="133d7-110">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="133d7-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="133d7-111">Animasyonların otomatik olarak başlayabilir veya kullanıcı etkileşimi tarafından örneğin fareyle tıklatarak tetiklenebilir.</span><span class="sxs-lookup"><span data-stu-id="133d7-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="133d7-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="133d7-112">Steps</span></span>

<span data-ttu-id="133d7-113">İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:</span><span class="sxs-lookup"><span data-stu-id="133d7-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

<span data-ttu-id="133d7-114">Animasyonun bir panel şuna benzer metin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="133d7-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

<span data-ttu-id="133d7-115">İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="133d7-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

<span data-ttu-id="133d7-116">Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="133d7-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

<span data-ttu-id="133d7-117">İçinde `<Animations>` düğümü, kullanıcı etkileşimi aracılığıyla animasyonu başlatmak için beş yol vardır (eksik öğe `<OnLoad>` sayfanın tamamını tam yüklü silindikten sonra yürütülen):</span><span class="sxs-lookup"><span data-stu-id="133d7-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="133d7-118">`<OnClick>` (Denetim fare tıklatın)</span><span class="sxs-lookup"><span data-stu-id="133d7-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="133d7-119">`<OnHoverOut>` (fare denetimi terk)</span><span class="sxs-lookup"><span data-stu-id="133d7-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="133d7-120">`<OnHoverOver>` (fare durdurulurken bir denetimin üzerine geldiğinde `<OnHoverOut>` animasyon)</span><span class="sxs-lookup"><span data-stu-id="133d7-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="133d7-121">`<OnMouseOut>` (fare, denetimin bırakır)</span><span class="sxs-lookup"><span data-stu-id="133d7-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="133d7-122">`<OnMouseOver>` (fare değil durdurulurken bir denetimin üzerine geldiğinde `<OnMouseOut>` animasyon)</span><span class="sxs-lookup"><span data-stu-id="133d7-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="133d7-123">Bu senaryoda, `<OnClick>` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="133d7-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="133d7-124">Kullanıcı panelinde tıkladığında boyutlandırılır ve aynı anda silinerek çıkar.</span><span class="sxs-lookup"><span data-stu-id="133d7-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


<span data-ttu-id="133d7-125">[![Fare tıklatın animasyonu başlatır](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="133d7-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span></span>

<span data-ttu-id="133d7-126">Animasyonun bir fare tıklaması başlar ([tam boyutlu görüntüyü görmek için tıklatın](animating-in-response-to-user-interaction-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="133d7-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="133d7-127">[Önceki](picking-one-animation-out-of-a-list-vb.md)
> [İleri](disabling-actions-during-animation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="133d7-127">[Previous](picking-one-animation-out-of-a-list-vb.md)
[Next](disabling-actions-during-animation-vb.md)</span></span>
