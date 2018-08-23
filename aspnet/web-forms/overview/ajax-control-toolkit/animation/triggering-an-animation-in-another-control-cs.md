---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: (C#) başka bir denetimde animasyon tetikleme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Genellikle, başlatma bir...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ad6dfecf71a7577215e43222a8788e5c48d0c4c2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753794"
---
<a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="9e2e9-104">(C#) başka bir denetimde animasyon tetikleme</span><span class="sxs-lookup"><span data-stu-id="9e2e9-104">Triggering an Animation in another Control (C#)</span></span>
====================
<span data-ttu-id="9e2e9-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9e2e9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9e2e9-106">[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="9e2e9-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="9e2e9-107">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9e2e9-108">Bir animasyonu başlatmak, aynı denetimi ile kullanıcı etkileşimi tarafından genel olarak, tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="9e2e9-109">Ancak aynı zamanda başka bir denetimde bir denetimi ve sonra animasyon ile etkileşim kurmak mümkün.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="9e2e9-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="9e2e9-110">Overview</span></span>

<span data-ttu-id="9e2e9-111">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9e2e9-112">Bir animasyonu başlatmak, aynı denetimi ile kullanıcı etkileşimi tarafından genel olarak, tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="9e2e9-113">Ancak aynı zamanda başka bir denetimde bir denetimi ve sonra animasyon ile etkileşim kurmak mümkün.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="9e2e9-114">Adımlar</span><span class="sxs-lookup"><span data-stu-id="9e2e9-114">Steps</span></span>

<span data-ttu-id="9e2e9-115">İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:</span><span class="sxs-lookup"><span data-stu-id="9e2e9-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="9e2e9-116">Animasyonun bir panel şuna benzer metin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="9e2e9-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="9e2e9-117">İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="9e2e9-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="9e2e9-118">Panel animasyonu başlatmak için bir HTML düğmesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="9e2e9-119">Unutmayın `<input type="button" />` üzerinden favoured `<asp:Button />` beri kullanıcı bu düğmeye tıkladığında bir geri gönderme istiyoruz değil.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="9e2e9-120">Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="9e2e9-121">Ayarlamak önemlidir `TargetControlID` kimliğine (öğesi animasyonun tetiklenmesi), düğmenin paneli (animasyon uygulanan öğesi) kimliği için</span><span class="sxs-lookup"><span data-stu-id="9e2e9-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="9e2e9-122">İçinde `<Animations>` düğümü, her zamanki şekilde Yerleştir animasyonları.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="9e2e9-123">Bunları değiştirmek panelin kolaylaştırmak için düğmesini ayarlayın `AnimationTarget` her animasyon öğesi içinde özniteliği `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="9e2e9-124">Değeri `AnimationTarget` paneli Elbette kimliğidir.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="9e2e9-125">Bu şekilde, animasyon tetikleme düğmesiyle değil paneli ile gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="9e2e9-126">İşte `AnimationExtender` biçimlendirme bu senaryo için:</span><span class="sxs-lookup"><span data-stu-id="9e2e9-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="9e2e9-127">Tek tek animasyonları göründüğü özel sıra unutmayın.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="9e2e9-128">İlk olarak, animasyon çalıştırıldıktan sonra düğme devre dışı.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="9e2e9-129">Olduğundan hiçbir `AnimationTarget` özniteliğini `<EnableAction>` öğesi, animasyon, kaynak denetimine uygulanır: düğme.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="9e2e9-130">Sonraki iki animasyon adımları parallelly gerçekleştirilebilmesi (`<Parallel>` öğesi).</span><span class="sxs-lookup"><span data-stu-id="9e2e9-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="9e2e9-131">Her ikisi de kendi `AnimationTarget` öznitelikleri kümesine `"Panel1"`, bu nedenle panelini düğme animasyon ekleme.</span><span class="sxs-lookup"><span data-stu-id="9e2e9-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="9e2e9-132">[![Bir fare tıklaması düğmesine panelinde animasyonun başlatır](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9e2e9-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="9e2e9-133">Bir fare tıklaması düğmesine panelinde animasyonun başlar ([tam boyutlu görüntüyü görmek için tıklatın](triggering-an-animation-in-another-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9e2e9-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9e2e9-134">[Önceki](disabling-actions-during-animation-cs.md)
> [İleri](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="9e2e9-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
