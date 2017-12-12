---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: "Dinamik olarak UpdatePanel animasyonları (C#) denetleme | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. İçeriği için bir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 0408556e322a46211388fbd557d7a967044129ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-controlling-updatepanel-animations-c"></a><span data-ttu-id="170dd-104">Dinamik olarak kontrol eden UpdatePanel animasyonları (C#)</span><span class="sxs-lookup"><span data-stu-id="170dd-104">Dynamically Controlling UpdatePanel Animations (C#)</span></span>
====================
<span data-ttu-id="170dd-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="170dd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="170dd-106">[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="170dd-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span></span>

> <span data-ttu-id="170dd-107">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="170dd-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="170dd-108">Bir UpdatePanel içeriği için özel bir genişletici yoğun animasyon Framework'te güvenen var: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="170dd-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="170dd-109">Ayrıca, UpdatePanel Tetikleyicileri birlikte de çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="170dd-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="170dd-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="170dd-110">Overview</span></span>

<span data-ttu-id="170dd-111">ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="170dd-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="170dd-112">İçeriği için bir `UpdatePanel`, özel bir genişletici yoğun animasyon Framework'te güvenen var: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="170dd-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="170dd-113">Ayrıca ile birlikte çalışabilir `UpdatePanel` tetikler.</span><span class="sxs-lookup"><span data-stu-id="170dd-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="170dd-114">Adımlar</span><span class="sxs-lookup"><span data-stu-id="170dd-114">Steps</span></span>

<span data-ttu-id="170dd-115">İlk adım her zamanki gibi eklemektir `ScriptManager` sayfasındaki ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="170dd-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

<span data-ttu-id="170dd-116">Bu senaryoda animasyon geçerli saati görüntüye uygulanacak.</span><span class="sxs-lookup"><span data-stu-id="170dd-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="170dd-117">Bu bilgileri kullanarak bir etiket yazılabilir `Page_Load()` yöntemini veya (basitleştirmek amacıyla) aşağıdaki satır içi kod kullanılır:</span><span class="sxs-lookup"><span data-stu-id="170dd-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

<span data-ttu-id="170dd-118">Ayrıca, saati güncelleştiriliyor tetiklemek için bir düğmeye oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="170dd-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

<span data-ttu-id="170dd-119">Bu kod içine konur `<ContentTemplate>` bölümünü bir `UpdatePanel` öğesi.</span><span class="sxs-lookup"><span data-stu-id="170dd-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="170dd-120">Bölmenin `UpdateMode` özniteliği ayarlanmalıdır `"Conditional"`, yalnızca Tetikleyicileri bölmenin içeriği güncelleştirebilir beri.</span><span class="sxs-lookup"><span data-stu-id="170dd-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="170dd-121">İçinde `<Triggers>` bölümünü `UpdatePanel`, zaman uyumsuz geri gönderme tetikleyici oluşturulur ve bağlı `Click` düğmesinin olayı.</span><span class="sxs-lookup"><span data-stu-id="170dd-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="170dd-122">Bu nedenle, kullanıcı düğmesine tıklarsa `UpdatePanel` yenilenmez.</span><span class="sxs-lookup"><span data-stu-id="170dd-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="170dd-123">İşaretleme için işte `UpdatePanel` denetimi:</span><span class="sxs-lookup"><span data-stu-id="170dd-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

<span data-ttu-id="170dd-124">Son olarak, `UpdatePanelAnimationExtender` yapılandırılmalıdır: ayarlamak `TargetControlID` özniteliği paneli Kimliğine ve bir animasyon genişletici içinde tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="170dd-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="170dd-125">İçinde yapar Soluklaşan iyi bir görsel Vurgu güncelleştirilmiş zamanında oluşturur algılama.</span><span class="sxs-lookup"><span data-stu-id="170dd-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="170dd-126">Genişletici biçimlendirme sonra şu şekilde görünebilir:</span><span class="sxs-lookup"><span data-stu-id="170dd-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

<span data-ttu-id="170dd-127">Tarayıcıda dosyasını çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="170dd-127">Run the file in the browser.</span></span> <span data-ttu-id="170dd-128">Düğmesini tıklattığınızda, geçerli saati panelinde, her zaman bir saniye boyunca Soluklaşan gösterilir.</span><span class="sxs-lookup"><span data-stu-id="170dd-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="170dd-129">[![Geçerli saati Soluklaşan](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="170dd-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span></span>

<span data-ttu-id="170dd-130">Geçerli saati Soluklaşan ([tam boyutlu görüntüyü görüntülemek için tıklatın](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="170dd-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="170dd-131">[Önceki](animating-an-updatepanel-control-cs.md)
[sonraki](adding-animation-to-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="170dd-131">[Previous](animating-an-updatepanel-control-cs.md)
[Next](adding-animation-to-a-control-vb.md)</span></span>
