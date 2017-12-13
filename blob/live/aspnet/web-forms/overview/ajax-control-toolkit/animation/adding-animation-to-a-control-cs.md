---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Animasyon denetimi (C#) ekleme | Microsoft Docs
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Bu öğreticide gösterilmiştir nasıl..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7016ae3c92c665136579a8588818e6e4179a102a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-animation-to-a-control-c"></a><span data-ttu-id="0c611-104">Bir denetime (C#) animasyon ekleme</span><span class="sxs-lookup"><span data-stu-id="0c611-104">Adding Animation to a Control (C#)</span></span>
====================
<span data-ttu-id="0c611-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0c611-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0c611-106">[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0c611-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="0c611-107">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="0c611-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="0c611-108">Bu öğretici, bu tür animasyonu ayarlamak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="0c611-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="0c611-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="0c611-109">Overview</span></span>

<span data-ttu-id="0c611-110">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="0c611-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="0c611-111">Bu öğretici, bu tür animasyonu ayarlamak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="0c611-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="0c611-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="0c611-112">Steps</span></span>

<span data-ttu-id="0c611-113">İlk adım her zamanki gibi eklemektir `ScriptManager` sayfasındaki ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="0c611-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="0c611-114">Bu senaryoda animasyon şöyle metin panelinin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="0c611-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="0c611-115">İlişkili CSS sınıfı panelinin arka plan rengi ve genişliği tanımlar:</span><span class="sxs-lookup"><span data-stu-id="0c611-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="0c611-116">Ardından, ihtiyacımız `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="0c611-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="0c611-117">Sağlama sonra bir `ID` ve normal `runat="server"`, `TargetControlID` özniteliği örneğimizde paneli animasyon için denetime ayarlanmalıdır:</span><span class="sxs-lookup"><span data-stu-id="0c611-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="0c611-118">Tüm animasyon bildirimli olarak, ne yazık ki şu anda tam olarak Visual Studio'nun IntelliSense tarafından desteklenen bir XML sözdizimi kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="0c611-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="0c611-119">Kök düğüm `<Animations>;` zaman animation(s) yer take(s) belirleyen çeşitli olaylarını bu düğümde izin verilir:</span><span class="sxs-lookup"><span data-stu-id="0c611-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="0c611-120">`OnClick`(fare tıklatın)</span><span class="sxs-lookup"><span data-stu-id="0c611-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="0c611-121">`OnHoverOut`(fare denetim ayrıldığında)</span><span class="sxs-lookup"><span data-stu-id="0c611-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="0c611-122">`OnHoverOver`(fare üzerinde denetim geldiğinde durdurma `OnHoverOut` animasyon)</span><span class="sxs-lookup"><span data-stu-id="0c611-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="0c611-123">`OnLoad`(sayfanın ne zaman yüklendi)</span><span class="sxs-lookup"><span data-stu-id="0c611-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="0c611-124">`OnMouseOut`(fare denetim ayrıldığında)</span><span class="sxs-lookup"><span data-stu-id="0c611-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="0c611-125">`OnMouseOver`(fare üzerinde denetim geldiğinde değil durdurma `OnMouseOut` animasyon)</span><span class="sxs-lookup"><span data-stu-id="0c611-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="0c611-126">Framework animasyonları, her biri kendi XML öğesi tarafından temsil edilen bir dizi ile gelir.</span><span class="sxs-lookup"><span data-stu-id="0c611-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="0c611-127">Bir seçim şöyledir:</span><span class="sxs-lookup"><span data-stu-id="0c611-127">Here is a selection:</span></span>

- <span data-ttu-id="0c611-128">`<Color>`(bir renk değiştirme)</span><span class="sxs-lookup"><span data-stu-id="0c611-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="0c611-129">`<FadeIn>`(yavaş giriş)</span><span class="sxs-lookup"><span data-stu-id="0c611-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="0c611-130">`<FadeOut>`(yavaş çıkış)</span><span class="sxs-lookup"><span data-stu-id="0c611-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="0c611-131">`<Property>`(bir denetimin özelliğini değiştirme)</span><span class="sxs-lookup"><span data-stu-id="0c611-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="0c611-132">`<Pulse>`(pulsating)</span><span class="sxs-lookup"><span data-stu-id="0c611-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="0c611-133">`<Resize>`(boyutunu değiştirme)</span><span class="sxs-lookup"><span data-stu-id="0c611-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="0c611-134">`<Scale>`(orantılı olarak boyutunu değiştirme)</span><span class="sxs-lookup"><span data-stu-id="0c611-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="0c611-135">Bu örnekte, bölmenin Kıs. Animasyonun 1.5 saniye sürebilir (`Duration` özniteliği), 24 çerçeveler (animasyon adımları) saniye başına görüntüleme (`Fps` attributs).</span><span class="sxs-lookup"><span data-stu-id="0c611-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="0c611-136">İçin tam biçimlendirme işte `AnimationExtender` denetimi:</span><span class="sxs-lookup"><span data-stu-id="0c611-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="0c611-137">Bu komut dosyasını çalıştırdığınızda paneli görüntülenir ve bir ve bir yarım saniye cinsinden yavaşça.</span><span class="sxs-lookup"><span data-stu-id="0c611-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="0c611-138">[![Bölmenin Soluklaşan](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0c611-138">[![The panel is fading out](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="0c611-139">Bölmenin Soluklaşan ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-animation-to-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0c611-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="0c611-140">Sonraki</span><span class="sxs-lookup"><span data-stu-id="0c611-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
