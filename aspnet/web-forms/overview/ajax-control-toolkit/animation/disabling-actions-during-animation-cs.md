---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Animasyon sırasında (C#) eylemleri devre dışı bırakma | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Ayrıca, eylem destekler...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 7862c5026a48fbee6eb48beb411e5e1d60c8b406
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="83821-104">Animasyon sırasında (C#) eylemleri devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="83821-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="83821-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="83821-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="83821-106">[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="83821-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="83821-107">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="83821-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="83821-108">Fare tıklamaları gibi eylemleri de destekler.</span><span class="sxs-lookup"><span data-stu-id="83821-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="83821-109">Ancak fare animasyonu başlattığında, fare tıklamaları animasyon sırasında devre dışı bırakmak için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="83821-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="83821-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="83821-110">Overview</span></span>

<span data-ttu-id="83821-111">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="83821-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="83821-112">Fare tıklamaları gibi eylemleri de destekler.</span><span class="sxs-lookup"><span data-stu-id="83821-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="83821-113">Ancak fare animasyonu başlattığında, fare tıklamaları animasyon sırasında devre dışı bırakmak için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="83821-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="83821-114">Adımlar</span><span class="sxs-lookup"><span data-stu-id="83821-114">Steps</span></span>

<span data-ttu-id="83821-115">İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:</span><span class="sxs-lookup"><span data-stu-id="83821-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="83821-116">Animasyonun bir HTML düğmesi bu gibi uygulanır:</span><span class="sxs-lookup"><span data-stu-id="83821-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="83821-117">Bir HTML denetimini geri gönderimin oluşturmak için düğmesini istemiyorsanız bu yana bir Web denetimi yerine kullanıldığını unutmayın; yalnızca istemci tarafında animasyonun bize başlatın.</span><span class="sxs-lookup"><span data-stu-id="83821-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="83821-118">Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="83821-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="83821-119">İçinde `<Animations>` düğümü `<OnClick>` fare tıklatma işlemeye sağ öğedir.</span><span class="sxs-lookup"><span data-stu-id="83821-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="83821-120">Ancak, animasyon sırasında da düğmesine tıklanana.</span><span class="sxs-lookup"><span data-stu-id="83821-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="83821-121">`<EnableAction>` Öğesi dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="83821-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="83821-122">Ayarı `Enabled="false"` animasyonun bir parçası olarak düğmesini devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="83821-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="83821-123">(Düğme ve gerçek animasyonlarını devre dışı bırakma), birkaç ayrı animasyon kullanıyoruz beri `<Parallel>` öğesi, tek bir animasyon birleştirerek bir Birleştirici için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="83821-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="83821-124">İçin tam biçimlendirme işte `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="83821-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="83821-125">Ayrıca düğmesine listesinin sonuna aşağıdaki XML öğesi kullanarak animasyon sonra yeniden etkinleştirmek mümkün olacaktır:</span><span class="sxs-lookup"><span data-stu-id="83821-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="83821-126">Ancak verilen senaryoda bu düğmesi gereksiz olacaktır yavaşça ve animasyon sonunda görünür değil.</span><span class="sxs-lookup"><span data-stu-id="83821-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="83821-127">[![Animasyonun tamamlanır tamamlanmaz düğmesi devre dışı bırakılır](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="83821-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="83821-128">Animasyonun tamamlanır tamamlanmaz düğmesi devre dışı ([tam boyutlu görüntüyü görüntülemek için tıklatın](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="83821-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="83821-129">[Önceki](animating-in-response-to-user-interaction-cs.md)
> [sonraki](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="83821-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
