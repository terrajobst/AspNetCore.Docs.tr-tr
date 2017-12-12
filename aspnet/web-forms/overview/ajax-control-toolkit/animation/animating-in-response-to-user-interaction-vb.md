---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: "Kullanıcı etkileşimi (VB) yanıt animasyon | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyon yıldız..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 3219e9d126b3225bfc78d08fb3ac7ef4cc3dca75
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="animating-in-response-to-user-interaction-vb"></a><span data-ttu-id="4bf90-104">Kullanıcı etkileşimi (VB) yanıt animasyon ekleme</span><span class="sxs-lookup"><span data-stu-id="4bf90-104">Animating in Response To User Interaction (VB)</span></span>
====================
<span data-ttu-id="4bf90-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4bf90-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4bf90-106">[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4bf90-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span></span>

> <span data-ttu-id="4bf90-107">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="4bf90-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4bf90-108">Animasyon kullanıcı etkileşimi tarafından örneğin fareyle tıklatarak tetiklenebilir veya otomatik olarak başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4bf90-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="4bf90-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="4bf90-109">Overview</span></span>

<span data-ttu-id="4bf90-110">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="4bf90-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4bf90-111">Animasyon kullanıcı etkileşimi tarafından örneğin fareyle tıklatarak tetiklenebilir veya otomatik olarak başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4bf90-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="4bf90-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="4bf90-112">Steps</span></span>

<span data-ttu-id="4bf90-113">İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:</span><span class="sxs-lookup"><span data-stu-id="4bf90-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

<span data-ttu-id="4bf90-114">Animasyonun bir panel şöyle metin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="4bf90-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

<span data-ttu-id="4bf90-115">İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="4bf90-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

<span data-ttu-id="4bf90-116">Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="4bf90-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

<span data-ttu-id="4bf90-117">İçinde `<Animations>` düğümü, kullanıcı etkileşimi aracılığıyla animasyonu başlatmak için beş yolu vardır (eksik öğesi `<OnLoad>` tüm sayfa tam yüklenmeden silindikten sonra yürütülen):</span><span class="sxs-lookup"><span data-stu-id="4bf90-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="4bf90-118">`<OnClick>`(fare denetimi tıklayın)</span><span class="sxs-lookup"><span data-stu-id="4bf90-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="4bf90-119">`<OnHoverOut>`(fare denetimi terk)</span><span class="sxs-lookup"><span data-stu-id="4bf90-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="4bf90-120">`<OnHoverOver>`(fare gezinen durdurma üzerinde bir denetim `<OnHoverOut>` animasyon)</span><span class="sxs-lookup"><span data-stu-id="4bf90-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="4bf90-121">`<OnMouseOut>`(fare denetim bırakır)</span><span class="sxs-lookup"><span data-stu-id="4bf90-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="4bf90-122">`<OnMouseOver>`(fare gezinen değil durdurma üzerinde bir denetim `<OnMouseOut>` animasyon)</span><span class="sxs-lookup"><span data-stu-id="4bf90-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="4bf90-123">Bu senaryoda, `<OnClick>` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4bf90-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="4bf90-124">Kullanıcı panelde tıklattığında boyutlandırılır ve aynı anda silinerek çıkar.</span><span class="sxs-lookup"><span data-stu-id="4bf90-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


<span data-ttu-id="4bf90-125">[![Animasyonun bir fare tıklatma başlatır](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4bf90-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span></span>

<span data-ttu-id="4bf90-126">Animasyonun bir fare tıklatma başlatır ([tam boyutlu görüntüyü görüntülemek için tıklatın](animating-in-response-to-user-interaction-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4bf90-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4bf90-127">[Önceki](picking-one-animation-out-of-a-list-vb.md)
[sonraki](disabling-actions-during-animation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4bf90-127">[Previous](picking-one-animation-out-of-a-list-vb.md)
[Next](disabling-actions-during-animation-vb.md)</span></span>
