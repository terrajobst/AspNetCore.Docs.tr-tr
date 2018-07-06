---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: İstemci tarafı kod (C#) kullanarak bir animasyonu değiştirme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Ayrıca animasyon...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 9711e93ee6f119ec1825e32bdad64535435970c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808962"
---
<a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="37dfb-104">İstemci tarafı kod (C#) kullanarak bir animasyonu değiştirme</span><span class="sxs-lookup"><span data-stu-id="37dfb-104">Changing an Animation Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="37dfb-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="37dfb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="37dfb-106">[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="37dfb-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="37dfb-107">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="37dfb-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="37dfb-108">Animasyon, özel istemci tarafı JavaScript kodu kullanarak da değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="37dfb-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="37dfb-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="37dfb-109">Overview</span></span>

<span data-ttu-id="37dfb-110">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="37dfb-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="37dfb-111">Animasyon, özel istemci tarafı JavaScript kodu kullanarak da değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="37dfb-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="37dfb-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="37dfb-112">Steps</span></span>

<span data-ttu-id="37dfb-113">İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:</span><span class="sxs-lookup"><span data-stu-id="37dfb-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="37dfb-114">Animasyonun bir panel şuna benzer metin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="37dfb-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="37dfb-115">İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="37dfb-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="37dfb-116">Gerçek animasyon tarafından bir HTML düğmesi başlatılır:</span><span class="sxs-lookup"><span data-stu-id="37dfb-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="37dfb-117">Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="37dfb-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="37dfb-118">Unutmayın hiçbir `<Animations>` düğümünde `AnimationExtender` denetimi.</span><span class="sxs-lookup"><span data-stu-id="37dfb-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="37dfb-119">Özel bir JavaScript kodu denetimiyle kullanılacak animasyonları sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="37dfb-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="37dfb-120">Sunucunun API ile `AnimationExtender`, animasyon için genişletici henüz atamak için kolay bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="37dfb-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="37dfb-121">Ancak genişletici okuma ve yazma animasyonları çeşitli yöntemleri açığa kaydedilen çeşitli olayları (`OnClick`, `OnLoad`, vb.).</span><span class="sxs-lookup"><span data-stu-id="37dfb-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="37dfb-122">Bazı örnekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="37dfb-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="37dfb-123">Dönüş değeri biçimi `get_*()` işlevler için bağımsız değişken biçimlerinin ve kimliklerinin `set_*()` işlevleri, bir nesne temsili, XML işaretlemesini ne olacağını sağlayan bir JSON dizesi.</span><span class="sxs-lookup"><span data-stu-id="37dfb-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="37dfb-124">Şu anda, nesneyi geçirmek için bir yolu yoktur, ancak belirli bir animasyon bir nesne okumak mümkündür (`get_OnXXXBehavior()` yöntemleri).</span><span class="sxs-lookup"><span data-stu-id="37dfb-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="37dfb-125">Bir JSON dizesi İşte (sınırlandırma tırnak işaretleri olmadan düzgün şekilde biçimlendirilmiş) düğmesi tarafından tetiklenen animasyon temsil eden, ancak yeniden boyutlandırdıktan ve aynı anda yavaş çıkış panelinde animasyon ekleme:</span><span class="sxs-lookup"><span data-stu-id="37dfb-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="37dfb-126">İçin bu JSON descripting aşağıdaki JavaScript kodunu atar `OnClick` geçerli genişletici animasyon ve çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="37dfb-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


<span data-ttu-id="37dfb-127">[![Fare tıklatın olmadan (ve çok az biçimlendirme) animasyon hemen çalışır](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="37dfb-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="37dfb-128">Fare tıklatın olmadan (ve çok az biçimlendirme ile) animasyon hemen çalışır ([tam boyutlu görüntüyü görmek için tıklatın](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="37dfb-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="37dfb-129">[Önceki](executing-animations-using-client-side-code-cs.md)
> [İleri](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="37dfb-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
