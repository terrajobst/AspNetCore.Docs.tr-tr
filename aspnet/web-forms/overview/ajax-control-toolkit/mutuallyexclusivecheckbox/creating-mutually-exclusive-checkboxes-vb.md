---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Birbirini dışlayan onay kutularını (VB) oluşturma | Microsoft Docs
author: wenz
description: 'Radyo düğmeleri, genellikle bir seçenek kümesi yalnızca biri seçilebilir olduğunda kullanılır. Var olan bir dezavantajı ancak: bir radyo düğmesi grubundaki seçildikten sonra...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: cdb93a080fb62cfdc7e3ff0604a1447e2bb99071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874258"
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="ca696-104">Birbirini dışlayan onay kutularını (VB) oluşturma</span><span class="sxs-lookup"><span data-stu-id="ca696-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>
====================
<span data-ttu-id="ca696-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ca696-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ca696-106">[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ca696-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="ca696-107">Radyo düğmeleri, genellikle bir seçenek kümesi yalnızca biri seçilebilir olduğunda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ca696-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="ca696-108">Var olan bir dezavantajı ancak: bir radyo düğmesi grubundaki seçildikten sonra tüm radyo düğmeleri işaretini mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ca696-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="ca696-109">Onay kutuları herhangi bir zamanda denetlenmeyen olabilir, ancak birbirini dışlamaz.</span><span class="sxs-lookup"><span data-stu-id="ca696-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="ca696-110">Bu öğreticide her iki yaklaşımın en iyi sağlar: onay kutularını, karşılıklı olarak birbirini dışlar.</span><span class="sxs-lookup"><span data-stu-id="ca696-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="ca696-111">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="ca696-111">Overview</span></span>

<span data-ttu-id="ca696-112">Radyo düğmeleri, genellikle bir seçenek kümesi yalnızca biri seçilebilir olduğunda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ca696-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="ca696-113">Var olan bir dezavantajı ancak: bir radyo düğmesi grubundaki seçildikten sonra tüm radyo düğmeleri işaretini mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ca696-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="ca696-114">Onay kutuları herhangi bir zamanda denetlenmeyen olabilir, ancak birbirini dışlamaz.</span><span class="sxs-lookup"><span data-stu-id="ca696-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="ca696-115">Bu öğreticide her iki yaklaşımın en iyi sağlar: onay kutularını, karşılıklı olarak birbirini dışlar.</span><span class="sxs-lookup"><span data-stu-id="ca696-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="ca696-116">Adımlar</span><span class="sxs-lookup"><span data-stu-id="ca696-116">Steps</span></span>

<span data-ttu-id="ca696-117">ASP.NET AJAX Denetim Araç Seti MutuallyExclusiveCheckBox genişletici içerir.</span><span class="sxs-lookup"><span data-stu-id="ca696-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="ca696-118">Bu bir onay kutusu için bir grup adı atamak programcıları sağlar (`Key` özniteliği).</span><span class="sxs-lookup"><span data-stu-id="ca696-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="ca696-119">Aynı gruptaki tüm onay kutularından tek seferde seçilebilir.</span><span class="sxs-lookup"><span data-stu-id="ca696-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="ca696-120">Yeni bir ASP.NET sayfasında iki onay kutularını koyma ile başlayalım.</span><span class="sxs-lookup"><span data-stu-id="ca696-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="ca696-121">Daha fazla da olabilir, ancak ikisi ilkesini göstermek için yeterli:</span><span class="sxs-lookup"><span data-stu-id="ca696-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="ca696-122">Her iki onay kutularını MutuallyExclusiveCheckBoxExtender denetim sayfada konulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ca696-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="ca696-123">Her iki anahtar öznitelik aynı değere, yalnızca HTML radyo düğmesi öğeleri özniteliklerini ait grup belirtmek için özdeş olması gereken değeri olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ca696-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="ca696-124">Kimliği onay kutusunun genişletici noktalarına TargetControlID özelliği.</span><span class="sxs-lookup"><span data-stu-id="ca696-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="ca696-125">Son olarak, ASP.NET AJAX dahil `ScriptManager` ASP.NET AJAX Denetim Araç Seti tüm öğeleri tarafından gereklidir:</span><span class="sxs-lookup"><span data-stu-id="ca696-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="ca696-126">Kaydet ve sayfayı çalıştırın: denetleyin ve her iki onay kutularının işaretini hiçbir zaman her iki onay kutusu ancak denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="ca696-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="ca696-127">[![Aynı anda yalnızca bir onay kutusu denetlenebilir](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ca696-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="ca696-128">Yalnızca bir onay kutusu aynı anda kontrol ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ca696-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ca696-129">Önceki</span><span class="sxs-lookup"><span data-stu-id="ca696-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
