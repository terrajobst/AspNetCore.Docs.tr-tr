---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Bir animasyon listesi (VB) dışında çekme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Framework de izin ver...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: f2bd1b3cc72595da7e8901786ea8415d7c1c524a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872071"
---
<a name="picking-one-animation-out-of-a-list-vb"></a><span data-ttu-id="6631a-104">Bir animasyon listesi (VB) dışında çekme</span><span class="sxs-lookup"><span data-stu-id="6631a-104">Picking One Animation Out Of a List (VB)</span></span>
====================
<span data-ttu-id="6631a-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6631a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6631a-106">[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6631a-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span></span>

> <span data-ttu-id="6631a-107">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="6631a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6631a-108">Çerçeve ayrıca bazı JavaScript kodları değerlendirmesine bağlı olarak bir animasyon listesi dışında bir animasyon seçmek Programcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="6631a-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="6631a-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="6631a-109">Overview</span></span>

<span data-ttu-id="6631a-110">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="6631a-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6631a-111">Çerçeve ayrıca bazı JavaScript kodları değerlendirmesine bağlı olarak bir animasyon listesi dışında bir animasyon seçmek Programcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="6631a-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="6631a-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="6631a-112">Steps</span></span>

<span data-ttu-id="6631a-113">İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:</span><span class="sxs-lookup"><span data-stu-id="6631a-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

<span data-ttu-id="6631a-114">Animasyonun bir panel şöyle metin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="6631a-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

<span data-ttu-id="6631a-115">İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="6631a-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

<span data-ttu-id="6631a-116">Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="6631a-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

<span data-ttu-id="6631a-117">İçinde `<Animations>` düğümü, kullanım `<OnLoad>` sayfa tam yüklenmeden silindikten sonra animasyonları çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="6631a-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="6631a-118">Normal bir animasyon birini yerine `<Case>` öğesi oyuna gelir.</span><span class="sxs-lookup"><span data-stu-id="6631a-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="6631a-119">Kendi SelectScript özniteliğinin değeri değerlendirilir; dönüş değeri sayısal olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6631a-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="6631a-120">Bu sayı, içinde subanimations birini bağlı olarak &lt;durumda&gt; yürütülür.</span><span class="sxs-lookup"><span data-stu-id="6631a-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="6631a-121">SelectScript 2 olarak değerlendirilirse, Denetim Araç Seti içinde üçüncü animasyon örneği için çalışan &lt;durumda&gt; (sayım 0 başlar).</span><span class="sxs-lookup"><span data-stu-id="6631a-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="6631a-122">Aşağıdaki biçimlendirmede üç subanimations tanımlar: genişliği yeniden boyutlandırma, yüksekliği yeniden boyutlandırma ve yavaş çıkış. JavaScript kodu (`Math.floor(3 * Math.random())`) üç animasyon birini çalışabilmesi 0 ve 2 ' arasında bir sayı seçer:</span><span class="sxs-lookup"><span data-stu-id="6631a-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


<span data-ttu-id="6631a-123">[![Olası üç animasyon birini: paneli daha geniş alır](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6631a-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span></span>

<span data-ttu-id="6631a-124">Olası üç animasyon birini: paneli daha geniş alır ([tam boyutlu görüntüyü görüntülemek için tıklatın](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6631a-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6631a-125">[Önceki](animation-depending-on-a-condition-vb.md)
> [sonraki](animating-in-response-to-user-interaction-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6631a-125">[Previous](animation-depending-on-a-condition-vb.md)
[Next](animating-in-response-to-user-interaction-vb.md)</span></span>
