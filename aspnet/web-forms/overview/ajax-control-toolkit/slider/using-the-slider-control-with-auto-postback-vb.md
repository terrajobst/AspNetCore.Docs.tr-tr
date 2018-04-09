---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Kaydırıcı denetimi otomatik-geri gönderme ile (VB) kullanarak | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar. Kaydırıcı autopost yapmak mümkündür...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: edb8fa13716c3c0beb7cf86dd3843caaec939483
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-slider-control-with-auto-postback-vb"></a><span data-ttu-id="b423c-104">Otomatik-geri gönderme ile (VB) kaydırıcı denetimi kullanma</span><span class="sxs-lookup"><span data-stu-id="b423c-104">Using the Slider Control With Auto-Postback (VB)</span></span>
====================
<span data-ttu-id="b423c-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b423c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b423c-106">[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b423c-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span></span>

> <span data-ttu-id="b423c-107">AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="b423c-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="b423c-108">Kaydırıcı autopostback kez kendi değer değişiklik mümkündür.</span><span class="sxs-lookup"><span data-stu-id="b423c-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="b423c-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="b423c-109">Overview</span></span>

<span data-ttu-id="b423c-110">AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="b423c-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="b423c-111">Kaydırıcı autopostback kez kendi değer değişiklik mümkündür.</span><span class="sxs-lookup"><span data-stu-id="b423c-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="b423c-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="b423c-112">Steps</span></span>

<span data-ttu-id="b423c-113">Kaydırıcıyı bir değişikliklerinde otomatik olarak geri gönderme yapmak için her iki metin kutuları öznitelik gereksinim `AutoPostBack="true"`: kaydırıcı olacak metin kutusu ve kaydırıcının konumunu içeren metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="b423c-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="b423c-114">Söz konusu gerekli biçimlendirme aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="b423c-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

<span data-ttu-id="b423c-115">`SliderExtender` ASP.NET AJAX Denetim Araç Seti denetiminden iki metin kutuları kaydırıcı işlevselliği atar:</span><span class="sxs-lookup"><span data-stu-id="b423c-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

<span data-ttu-id="b423c-116">Bir ek label öğesi, daha sonra geri gönderimin kullanıcı bildirmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="b423c-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

<span data-ttu-id="b423c-117">Son olarak, `ScriptManager` Denetim ASP.NET AJAX Denetim Araç Seti çalışması için gerekli JavaScript yükler:</span><span class="sxs-lookup"><span data-stu-id="b423c-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

<span data-ttu-id="b423c-118">Şimdi kaydırıcıyı geri nakil; Sunucu tarafında bu olay yakalanan ve üzerinde işlem:</span><span class="sxs-lookup"><span data-stu-id="b423c-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


<span data-ttu-id="b423c-119">[![Kaydırıcıyı hareket geri gönderimin tetikler](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b423c-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span></span>

<span data-ttu-id="b423c-120">Kaydırıcıyı hareket tetikler geri gönderimin ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b423c-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span></span>


<span data-ttu-id="b423c-121">[![Daha sonra bu değişiklik tarihi etiketinde yazılır](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="b423c-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span></span>

<span data-ttu-id="b423c-122">Daha sonra bu değişiklik tarihi etiketinde yazılır ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b423c-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b423c-123">[Önceki](databinding-the-slider-control-cs.md)
> [sonraki](databinding-the-slider-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b423c-123">[Previous](databinding-the-slider-control-cs.md)
[Next](databinding-the-slider-control-vb.md)</span></span>
