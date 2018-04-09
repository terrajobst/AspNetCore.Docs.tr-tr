---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: DropShadow (C#) Z-Index ayarlama | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Ancak bu gölge bazen yükleme konumu için diğer denetimleri çakışıyor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 82add8427c8e574b213b67315e69bb4c28846095
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a><span data-ttu-id="f2510-104">DropShadow (C#) Z-Index ayarlama</span><span class="sxs-lookup"><span data-stu-id="f2510-104">Adjusting the Z-Index of a DropShadow (C#)</span></span>
====================
<span data-ttu-id="f2510-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f2510-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f2510-106">[Kodu indirme](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f2510-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span></span>

> <span data-ttu-id="f2510-107">AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir.</span><span class="sxs-lookup"><span data-stu-id="f2510-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="f2510-108">Ancak bu gölge, diğer denetimler örneği için ASP.NET menü denetimi çakışıyor.</span><span class="sxs-lookup"><span data-stu-id="f2510-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="f2510-109">Ne zaman bir menü girişi, gölge göründüğü açılır.</span><span class="sxs-lookup"><span data-stu-id="f2510-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="f2510-110">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="f2510-110">Overview</span></span>

<span data-ttu-id="f2510-111">AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir.</span><span class="sxs-lookup"><span data-stu-id="f2510-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="f2510-112">Ancak bu gölge, diğer denetimler örneği için ASP.NET menü denetimi çakışıyor.</span><span class="sxs-lookup"><span data-stu-id="f2510-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="f2510-113">Ne zaman bir menü girişi, gölge göründüğü açılır.</span><span class="sxs-lookup"><span data-stu-id="f2510-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="f2510-114">Adımlar</span><span class="sxs-lookup"><span data-stu-id="f2510-114">Steps</span></span>

<span data-ttu-id="f2510-115">Bölmenin görünür olmasını etkisi için yeterince metin içeren yeterli metni içeren paneli kendisi ile kod gönderir:</span><span class="sxs-lookup"><span data-stu-id="f2510-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

<span data-ttu-id="f2510-116">Başka bir panel hemen öncesine yerleştirilir `panelShadow` paneli.</span><span class="sxs-lookup"><span data-stu-id="f2510-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="f2510-117">Üzerinden menü girişlerini görünmesini sağlayacak şekilde yatay yönlendirme menüsüyle içerir (veya bunun yerine: altında) `dropShadow` Masası):</span><span class="sxs-lookup"><span data-stu-id="f2510-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

<span data-ttu-id="f2510-118">Ardından, `DropShadowExtender` genişletmek için eklenen `panelShadow` gölge efekti paneliyle:</span><span class="sxs-lookup"><span data-stu-id="f2510-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

<span data-ttu-id="f2510-119">Son olarak, ASP.NET AJAX `ScriptManager` denetimi çalışmak Denetim Araç Seti sağlar:</span><span class="sxs-lookup"><span data-stu-id="f2510-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

<span data-ttu-id="f2510-120">Bu komut dosyasını çalıştırdığınızda menü girişlerini paneli altında görünür.</span><span class="sxs-lookup"><span data-stu-id="f2510-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="f2510-121">Ancak menü CSS sınıfı kullanır `panel` yalnızca olduğu öğelerin önünde diğer paneli görünür yapmak için iki şey tanımlamak:</span><span class="sxs-lookup"><span data-stu-id="f2510-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="f2510-122">Göreli konumlandırma</span><span class="sxs-lookup"><span data-stu-id="f2510-122">Relative positioning</span></span>
- <span data-ttu-id="f2510-123">Pozitif z-index</span><span class="sxs-lookup"><span data-stu-id="f2510-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

<span data-ttu-id="f2510-124">Ardından, `DropShadowExtender` denetim çakışıp artık menü denetimi ile.</span><span class="sxs-lookup"><span data-stu-id="f2510-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="f2510-125">[![Önce: Menü girişi görünür değil](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f2510-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span></span>

<span data-ttu-id="f2510-126">Önce: Menü girişi görünür değil ([tam boyutlu görüntüyü görüntülemek için tıklatın](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f2510-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span></span>


<span data-ttu-id="f2510-127">[![Sonra: Menü giriş görüntülenir.](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="f2510-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span></span>

<span data-ttu-id="f2510-128">Sonra: Menü girişi görünür ([tam boyutlu görüntüyü görüntülemek için tıklatın](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f2510-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f2510-129">Next</span><span class="sxs-lookup"><span data-stu-id="f2510-129">Next</span></span>](manipulating-dropshadow-properties-from-client-code-cs.md)
