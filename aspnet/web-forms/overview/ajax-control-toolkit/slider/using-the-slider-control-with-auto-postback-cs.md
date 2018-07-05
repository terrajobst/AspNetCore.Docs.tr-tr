---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Kaydırıcı denetimi otomatik geri gönderme ile (C#) kullanma | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar. Kaydırıcı autopost yapmak mümkündür...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: 8e855bebf557536b5f9e2b2730f828e54c63221e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366838"
---
<a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="96770-104">Kaydırıcı denetimi otomatik geri gönderme ile (C#) kullanma</span><span class="sxs-lookup"><span data-stu-id="96770-104">Using the Slider Control With Auto-Postback (C#)</span></span>
====================
<span data-ttu-id="96770-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="96770-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="96770-106">[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="96770-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="96770-107">AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="96770-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="96770-108">Kaydırıcı autopostback değeri değiştiğinde bir kez yapmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="96770-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="96770-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="96770-109">Overview</span></span>

<span data-ttu-id="96770-110">AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="96770-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="96770-111">Kaydırıcı autopostback değeri değiştiğinde bir kez yapmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="96770-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="96770-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="96770-112">Steps</span></span>

<span data-ttu-id="96770-113">Kaydırıcı otomatik olarak geri gönderme sırasında bir değişiklik yapmak için iki metin kutusuna öznitelik gerekiyor `AutoPostBack="true"`: kaydırıcı olacak metin kutusu ve kaydırıcının konumunu içeren metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="96770-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="96770-114">Söz konusu gerekli biçimlendirmesi şöyledir:</span><span class="sxs-lookup"><span data-stu-id="96770-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="96770-115">`SliderExtender` ASP.NET AJAX Denetim Araç Seti denetiminden iki metin kutuları kaydırıcı işlevi atar:</span><span class="sxs-lookup"><span data-stu-id="96770-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="96770-116">Bir ek label öğesini, daha sonra bir geri gönderme kullanıcıya bildirmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="96770-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="96770-117">Son olarak, `ScriptManager` Denetim ASP.NET AJAX Denetim Araç Seti çalışması için gerekli JavaScript yükler:</span><span class="sxs-lookup"><span data-stu-id="96770-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="96770-118">Kaydırıcı geri gönderen artık; Sunucu tarafında bu olay yakalandı ve izlemede:</span><span class="sxs-lookup"><span data-stu-id="96770-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


<span data-ttu-id="96770-119">[![Bir geri gönderme kaydırıcıyı hareket tetikler](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="96770-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="96770-120">Kaydırıcıyı hareket, bir geri gönderme tetikler ([tam boyutlu görüntüyü görmek için tıklatın](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="96770-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>


<span data-ttu-id="96770-121">[![Daha sonra bu değişiklik tarihini etikette yazılır](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="96770-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="96770-122">Daha sonra bu değişiklik tarihini etikette yazılır ([tam boyutlu görüntüyü görmek için tıklatın](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="96770-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="96770-123">Next</span><span class="sxs-lookup"><span data-stu-id="96770-123">Next</span></span>](databinding-the-slider-control-cs.md)
