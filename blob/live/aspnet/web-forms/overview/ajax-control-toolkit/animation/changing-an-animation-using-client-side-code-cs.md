---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: "İstemci tarafı kod (C#) kullanarak bir animasyon değiştirme | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyonun da yapabilirsiniz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 6dcaeac073f54b0804fe3acf7ec22491b1cbbba5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="ac6dc-104">İstemci tarafı kod (C#) kullanarak bir animasyon değiştirme</span><span class="sxs-lookup"><span data-stu-id="ac6dc-104">Changing an Animation Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="ac6dc-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ac6dc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ac6dc-106">[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ac6dc-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="ac6dc-107">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="ac6dc-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ac6dc-108">Animasyonun, özel istemci tarafı JavaScript kodu kullanarak da değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ac6dc-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="ac6dc-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="ac6dc-109">Overview</span></span>

<span data-ttu-id="ac6dc-110">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="ac6dc-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ac6dc-111">Animasyonun, özel istemci tarafı JavaScript kodu kullanarak da değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ac6dc-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="ac6dc-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="ac6dc-112">Steps</span></span>

<span data-ttu-id="ac6dc-113">İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:</span><span class="sxs-lookup"><span data-stu-id="ac6dc-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="ac6dc-114">Animasyonun bir panel şöyle metin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="ac6dc-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="ac6dc-115">İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="ac6dc-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="ac6dc-116">Gerçek animasyon bir HTML düğmesi tarafından başlatılır:</span><span class="sxs-lookup"><span data-stu-id="ac6dc-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="ac6dc-117">Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="ac6dc-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="ac6dc-118">Unutmayın hiçbir `<Animations>` düğümde `AnimationExtender` denetim.</span><span class="sxs-lookup"><span data-stu-id="ac6dc-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="ac6dc-119">Özel JavaScript kodu denetimiyle kullanılacak animasyonları sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ac6dc-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="ac6dc-120">Sunucunun API ile `AnimationExtender`, animasyonun genişletici henüz atamak için kolay bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="ac6dc-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="ac6dc-121">Genişletici okuyup animasyonları yazmak için çeşitli yöntemler ancak kullanıma kayıtlı olan çeşitli olaylar (`OnClick`, `OnLoad`, vb.).</span><span class="sxs-lookup"><span data-stu-id="ac6dc-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="ac6dc-122">Bazı örnekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="ac6dc-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="ac6dc-123">Dönüş değerini biçimi `get_*()` işlevleri ve ilgili bağımsız değişken biçimi `set_*()` işlevleri XML Biçimlendirme ne olacağını, bir nesne temsili sağlayan bir JSON dizesi değil.</span><span class="sxs-lookup"><span data-stu-id="ac6dc-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="ac6dc-124">Şu anda bir nesneyi içeri aktarmanız yolu yoktur, ancak bir nesne belirli bir animasyon okumak mümkündür (`get_OnXXXBehavior()` yöntemleri).</span><span class="sxs-lookup"><span data-stu-id="ac6dc-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="ac6dc-125">Bir JSON dizesinde İşte (sınırlandırma tırnak işaretleri olmadan ve düzgün şekilde biçimlendirilmiş) düğmesini tarafından tetiklenen bir animasyon temsil eden, ancak bunu yeniden boyutlandırma ve aynı anda yavaş çıkış paneli animasyon:</span><span class="sxs-lookup"><span data-stu-id="ac6dc-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="ac6dc-126">Şu JavaScript kodunu için bu JSON descripting atar `OnClick` geçerli genişletici animasyon ve bunu çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="ac6dc-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


<span data-ttu-id="ac6dc-127">[![Fare olmadan (ve çok az biçimlendirme) animasyon hemen çalışır](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ac6dc-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="ac6dc-128">Animasyonun bir tıklatma olmadan (ve çok az biçimlendirme) hemen çalıştırır ([tam boyutlu görüntüyü görüntülemek için tıklatın](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ac6dc-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ac6dc-129">[Önceki](executing-animations-using-client-side-code-cs.md)
[sonraki](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="ac6dc-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
