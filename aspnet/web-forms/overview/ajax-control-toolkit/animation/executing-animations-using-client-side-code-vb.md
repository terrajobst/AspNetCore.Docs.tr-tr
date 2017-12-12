---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: "İstemci tarafı kodlar (VB) kullanarak animasyonları yürütme | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyon yürütme..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c97ce87addd566ed1ba63ccdb81206c6449f49a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="c8490-104">İstemci tarafı kodlar (VB) kullanarak animasyonları yürütme</span><span class="sxs-lookup"><span data-stu-id="c8490-104">Executing Animations Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="c8490-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c8490-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c8490-106">[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c8490-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="c8490-107">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="c8490-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c8490-108">Animasyon yürütme, özel istemci tarafı JavaScript kodu kullanarak da tetiklenebilir.</span><span class="sxs-lookup"><span data-stu-id="c8490-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="c8490-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="c8490-109">Overview</span></span>

<span data-ttu-id="c8490-110">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="c8490-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c8490-111">Animasyon yürütme, özel istemci tarafı JavaScript kodu kullanarak da tetiklenebilir.</span><span class="sxs-lookup"><span data-stu-id="c8490-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="c8490-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="c8490-112">Steps</span></span>

<span data-ttu-id="c8490-113">İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:</span><span class="sxs-lookup"><span data-stu-id="c8490-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="c8490-114">Animasyonun bir panel şöyle metin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="c8490-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="c8490-115">İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="c8490-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="c8490-116">Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="c8490-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="c8490-117">İçinde `<Animations>` düğümü, kullanım `<OnClick>` animasyonları kullanıcı bir kez çalıştırmayı panelde tıklar.</span><span class="sxs-lookup"><span data-stu-id="c8490-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="c8490-118">Parallelly yürütülecek iki animasyon ekleyin:</span><span class="sxs-lookup"><span data-stu-id="c8490-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="c8490-119">Tanıtım amacıyla, bu animasyon (ve Denetim Araç Seti kullanılarak oluşturulan animasyon) sayfayı çalıştıran sonra JavaScript kodu kullanarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="c8490-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="c8490-120">Erişim için öncelikle ihtiyacımız `AnimationExtender` denetim.</span><span class="sxs-lookup"><span data-stu-id="c8490-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="c8490-121">ASP.NET AJAX kitaplığı sağlayan `$find()` işlevi bu görev için:</span><span class="sxs-lookup"><span data-stu-id="c8490-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="c8490-122">`AnimationExtender` Denetim XML biçimlendirmede kullanılan olay işleyicileri aynı adlarla yöntemleri de dahil olmak üzere zengin bir API sunar: `OnClick()`, `OnLoad()`ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="c8490-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="c8490-123">Örneği için bir çağrı `OnClick()` yöntemini yürütür içinde animasyon `<OnClick>` öğesinin `AnimationExtender` denetimi:</span><span class="sxs-lookup"><span data-stu-id="c8490-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="c8490-124">İşte panelindeki tıklatın sayfa tam yüklenmeden silindikten sonra öykünen tam istemci tarafı JavaScript kodu Not `pageLoad()` işlev adı, ASP.NET AJAX tarafından bir kez sayfası olarak adlandırılır kullanılır ve tüm kitaplıklar olmuştur JavaScript dahil yüklendi.</span><span class="sxs-lookup"><span data-stu-id="c8490-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


<span data-ttu-id="c8490-125">[![Animasyonun bir fare tıklatma hemen çalışır](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c8490-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="c8490-126">Animasyonun bir tıklatma olmadan hemen çalıştırır ([tam boyutlu görüntüyü görüntülemek için tıklatın](executing-animations-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c8490-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c8490-127">[Önceki](modifying-animations-from-the-server-side-vb.md)
[sonraki](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c8490-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
