---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: UpdatePanel animasyonlarını (VB) dinamik olarak denetleme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. İçeriği için bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 79a256f5d5c62123184beffce3365ade0ac13e46
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365947"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a><span data-ttu-id="deafc-104">UpdatePanel animasyonlarını (VB) dinamik olarak denetleme</span><span class="sxs-lookup"><span data-stu-id="deafc-104">Dynamically Controlling UpdatePanel Animations (VB)</span></span>
====================
<span data-ttu-id="deafc-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="deafc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="deafc-106">[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="deafc-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span></span>

> <span data-ttu-id="deafc-107">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="deafc-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="deafc-108">UpdatePanel içeriğini için mevcut özel genişletici olan yoğun animasyon Framework'te kullanır: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="deafc-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="deafc-109">Ayrıca, UpdatePanel tetikleyicilerini birlikte de çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="deafc-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="deafc-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="deafc-110">Overview</span></span>

<span data-ttu-id="deafc-111">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="deafc-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="deafc-112">İçeriği için bir `UpdatePanel`, yoğun animasyon framework kullanan özel genişletici var: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="deafc-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="deafc-113">Ayrıca ile birlikte çalışabilir `UpdatePanel` tetikler.</span><span class="sxs-lookup"><span data-stu-id="deafc-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="deafc-114">Adımlar</span><span class="sxs-lookup"><span data-stu-id="deafc-114">Steps</span></span>

<span data-ttu-id="deafc-115">İlk adım zamanki dahil etmektir `ScriptManager` sayfasında ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="deafc-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

<span data-ttu-id="deafc-116">Bu senaryoda animasyon geçerli zaman bir ekrana uygulanır.</span><span class="sxs-lookup"><span data-stu-id="deafc-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="deafc-117">Bu bilgileri kullanarak bir etiket yazılabilir `Page_Load()` yöntemi veya aşağıdaki satır içi kod (basitleştirmek amacıyla) kullanılır:</span><span class="sxs-lookup"><span data-stu-id="deafc-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

<span data-ttu-id="deafc-118">Ayrıca, bir düğme tetikleyicisi sürekli güncelleştirmek için oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="deafc-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

<span data-ttu-id="deafc-119">Bu kod içine yerleştirilir `<ContentTemplate>` bölümünü bir `UpdatePanel` öğesi.</span><span class="sxs-lookup"><span data-stu-id="deafc-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="deafc-120">Bölmenin `UpdateMode` özniteliği ayarlanmalıdır `"Conditional"`, olduğundan yalnızca Tetikleyicileri bölmenin içeriği güncelleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="deafc-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="deafc-121">İçinde `<Triggers>` bölümünü `UpdatePanel`, bir zaman uyumsuz geri gönderme tetikleyici oluşturulur ve bağlı `Click` düğmesinin olayı.</span><span class="sxs-lookup"><span data-stu-id="deafc-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="deafc-122">Bu nedenle, kullanıcı düğmesine tıklarsa `UpdatePanel` yenilenir.</span><span class="sxs-lookup"><span data-stu-id="deafc-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="deafc-123">İçin biçimlendirme şöyledir `UpdatePanel` denetimi:</span><span class="sxs-lookup"><span data-stu-id="deafc-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

<span data-ttu-id="deafc-124">Son olarak, `UpdatePanelAnimationExtender` yapılandırılmalıdır: ayarlayın `TargetControlID` özniteliği panel Kimliğine ve bir animasyon Genişleticisi içinde tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="deafc-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="deafc-125">Soluklaşan yapar güncelleştirilmiş zaman iyi bir visual indirimlere oluşturan mantıklı.</span><span class="sxs-lookup"><span data-stu-id="deafc-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="deafc-126">Ardından, genişletici biçimlendirme şöyle görünebilir:</span><span class="sxs-lookup"><span data-stu-id="deafc-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

<span data-ttu-id="deafc-127">Dosyayı tarayıcıda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="deafc-127">Run the file in the browser.</span></span> <span data-ttu-id="deafc-128">Düğmeye tıklattığınızda, geçerli zamanı her zaman bir saniye boyunca Soldurma panelinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="deafc-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="deafc-129">[![Geçerli saati Soluklaşan](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="deafc-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span></span>

<span data-ttu-id="deafc-130">Geçerli saati Soluklaşan ([tam boyutlu görüntüyü görmek için tıklatın](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="deafc-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="deafc-131">Önceki</span><span class="sxs-lookup"><span data-stu-id="deafc-131">Previous</span></span>](animating-an-updatepanel-control-vb.md)
