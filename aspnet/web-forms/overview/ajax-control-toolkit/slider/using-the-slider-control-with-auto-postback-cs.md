---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Kaydırıcı denetimi otomatik-geri gönderme ile (C#) kullanarak | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar. Kaydırıcı autopost yapmak mümkündür...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: e347d20c5c2ee48e6ed801e95459af6f0bcd2667
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="150f5-104">Kaydırıcı denetimi kullanarak otomatik-geri gönderme ile (C#)</span><span class="sxs-lookup"><span data-stu-id="150f5-104">Using the Slider Control With Auto-Postback (C#)</span></span>
====================
<span data-ttu-id="150f5-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="150f5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="150f5-106">[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="150f5-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="150f5-107">AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="150f5-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="150f5-108">Kaydırıcı autopostback kez kendi değer değişiklik mümkündür.</span><span class="sxs-lookup"><span data-stu-id="150f5-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="150f5-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="150f5-109">Overview</span></span>

<span data-ttu-id="150f5-110">AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="150f5-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="150f5-111">Kaydırıcı autopostback kez kendi değer değişiklik mümkündür.</span><span class="sxs-lookup"><span data-stu-id="150f5-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="150f5-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="150f5-112">Steps</span></span>

<span data-ttu-id="150f5-113">Kaydırıcıyı bir değişikliklerinde otomatik olarak geri gönderme yapmak için her iki metin kutuları öznitelik gereksinim `AutoPostBack="true"`: kaydırıcı olacak metin kutusu ve kaydırıcının konumunu içeren metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="150f5-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="150f5-114">Söz konusu gerekli biçimlendirme aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="150f5-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="150f5-115">`SliderExtender` ASP.NET AJAX Denetim Araç Seti denetiminden iki metin kutuları kaydırıcı işlevselliği atar:</span><span class="sxs-lookup"><span data-stu-id="150f5-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="150f5-116">Bir ek label öğesi, daha sonra geri gönderimin kullanıcı bildirmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="150f5-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="150f5-117">Son olarak, `ScriptManager` Denetim ASP.NET AJAX Denetim Araç Seti çalışması için gerekli JavaScript yükler:</span><span class="sxs-lookup"><span data-stu-id="150f5-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="150f5-118">Şimdi kaydırıcıyı geri nakil; Sunucu tarafında bu olay yakalanan ve üzerinde işlem:</span><span class="sxs-lookup"><span data-stu-id="150f5-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


<span data-ttu-id="150f5-119">[![Kaydırıcıyı hareket geri gönderimin tetikler](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="150f5-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="150f5-120">Kaydırıcıyı hareket tetikler geri gönderimin ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="150f5-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>


<span data-ttu-id="150f5-121">[![Daha sonra bu değişiklik tarihi etiketinde yazılır](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="150f5-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="150f5-122">Daha sonra bu değişiklik tarihi etiketinde yazılır ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="150f5-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="150f5-123">Next</span><span class="sxs-lookup"><span data-stu-id="150f5-123">Next</span></span>](databinding-the-slider-control-cs.md)
