---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Bir UpdatePanel denetimi (C#) animasyon | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. İçeriği için bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 5d8d5b9c3f15b39045b5e01b455bdddfc9443a24
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878548"
---
<a name="animating-an-updatepanel-control-c"></a><span data-ttu-id="7d211-104">Animasyon bir UpdatePanel denetimi (C#)</span><span class="sxs-lookup"><span data-stu-id="7d211-104">Animating an UpdatePanel Control (C#)</span></span>
====================
<span data-ttu-id="7d211-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7d211-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7d211-106">[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7d211-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)</span></span>

> <span data-ttu-id="7d211-107">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="7d211-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7d211-108">Bir UpdatePanel içeriği için özel bir genişletici yoğun animasyon Framework'te güvenen var: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="7d211-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="7d211-109">Bu öğretici, bu tür bir animasyon için bir UpdatePanel ayarlamak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7d211-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="7d211-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="7d211-110">Overview</span></span>

<span data-ttu-id="7d211-111">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="7d211-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7d211-112">İçeriği için bir `UpdatePanel`, özel bir genişletici yoğun animasyon Framework'te güvenen var: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="7d211-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="7d211-113">Bu öğretici, bu tür bir animasyon için ayarlamak üzere gösterilmiştir bir `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="7d211-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="7d211-114">Adımlar</span><span class="sxs-lookup"><span data-stu-id="7d211-114">Steps</span></span>

<span data-ttu-id="7d211-115">İlk adım her zamanki gibi eklemektir `ScriptManager` sayfasındaki ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="7d211-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

<span data-ttu-id="7d211-116">Bu senaryoda animasyon bir ASP.NET uygulanacak `Wizard` web denetimi bulunan bir `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="7d211-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="7d211-117">Üç (isteğe bağlı) adımı Geri göndermeler tetiklemek için yeterli seçenekleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="7d211-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

<span data-ttu-id="7d211-118">İşaretleme için gerekli `UpdatePanelAnimationExtender` denetimi için kullanılan biçimlendirme oldukça benzer `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="7d211-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="7d211-119">İçinde `TargetControlID` sağladığımız özniteliği `ID` , `UpdatePanel` animasyon için; içinde `UpdatePanelAnimationExtender` denetimi `<Animations>` öğesi animation(s) için XML biçimlendirmesini tutar.</span><span class="sxs-lookup"><span data-stu-id="7d211-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="7d211-120">Ancak bir fark yoktur: miktarını olayların ve olay işleyicileri comparison için sınırlı `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="7d211-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="7d211-121">İçin `UpdatePanels`, yalnızca iki bunları var:</span><span class="sxs-lookup"><span data-stu-id="7d211-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="7d211-122">`<OnUpdated>` UpdatePanel ne zaman güncelleştirildiğini</span><span class="sxs-lookup"><span data-stu-id="7d211-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="7d211-123">`<OnUpdating>` Güncelleştirme UpdatePanel başladığında</span><span class="sxs-lookup"><span data-stu-id="7d211-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="7d211-124">Bu senaryoda, yeni içeriği `UpdatePanel` (sonra geri gönderme) belirerek.</span><span class="sxs-lookup"><span data-stu-id="7d211-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="7d211-125">Bu, söz konusu gerekli biçimlendirme oluşur:</span><span class="sxs-lookup"><span data-stu-id="7d211-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

<span data-ttu-id="7d211-126">Geri gönderimin içinde UpdatePanel oluştuğunda artık yeni içeriği bölmenin yavaşça.</span><span class="sxs-lookup"><span data-stu-id="7d211-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


<span data-ttu-id="7d211-127">[![Sonraki sihirbaz adımı Soluklaşan](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7d211-127">[![The next wizard step is fading in](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="7d211-128">Sonraki sihirbaz adımı Soluklaşan ([tam boyutlu görüntüyü görüntülemek için tıklatın](animating-an-updatepanel-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7d211-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7d211-129">[Önceki](changing-an-animation-using-client-side-code-cs.md)
> [sonraki](dynamically-controlling-updatepanel-animations-cs.md)</span><span class="sxs-lookup"><span data-stu-id="7d211-129">[Previous](changing-an-animation-using-client-side-code-cs.md)
[Next](dynamically-controlling-updatepanel-animations-cs.md)</span></span>
