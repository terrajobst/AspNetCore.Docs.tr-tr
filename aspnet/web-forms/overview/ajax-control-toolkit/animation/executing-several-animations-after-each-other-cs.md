---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Arkaya (C#) birkaç animasyon yürütme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Severa çalıştırılacak sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 49d33ee28984981c7757f14fe7c16fb2dc8f744e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362157"
---
<a name="executing-several-animations-after-each-other-c"></a><span data-ttu-id="d6f79-104">Arkaya (C#) birkaç animasyon yürütme</span><span class="sxs-lookup"><span data-stu-id="d6f79-104">Executing Several Animations after Each Other (C#)</span></span>
====================
<span data-ttu-id="d6f79-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d6f79-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d6f79-106">[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d6f79-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span></span>

> <span data-ttu-id="d6f79-107">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="d6f79-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d6f79-108">Art arda birkaç animasyon bir çalıştırmayı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d6f79-108">It allows to run several animations one after the other.</span></span>


<span data-ttu-id="d6f79-109">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="d6f79-109">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d6f79-110">Art arda birkaç animasyon bir çalıştırmayı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d6f79-110">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="d6f79-111">Adımlar</span><span class="sxs-lookup"><span data-stu-id="d6f79-111">Steps</span></span>

<span data-ttu-id="d6f79-112">İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:</span><span class="sxs-lookup"><span data-stu-id="d6f79-112">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

<span data-ttu-id="d6f79-113">Animasyonun bir panel şuna benzer metin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="d6f79-113">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

<span data-ttu-id="d6f79-114">İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="d6f79-114">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

<span data-ttu-id="d6f79-115">Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="d6f79-115">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

<span data-ttu-id="d6f79-116">İçinde `<Animations>` düğümü, kullanım `<OnLoad>` animasyonları sayfanın tam yüklü silindikten sonra çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="d6f79-116">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="d6f79-117">Genellikle, `<OnLoad>` yalnızca bir animasyon kabul eder.</span><span class="sxs-lookup"><span data-stu-id="d6f79-117">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="d6f79-118">Animasyon çerçevesi bir kullanarak birkaç animasyon katılmaya sağlar `<Sequence>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="d6f79-118">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="d6f79-119">İçindeki tüm animasyonları `<Sequence>` yürütülen art arda olan.</span><span class="sxs-lookup"><span data-stu-id="d6f79-119">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="d6f79-120">İşte için olası bir biçimlendirme `AnimationExtender` ilk geniş paneli yapma ve ardından yükseklik azalan denetimi:</span><span class="sxs-lookup"><span data-stu-id="d6f79-120">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

<span data-ttu-id="d6f79-121">Bu komut, panel ilk alır ve ardından daha küçük geniş çalıştırıldığında.</span><span class="sxs-lookup"><span data-stu-id="d6f79-121">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="d6f79-122">[![İlk genişliğini artırılır](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d6f79-122">[![First the width is increased](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span></span>

<span data-ttu-id="d6f79-123">İlk genişliğini artırılır ([tam boyutlu görüntüyü görmek için tıklatın](executing-several-animations-after-each-other-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d6f79-123">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image3.png))</span></span>


<span data-ttu-id="d6f79-124">[![Ardından yüksekliği azalır](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="d6f79-124">[![Then the height is decreased](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span></span>

<span data-ttu-id="d6f79-125">Yüksekliği azalır sonra ([tam boyutlu görüntüyü görmek için tıklatın](executing-several-animations-after-each-other-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d6f79-125">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d6f79-126">[Önceki](executing-several-animations-at-the-same-time-cs.md)
> [İleri](animation-depending-on-a-condition-cs.md)</span><span class="sxs-lookup"><span data-stu-id="d6f79-126">[Previous](executing-several-animations-at-the-same-time-cs.md)
[Next](animation-depending-on-a-condition-cs.md)</span></span>
