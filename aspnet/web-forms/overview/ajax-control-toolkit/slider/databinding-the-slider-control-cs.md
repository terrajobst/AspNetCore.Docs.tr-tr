---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Veri bağlama kaydırıcı denetimi (C#) | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar. Geçerli konum bağlamak mümkündür...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7644c991cd88868235511ba372be1f5b47c68fea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870589"
---
<a name="databinding-the-slider-control-c"></a><span data-ttu-id="7e57d-104">Veri bağlama kaydırıcı denetimi (C#)</span><span class="sxs-lookup"><span data-stu-id="7e57d-104">Databinding the Slider Control (C#)</span></span>
====================
<span data-ttu-id="7e57d-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7e57d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7e57d-106">[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7e57d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="7e57d-107">AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="7e57d-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="7e57d-108">Başka bir ASP.NET denetimi kaydırıcıyı geçerli konumunu bağlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="7e57d-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="7e57d-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="7e57d-109">Overview</span></span>

<span data-ttu-id="7e57d-110">AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="7e57d-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="7e57d-111">Başka bir ASP.NET denetimi kaydırıcıyı geçerli konumunu bağlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="7e57d-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="7e57d-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="7e57d-112">Steps</span></span>

<span data-ttu-id="7e57d-113">ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="7e57d-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="7e57d-114">Ardından, iki eklemek `TextBox` sayfasına denetimler.</span><span class="sxs-lookup"><span data-stu-id="7e57d-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="7e57d-115">Bir grafik kaydırıcı dönüştürülür ve diğerinde kaydırıcıyı konumunu tutacaktır.</span><span class="sxs-lookup"><span data-stu-id="7e57d-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="7e57d-116">Sonraki adım zaten son adımdır.</span><span class="sxs-lookup"><span data-stu-id="7e57d-116">The next step is already the final step.</span></span> <span data-ttu-id="7e57d-117">`SliderExtender` ASP.NET AJAX Denetim Araç Seti denetiminden ilk metin kutusunun dışında bir kaydırıcı yapar ve kaydırıcıyı getirin değiştiğinde ikinci metin kutusu otomatik olarak güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="7e57d-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="7e57d-118">Çalışmak, o sırada `SliderExtender`'s `TargetControlID` özniteliği ilk metin kutusunun; Kimliğine ayarlanmalıdır `BoundControlID` özniteliği ikinci metin kutusunun Kimliğine ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="7e57d-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="7e57d-119">Veri bağlama tarayıcıda gördüğünüz gibi her iki yönde de çalışır: metin kutusuna yeni bir değer girme kaydırıcının konumunu güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="7e57d-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="7e57d-120">Salt okunur ikinci metin kutusu yaparsanız, böylece kullanıcının orada değerini el ile güncelleştirmeniz daha zayıf bir koruma metin alanı ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="7e57d-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="7e57d-121">[![Kaydırıcı ve metin kutusu eşitleme](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7e57d-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="7e57d-122">Kaydırıcı ve metin kutusu eşitlenmiş ([tam boyutlu görüntüyü görüntülemek için tıklatın](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7e57d-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7e57d-123">[Önceki](using-the-slider-control-with-auto-postback-cs.md)
> [sonraki](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7e57d-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
