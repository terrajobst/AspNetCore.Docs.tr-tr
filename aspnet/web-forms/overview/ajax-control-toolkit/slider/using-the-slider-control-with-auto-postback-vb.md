---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Kaydırıcı denetimi otomatik geri gönderme ile (VB) kullanarak | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar. Kaydırıcı autopost yapmak mümkündür...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: 403e8fa6e54d84eb091769cbdb6ef1003ad4245c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754930"
---
<a name="using-the-slider-control-with-auto-postback-vb"></a><span data-ttu-id="6f382-104">Kaydırıcı denetimi kullanarak otomatik geri gönderme ile (VB)</span><span class="sxs-lookup"><span data-stu-id="6f382-104">Using the Slider Control With Auto-Postback (VB)</span></span>
====================
<span data-ttu-id="6f382-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6f382-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6f382-106">[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6f382-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span></span>

> <span data-ttu-id="6f382-107">AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="6f382-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="6f382-108">Kaydırıcı autopostback değeri değiştiğinde bir kez yapmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6f382-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="6f382-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="6f382-109">Overview</span></span>

<span data-ttu-id="6f382-110">AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="6f382-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="6f382-111">Kaydırıcı autopostback değeri değiştiğinde bir kez yapmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6f382-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="6f382-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="6f382-112">Steps</span></span>

<span data-ttu-id="6f382-113">Kaydırıcı otomatik olarak geri gönderme sırasında bir değişiklik yapmak için iki metin kutusuna öznitelik gerekiyor `AutoPostBack="true"`: kaydırıcı olacak metin kutusu ve kaydırıcının konumunu içeren metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="6f382-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="6f382-114">Söz konusu gerekli biçimlendirmesi şöyledir:</span><span class="sxs-lookup"><span data-stu-id="6f382-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

<span data-ttu-id="6f382-115">`SliderExtender` ASP.NET AJAX Denetim Araç Seti denetiminden iki metin kutuları kaydırıcı işlevi atar:</span><span class="sxs-lookup"><span data-stu-id="6f382-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

<span data-ttu-id="6f382-116">Bir ek label öğesini, daha sonra bir geri gönderme kullanıcıya bildirmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="6f382-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

<span data-ttu-id="6f382-117">Son olarak, `ScriptManager` Denetim ASP.NET AJAX Denetim Araç Seti çalışması için gerekli JavaScript yükler:</span><span class="sxs-lookup"><span data-stu-id="6f382-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

<span data-ttu-id="6f382-118">Kaydırıcı geri gönderen artık; Sunucu tarafında bu olay yakalandı ve izlemede:</span><span class="sxs-lookup"><span data-stu-id="6f382-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


<span data-ttu-id="6f382-119">[![Bir geri gönderme kaydırıcıyı hareket tetikler](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6f382-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span></span>

<span data-ttu-id="6f382-120">Kaydırıcıyı hareket, bir geri gönderme tetikler ([tam boyutlu görüntüyü görmek için tıklatın](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6f382-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span></span>


<span data-ttu-id="6f382-121">[![Daha sonra bu değişiklik tarihini etikette yazılır](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="6f382-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span></span>

<span data-ttu-id="6f382-122">Daha sonra bu değişiklik tarihini etikette yazılır ([tam boyutlu görüntüyü görmek için tıklatın](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="6f382-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6f382-123">[Önceki](databinding-the-slider-control-cs.md)
> [İleri](databinding-the-slider-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6f382-123">[Previous](databinding-the-slider-control-cs.md)
[Next](databinding-the-slider-control-vb.md)</span></span>
