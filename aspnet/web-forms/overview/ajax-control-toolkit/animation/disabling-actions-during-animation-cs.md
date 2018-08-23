---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: (C#) animasyon sırasında eylemleri devre dışı bırakma | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Ayrıca, eylem destekler...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 4315f06ea1599bacb93a23a3759610e19754cfba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752946"
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="a24a2-104">(C#) animasyon sırasında eylemleri devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="a24a2-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="a24a2-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a24a2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a24a2-106">[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a24a2-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="a24a2-107">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="a24a2-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a24a2-108">Fare tıklamaları gibi eylemleri de destekler.</span><span class="sxs-lookup"><span data-stu-id="a24a2-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="a24a2-109">Ancak bir fare tıklaması bir animasyonu başlattığında, fare tıklamasına animasyon sırasında devre dışı bırakmak için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="a24a2-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="a24a2-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="a24a2-110">Overview</span></span>

<span data-ttu-id="a24a2-111">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="a24a2-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a24a2-112">Fare tıklamaları gibi eylemleri de destekler.</span><span class="sxs-lookup"><span data-stu-id="a24a2-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="a24a2-113">Ancak bir fare tıklaması bir animasyonu başlattığında, fare tıklamasına animasyon sırasında devre dışı bırakmak için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="a24a2-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="a24a2-114">Adımlar</span><span class="sxs-lookup"><span data-stu-id="a24a2-114">Steps</span></span>

<span data-ttu-id="a24a2-115">İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:</span><span class="sxs-lookup"><span data-stu-id="a24a2-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="a24a2-116">Böyle bir HTML düğmesi animasyonun uygulanır:</span><span class="sxs-lookup"><span data-stu-id="a24a2-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="a24a2-117">Bir geri gönderme oluşturmak için düğmeye istemiyorsanız bu yana bir HTML denetimini yerine Web denetimi kullanıldığını unutmayın; yalnızca bizim için istemci tarafı animasyonu başlatmak.</span><span class="sxs-lookup"><span data-stu-id="a24a2-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="a24a2-118">Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="a24a2-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="a24a2-119">İçinde `<Animations>` düğümünün `<OnClick>` fare tıklatın işlemek için doğru öğedir.</span><span class="sxs-lookup"><span data-stu-id="a24a2-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="a24a2-120">Ancak, animasyon sırasında de düğmeye tıkladı.</span><span class="sxs-lookup"><span data-stu-id="a24a2-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="a24a2-121">`<EnableAction>` Öğesi dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="a24a2-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="a24a2-122">Ayar `Enabled="false"` düğmeye animasyon bir parçası olarak devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="a24a2-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="a24a2-123">(Düğme ve gerçek animasyonlarını devre dışı bırakma), birkaç bireysel animasyon kullandığımızdan `<Parallel>` öğesi birlikte tek bir tek animasyonları yapıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a24a2-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="a24a2-124">İçin tam biçimlendirmesi şöyledir `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="a24a2-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="a24a2-125">Ayrıca düğmeyi sonra aşağıdaki XML öğesi listesinin sonunda kullanarak animasyon, yeniden etkinleştirmek mümkün olacaktır:</span><span class="sxs-lookup"><span data-stu-id="a24a2-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="a24a2-126">Ancak belirli bir senaryoda bu gereksiz düğmesidir yavaşça ve animasyon sonunda görünür değil.</span><span class="sxs-lookup"><span data-stu-id="a24a2-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="a24a2-127">[![Animasyon tamamlanmaz düğmesi devre dışıdır](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a24a2-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="a24a2-128">Animasyon tamamlanmaz düğmesi devre dışıdır ([tam boyutlu görüntüyü görmek için tıklatın](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a24a2-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a24a2-129">[Önceki](animating-in-response-to-user-interaction-cs.md)
> [İleri](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a24a2-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
