---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: (VB) istemci kodundan DropShadow özelliklerini düzenleme | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Bu genişletici özelliklerini kullanarak istemci JavaScrip da değiştirilebilir...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: d8b8330174f6f49e96c42a0e15eeaf934bf24f87
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756142"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="a7ae5-104">(VB) istemci kodundan DropShadow özelliklerini düzenleme</span><span class="sxs-lookup"><span data-stu-id="a7ae5-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="a7ae5-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a7ae5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a7ae5-106">[Kodu indir](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a7ae5-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="a7ae5-107">AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir.</span><span class="sxs-lookup"><span data-stu-id="a7ae5-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a7ae5-108">Bu genişletici özelliklerinin, istemci JavaScript kodunu kullanarak da değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="a7ae5-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="a7ae5-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="a7ae5-109">Overview</span></span>

<span data-ttu-id="a7ae5-110">AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir.</span><span class="sxs-lookup"><span data-stu-id="a7ae5-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a7ae5-111">Bu genişletici özelliklerinin, istemci JavaScript kodunu kullanarak da değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="a7ae5-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="a7ae5-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="a7ae5-112">Steps</span></span>

<span data-ttu-id="a7ae5-113">Kod, bazı satırlık metin içeren bir paneli ile başlar:</span><span class="sxs-lookup"><span data-stu-id="a7ae5-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="a7ae5-114">İlişkili CSS sınıfının Masası iyi arka plan rengi verir:</span><span class="sxs-lookup"><span data-stu-id="a7ae5-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="a7ae5-115">`DropShadowExtender` Panel gölge etkisi, % 50 olarak belirlenmiş opaklık ile genişletmek için eklenir:</span><span class="sxs-lookup"><span data-stu-id="a7ae5-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="a7ae5-116">Ardından, ASP.NET AJAX `ScriptManager` denetimi çalışmak Denetim Araç Seti sağlar:</span><span class="sxs-lookup"><span data-stu-id="a7ae5-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="a7ae5-117">Başka bir panel gölge opaklığını ayarlamak için iki JavaScript bağlantılar içeriyor: eksi bağlantı gölgenin opaklık azaltır, artı bağlantıyı da artar.</span><span class="sxs-lookup"><span data-stu-id="a7ae5-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="a7ae5-118">JavaScript işlevinin `changeOpacity()` sonra ilk bulmalıdır `DropShadowExtender` sayfasında denetimi.</span><span class="sxs-lookup"><span data-stu-id="a7ae5-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="a7ae5-119">ASP.NET AJAX tanımlar `$find()` tam olarak bu görev için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a7ae5-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="a7ae5-120">Ardından, `get_Opacity()` yöntemi geçerli opaklığını alır `set_Opacity()` yöntemini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a7ae5-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="a7ae5-121">JavaScript kodu ardından geçerli opaklık değerini geçirir `<label>` öğe:</span><span class="sxs-lookup"><span data-stu-id="a7ae5-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="a7ae5-122">[![İstemci tarafında opaklık değiştirilir](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a7ae5-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="a7ae5-123">İstemci tarafında opaklık değiştirildiğinde ([tam boyutlu görüntüyü görmek için tıklatın](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a7ae5-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a7ae5-124">Önceki</span><span class="sxs-lookup"><span data-stu-id="a7ae5-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
