---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: (C#) kaydırıcı denetiminde veri bağlama | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar. Geçerli konum bağlamak mümkündür...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: b7aebb8dd180113b011ac038e8da4a3baa701485
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818669"
---
<a name="databinding-the-slider-control-c"></a><span data-ttu-id="bd733-104">Veri bağlama kaydırıcı denetimi (C#)</span><span class="sxs-lookup"><span data-stu-id="bd733-104">Databinding the Slider Control (C#)</span></span>
====================
<span data-ttu-id="bd733-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bd733-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bd733-106">[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bd733-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="bd733-107">AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd733-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="bd733-108">Geçerli konumun slider'ın başka bir ASP.NET denetime bağlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="bd733-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="bd733-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="bd733-109">Overview</span></span>

<span data-ttu-id="bd733-110">AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd733-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="bd733-111">Geçerli konumun slider'ın başka bir ASP.NET denetime bağlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="bd733-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="bd733-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="bd733-112">Steps</span></span>

<span data-ttu-id="bd733-113">ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="bd733-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="bd733-114">Ardından, iki ekleyin `TextBox` sayfasına denetimleri.</span><span class="sxs-lookup"><span data-stu-id="bd733-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="bd733-115">Bir grafik bir kaydırıcı dönüştürülür ve diğer bir kaydırıcının konumu tutar.</span><span class="sxs-lookup"><span data-stu-id="bd733-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="bd733-116">Sonraki adım zaten son adımdır.</span><span class="sxs-lookup"><span data-stu-id="bd733-116">The next step is already the final step.</span></span> <span data-ttu-id="bd733-117">`SliderExtender` ASP.NET AJAX Denetim Araç Seti denetiminden dışında ilk metin kutusuna bir kaydırıcı yapar ve kaydırıcıyı değişiklikleri getirdiğinizde ikinci metin kutusunda otomatik olarak güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="bd733-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="bd733-118">Çalışmak, o sırada `SliderExtender`'s `TargetControlID` özniteliği ilk metin kutusuna; Kimliğine ayarlanmalıdır `BoundControlID` özniteliği ikinci metin kutusunda Kimliğine ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bd733-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="bd733-119">Tarayıcıda gördüğünüz gibi her iki yönde veri bağlama çalışır: yeni bir değer metin kutusuna girerek kaydırıcının konumunu güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="bd733-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="bd733-120">İkinci metin kutusunda salt okunur yaparsanız, böylece orada değeri el ile güncelleştirmek kullanıcının daha zayıf bir koruma metin alanına ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bd733-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="bd733-121">[![Kaydırıcı ve metin kutusu eşitlenmiş durumda](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bd733-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="bd733-122">Kaydırıcı ve metin kutusu eşitlenmiş ([tam boyutlu görüntüyü görmek için tıklatın](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bd733-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bd733-123">[Önceki](using-the-slider-control-with-auto-postback-cs.md)
> [İleri](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="bd733-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
