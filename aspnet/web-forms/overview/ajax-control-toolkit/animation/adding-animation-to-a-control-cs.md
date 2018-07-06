---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: (C#) bir denetime animasyon ekleme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Bu öğreticide gösterilmiştir nasıl...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 10e541bc7e8a9a72d27909d7b37cfaf1860fe369
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832956"
---
<a name="adding-animation-to-a-control-c"></a><span data-ttu-id="cad04-104">(C#) bir denetime animasyon ekleme</span><span class="sxs-lookup"><span data-stu-id="cad04-104">Adding Animation to a Control (C#)</span></span>
====================
<span data-ttu-id="cad04-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="cad04-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="cad04-106">[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="cad04-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="cad04-107">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="cad04-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="cad04-108">Bu öğreticide, gibi animasyon ayarlama işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="cad04-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="cad04-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="cad04-109">Overview</span></span>

<span data-ttu-id="cad04-110">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="cad04-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="cad04-111">Bu öğreticide, gibi animasyon ayarlama işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="cad04-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="cad04-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="cad04-112">Steps</span></span>

<span data-ttu-id="cad04-113">İlk adım zamanki dahil etmektir `ScriptManager` sayfasında ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="cad04-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="cad04-114">Bu senaryoda animasyon metin şuna benzer bir panel uygulanır:</span><span class="sxs-lookup"><span data-stu-id="cad04-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="cad04-115">İlişkili bir CSS sınıfı panelinin arka plan rengi ve genişliği tanımlar:</span><span class="sxs-lookup"><span data-stu-id="cad04-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="cad04-116">Ardından, ihtiyacımız `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="cad04-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="cad04-117">Girdikten sonra bir `ID` ve normal `runat="server"`, `TargetControlID` özniteliği örneğimizde panelinde animasyon uygulamak için Denetim ayarlanmalıdır:</span><span class="sxs-lookup"><span data-stu-id="cad04-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="cad04-118">Tüm animasyon bildirimli olarak, ne yazık ki Visual Studio'nun IntelliSense tarafından şu anda tamamen desteklenen XML söz dizimi kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="cad04-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="cad04-119">Kök düğüm `<Animations>;` ne zaman yer animation(s) take(s) belirleyen çeşitli etkinliklerde bu düğümde izin verilir:</span><span class="sxs-lookup"><span data-stu-id="cad04-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="cad04-120">`OnClick` (fare tıklatın)</span><span class="sxs-lookup"><span data-stu-id="cad04-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="cad04-121">`OnHoverOut` (fare bir denetim ayrıldığında)</span><span class="sxs-lookup"><span data-stu-id="cad04-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="cad04-122">`OnHoverOver` (fare bir denetimin üzerine geldiğinde durdurma `OnHoverOut` animasyon)</span><span class="sxs-lookup"><span data-stu-id="cad04-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="cad04-123">`OnLoad` (sayfanın ne zaman yüklendi)</span><span class="sxs-lookup"><span data-stu-id="cad04-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="cad04-124">`OnMouseOut` (fare bir denetim ayrıldığında)</span><span class="sxs-lookup"><span data-stu-id="cad04-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="cad04-125">`OnMouseOver` (fare bir denetimin üzerine geldiğinde değil durdurma `OnMouseOut` animasyon)</span><span class="sxs-lookup"><span data-stu-id="cad04-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="cad04-126">Framework, animasyon, her biri kendi XML öğesi tarafından temsil edilen bir dizisiyle gelir.</span><span class="sxs-lookup"><span data-stu-id="cad04-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="cad04-127">Seçimi şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="cad04-127">Here is a selection:</span></span>

- <span data-ttu-id="cad04-128">`<Color>` (bir rengi değiştirme)</span><span class="sxs-lookup"><span data-stu-id="cad04-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="cad04-129">`<FadeIn>` (yavaş giriş)</span><span class="sxs-lookup"><span data-stu-id="cad04-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="cad04-130">`<FadeOut>` (yavaş çıkış)</span><span class="sxs-lookup"><span data-stu-id="cad04-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="cad04-131">`<Property>` (bir denetimin özelliğini değiştirme)</span><span class="sxs-lookup"><span data-stu-id="cad04-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="cad04-132">`<Pulse>` (pulsating)</span><span class="sxs-lookup"><span data-stu-id="cad04-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="cad04-133">`<Resize>` (boyutunu değiştirme)</span><span class="sxs-lookup"><span data-stu-id="cad04-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="cad04-134">`<Scale>` (orantılı olarak boyutunu değiştirme)</span><span class="sxs-lookup"><span data-stu-id="cad04-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="cad04-135">Bu örnekte, panel Kıs. Animasyon 1.5 saniye almalı (`Duration` özniteliği), 24 çerçeveler (animasyon adımlar) saniye başına görüntüleme (`Fps` attributs).</span><span class="sxs-lookup"><span data-stu-id="cad04-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="cad04-136">İçin tam biçimlendirmesi şöyledir `AnimationExtender` denetimi:</span><span class="sxs-lookup"><span data-stu-id="cad04-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="cad04-137">Bu komut dosyasını çalıştırdığınızda, paneli görüntülenir ve bir buçuk saniyeler içinde silinerek çıkar.</span><span class="sxs-lookup"><span data-stu-id="cad04-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="cad04-138">[![Panel Soluklaşan](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cad04-138">[![The panel is fading out](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="cad04-139">Panel Soluklaşan ([tam boyutlu görüntüyü görmek için tıklatın](adding-animation-to-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="cad04-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="cad04-140">Next</span><span class="sxs-lookup"><span data-stu-id="cad04-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
