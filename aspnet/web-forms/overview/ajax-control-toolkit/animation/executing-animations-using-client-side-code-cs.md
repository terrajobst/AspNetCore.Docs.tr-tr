---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Animasyonları istemci tarafı kod (C#) kullanarak yürütme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyon yürütme...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: ae8c4df3c5852e9ee95f6184a86059859c56b128
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824564"
---
<a name="executing-animations-using-client-side-code-c"></a><span data-ttu-id="4cf78-104">Animasyonları istemci tarafı kod (C#) kullanarak yürütme</span><span class="sxs-lookup"><span data-stu-id="4cf78-104">Executing Animations Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="4cf78-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4cf78-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4cf78-106">[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4cf78-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span></span>

> <span data-ttu-id="4cf78-107">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="4cf78-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4cf78-108">Animasyon yürütme, özel istemci tarafı JavaScript kodu kullanarak da tetiklenebilir.</span><span class="sxs-lookup"><span data-stu-id="4cf78-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="4cf78-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="4cf78-109">Overview</span></span>

<span data-ttu-id="4cf78-110">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="4cf78-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4cf78-111">Animasyon yürütme, özel istemci tarafı JavaScript kodu kullanarak da tetiklenebilir.</span><span class="sxs-lookup"><span data-stu-id="4cf78-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="4cf78-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="4cf78-112">Steps</span></span>

<span data-ttu-id="4cf78-113">İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:</span><span class="sxs-lookup"><span data-stu-id="4cf78-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="4cf78-114">Animasyonun bir panel şuna benzer metin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="4cf78-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="4cf78-115">İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="4cf78-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="4cf78-116">Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="4cf78-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="4cf78-117">İçinde `<Animations>` düğümü, kullanım `<OnClick>` animasyonlar kullanıcı bir kez çalıştırılacak panelinde tıklar.</span><span class="sxs-lookup"><span data-stu-id="4cf78-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="4cf78-118">Parallelly yürütülecek iki animasyon ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4cf78-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

<span data-ttu-id="4cf78-119">Gösterim amacıyla, bu animasyonu (ve Denetim Araç Seti kullanılarak oluşturulan animasyon) sayfanın çalıştırıldıktan sonra JavaScript kodu kullanarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4cf78-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="4cf78-120">İlk olarak erişim ihtiyacımız `AnimationExtender` denetimi.</span><span class="sxs-lookup"><span data-stu-id="4cf78-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="4cf78-121">ASP.NET AJAX kitaplığı sağlar `$find()` işlevi bu görev için:</span><span class="sxs-lookup"><span data-stu-id="4cf78-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

<span data-ttu-id="4cf78-122">`AnimationExtender` Denetim XML biçimlendirmede kullanılan olay işleyicilerine benzer adlarla yöntemleri dahil olmak üzere zengin bir API sunar: `OnClick()`, `OnLoad()`ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="4cf78-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="4cf78-123">Örneği için bir çağrı `OnClick()` yöntemini yürütür içinde animasyon `<OnClick>` öğesinin `AnimationExtender` denetimi:</span><span class="sxs-lookup"><span data-stu-id="4cf78-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

<span data-ttu-id="4cf78-124">İşte sayfanın tam olarak yüklendikten sonra tıklayarak Panodaki öykünen tüm istemci tarafı JavaScript kod Not `pageLoad()` işlev adı ASP.NET AJAX ile bir kez sayfa çağrılan kullanılır ve dahil olan tüm JavaScript kitaplıkları yapıldı yüklendi.</span><span class="sxs-lookup"><span data-stu-id="4cf78-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


<span data-ttu-id="4cf78-125">[![Animasyonun bir fare tıklaması hemen çalışır](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4cf78-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="4cf78-126">Fare tıklatın olmadan animasyon hemen çalışır ([tam boyutlu görüntüyü görmek için tıklatın](executing-animations-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4cf78-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4cf78-127">[Önceki](modifying-animations-from-the-server-side-cs.md)
> [İleri](changing-an-animation-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="4cf78-127">[Previous](modifying-animations-from-the-server-side-cs.md)
[Next](changing-an-animation-using-client-side-code-cs.md)</span></span>
