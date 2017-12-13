---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: "Animasyonun bir koşul (C#) bağlı olarak | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Bir animasyon olup..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: 13366a86be01f41e27db1869b93192520190387a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="animation-depending-on-a-condition-c"></a><span data-ttu-id="f17fd-104">Animasyon bağlı olarak bir koşul (C#)</span><span class="sxs-lookup"><span data-stu-id="f17fd-104">Animation Depending On a Condition (C#)</span></span>
====================
<span data-ttu-id="f17fd-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f17fd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f17fd-106">[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f17fd-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span></span>

> <span data-ttu-id="f17fd-107">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="f17fd-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f17fd-108">Bir animasyon veya çalıştırılan ayrıca bazı JavaScript kodu biçiminde bir koşulunu bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="f17fd-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="f17fd-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="f17fd-109">Overview</span></span>

<span data-ttu-id="f17fd-110">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="f17fd-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f17fd-111">Bir animasyon veya çalıştırılan ayrıca bazı JavaScript kodu biçiminde bir koşulunu bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="f17fd-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="f17fd-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="f17fd-112">Steps</span></span>

<span data-ttu-id="f17fd-113">İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:</span><span class="sxs-lookup"><span data-stu-id="f17fd-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

<span data-ttu-id="f17fd-114">Animasyonun bir panel şöyle metin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="f17fd-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

<span data-ttu-id="f17fd-115">İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="f17fd-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

<span data-ttu-id="f17fd-116">Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu`runat="server":`</span><span class="sxs-lookup"><span data-stu-id="f17fd-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

<span data-ttu-id="f17fd-117">İçinde `<Animations>` düğümü, kullanım `<OnLoad>` sayfa tam yüklenmeden silindikten sonra animasyonları çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="f17fd-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="f17fd-118">Normal bir animasyon birini yerine `<Condition>` öğesi oyuna gelir.</span><span class="sxs-lookup"><span data-stu-id="f17fd-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="f17fd-119">Değeri olarak sağlanan JavaScript kodu `ConditionScript` özniteliği çalışma zamanında gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f17fd-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="f17fd-120">Doğru olarak değerlendirir, animasyon, aksi takdirde yürütülemiyor.</span><span class="sxs-lookup"><span data-stu-id="f17fd-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="f17fd-121">Aşağıdaki biçimlendirmede bunların rastgele bağlı örneklerinin % 50 yürütülen her birini iki animasyon sağlar.</span><span class="sxs-lookup"><span data-stu-id="f17fd-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="f17fd-122">Yalnızca olabilir beri içinde bir animasyon `<OnLoad>`, iki `<Condition>` animasyonları birlikte kullanarak birleştirilir `<Sequence>` öğe:</span><span class="sxs-lookup"><span data-stu-id="f17fd-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

<span data-ttu-id="f17fd-123">Unutmayın küçüktür işareti (`<`) içinde `ConditionScript` özniteliği Atlanan () olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f17fd-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="f17fd-124">Ne zaman bu komut, ya da hiçbir animasyon çalıştığında, çalıştırmak veya iki birini desteklemez veya her ikisini birden yapın.</span><span class="sxs-lookup"><span data-stu-id="f17fd-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="f17fd-125">[![İkinci animasyon çalıştığında, ilk kaydetmedi şekilde paneli boyutlandırmadan, yavaş çıkış](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f17fd-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span></span>

<span data-ttu-id="f17fd-126">İkinci animasyon çalıştığında, ilk kaydetmedi şekilde paneli boyutlandırmadan, yavaş çıkış ([tam boyutlu görüntüyü görüntülemek için tıklatın](animation-depending-on-a-condition-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f17fd-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f17fd-127">[Önceki](executing-several-animations-after-each-other-cs.md)
[sonraki](picking-one-animation-out-of-a-list-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f17fd-127">[Previous](executing-several-animations-after-each-other-cs.md)
[Next](picking-one-animation-out-of-a-list-cs.md)</span></span>
