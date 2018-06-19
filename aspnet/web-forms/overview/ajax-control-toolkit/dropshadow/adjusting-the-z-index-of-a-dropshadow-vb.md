---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: DropShadow (VB) Z-Index ayarlama | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Ancak bu gölge bazen yükleme konumu için diğer denetimleri çakışıyor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: b484dc6bfa6f67bd6b70f7c36c2eb2ec7143edaf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871304"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="11e30-104">DropShadow (VB) Z-Index ayarlama</span><span class="sxs-lookup"><span data-stu-id="11e30-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="11e30-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="11e30-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="11e30-106">[Kodu indirme](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="11e30-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="11e30-107">AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir.</span><span class="sxs-lookup"><span data-stu-id="11e30-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="11e30-108">Ancak bu gölge, diğer denetimler örneği için ASP.NET menü denetimi çakışıyor.</span><span class="sxs-lookup"><span data-stu-id="11e30-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="11e30-109">Ne zaman bir menü girişi, gölge göründüğü açılır.</span><span class="sxs-lookup"><span data-stu-id="11e30-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="11e30-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="11e30-110">Overview</span></span>

<span data-ttu-id="11e30-111">AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir.</span><span class="sxs-lookup"><span data-stu-id="11e30-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="11e30-112">Ancak bu gölge, diğer denetimler örneği için ASP.NET menü denetimi çakışıyor.</span><span class="sxs-lookup"><span data-stu-id="11e30-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="11e30-113">Ne zaman bir menü girişi, gölge göründüğü açılır.</span><span class="sxs-lookup"><span data-stu-id="11e30-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="11e30-114">Adımlar</span><span class="sxs-lookup"><span data-stu-id="11e30-114">Steps</span></span>

<span data-ttu-id="11e30-115">Bölmenin görünür olmasını etkisi için yeterince metin içeren yeterli metni içeren paneli kendisi ile kod gönderir:</span><span class="sxs-lookup"><span data-stu-id="11e30-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="11e30-116">Başka bir panel hemen öncesine yerleştirilir `panelShadow` paneli.</span><span class="sxs-lookup"><span data-stu-id="11e30-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="11e30-117">Üzerinden menü girişlerini görünmesini sağlayacak şekilde yatay yönlendirme menüsüyle içerir (veya bunun yerine: altında) `dropShadow` Masası):</span><span class="sxs-lookup"><span data-stu-id="11e30-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="11e30-118">Ardından, `DropShadowExtender` genişletmek için eklenen `panelShadow` gölge efekti paneliyle:</span><span class="sxs-lookup"><span data-stu-id="11e30-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="11e30-119">Son olarak, ASP.NET AJAX `ScriptManager` denetimi çalışmak Denetim Araç Seti sağlar:</span><span class="sxs-lookup"><span data-stu-id="11e30-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="11e30-120">Bu komut dosyasını çalıştırdığınızda menü girişlerini paneli altında görünür.</span><span class="sxs-lookup"><span data-stu-id="11e30-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="11e30-121">Ancak menü CSS sınıfı kullanır `panel` yalnızca olduğu öğelerin önünde diğer paneli görünür yapmak için iki şey tanımlamak:</span><span class="sxs-lookup"><span data-stu-id="11e30-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="11e30-122">Göreli konumlandırma</span><span class="sxs-lookup"><span data-stu-id="11e30-122">Relative positioning</span></span>
- <span data-ttu-id="11e30-123">Pozitif z-index</span><span class="sxs-lookup"><span data-stu-id="11e30-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="11e30-124">Ardından, `DropShadowExtender` denetim çakışıp artık menü denetimi ile.</span><span class="sxs-lookup"><span data-stu-id="11e30-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="11e30-125">[![Önce: Menü girişi görünür değil](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="11e30-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="11e30-126">Önce: Menü girişi görünür değil ([tam boyutlu görüntüyü görüntülemek için tıklatın](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="11e30-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="11e30-127">[![Sonra: Menü giriş görüntülenir.](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="11e30-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="11e30-128">Sonra: Menü girişi görünür ([tam boyutlu görüntüyü görüntülemek için tıklatın](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="11e30-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="11e30-129">[Önceki](manipulating-dropshadow-properties-from-client-code-cs.md)
> [sonraki](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="11e30-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
