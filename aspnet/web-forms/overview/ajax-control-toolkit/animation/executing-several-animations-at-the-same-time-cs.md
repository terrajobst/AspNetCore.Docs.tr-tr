---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: "Aynı anda (C#) birkaç animasyon yürütme | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Severa çalışmasına izin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: b7ee9c2da392bed512e259b44237e5ff997dca23
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="executing-several-animations-at-the-same-time-c"></a><span data-ttu-id="8b054-104">Aynı anda (C#) birkaç animasyon yürütme</span><span class="sxs-lookup"><span data-stu-id="8b054-104">Executing Several Animations at The Same Time (C#)</span></span>
====================
<span data-ttu-id="8b054-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8b054-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8b054-106">[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8b054-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span></span>

> <span data-ttu-id="8b054-107">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="8b054-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8b054-108">Bu, birkaç animasyon paralel bir şekilde çalışmasına olanak verir.</span><span class="sxs-lookup"><span data-stu-id="8b054-108">It allows to run several animations in a parallel fashion.</span></span>


## <a name="overview"></a><span data-ttu-id="8b054-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="8b054-109">Overview</span></span>

<span data-ttu-id="8b054-110">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="8b054-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8b054-111">Bu, birkaç animasyon paralel bir şekilde çalışmasına olanak verir.</span><span class="sxs-lookup"><span data-stu-id="8b054-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="8b054-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="8b054-112">Steps</span></span>

<span data-ttu-id="8b054-113">İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:</span><span class="sxs-lookup"><span data-stu-id="8b054-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

<span data-ttu-id="8b054-114">Animasyonun bir panel şöyle metin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="8b054-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

<span data-ttu-id="8b054-115">İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="8b054-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

<span data-ttu-id="8b054-116">Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="8b054-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

<span data-ttu-id="8b054-117">İçinde `<Animations>` düğümü, kullanım `<OnLoad>` sayfa tam yüklenmeden silindikten sonra animasyonları çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="8b054-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="8b054-118">Genellikle, `<OnLoad>` yalnızca bir animasyon kabul eder.</span><span class="sxs-lookup"><span data-stu-id="8b054-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="8b054-119">Animasyon çerçevesi bir kullanarak birkaç animasyon katılma sayesinde `<Parallel>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="8b054-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="8b054-120">İçindeki tüm animasyonları `<Parallel>` aynı anda çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="8b054-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="8b054-121">İşte için olası bir biçimlendirme `AnimationExtender` yavaş çıkış ve paneli aynı anda yeniden boyutlandırma denetimi:</span><span class="sxs-lookup"><span data-stu-id="8b054-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

<span data-ttu-id="8b054-122">Ve gerçekten: Bu komut dosyasını çalıştırdığınızda paneli görüntülenir, sonra yeniden boyutlandırır (threefolding'den fazla genişlik ve halfing yüksekliğini) ve aynı anda silinerek çıkar.</span><span class="sxs-lookup"><span data-stu-id="8b054-122">And indeed: when you run this script, the panel is displayed, then resizes (more than threefolding its width and halfing its height) and fades out at the same time.</span></span>


<span data-ttu-id="8b054-123">[![Bölmenin yavaş çıkış ve (tarayıcının işleme altyapısı sayesinde içeriğine dahil) yeniden boyutlandırma](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8b054-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span></span>

<span data-ttu-id="8b054-124">Bölmenin yavaş çıkış ve (tarayıcının işleme altyapısı sayesinde içeriğine dahil) yeniden boyutlandırma ([tam boyutlu görüntüyü görüntülemek için tıklatın](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8b054-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8b054-125">[Önceki](adding-animation-to-a-control-cs.md)
[sonraki](executing-several-animations-after-each-other-cs.md)</span><span class="sxs-lookup"><span data-stu-id="8b054-125">[Previous](adding-animation-to-a-control-cs.md)
[Next](executing-several-animations-after-each-other-cs.md)</span></span>
