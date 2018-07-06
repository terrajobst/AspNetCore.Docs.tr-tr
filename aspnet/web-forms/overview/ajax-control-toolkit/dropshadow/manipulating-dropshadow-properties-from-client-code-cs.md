---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: (C#) istemci kodundan DropShadow özelliklerini düzenleme | Microsoft Docs
author: wenz
description: DataList'in düzenleme arabirimini özelleştirme
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 7255e8b2b4c3c5f1c5ebf5a05851c13c07e49c5e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820317"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="b9b59-103">(C#) istemci kodundan DropShadow özelliklerini düzenleme</span><span class="sxs-lookup"><span data-stu-id="b9b59-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="b9b59-104">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b9b59-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b9b59-105">[Kodu indir](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b9b59-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="b9b59-106">AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir.</span><span class="sxs-lookup"><span data-stu-id="b9b59-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="b9b59-107">Bu genişletici özelliklerinin, istemci JavaScript kodunu kullanarak da değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="b9b59-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="b9b59-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="b9b59-108">Overview</span></span>

<span data-ttu-id="b9b59-109">AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir.</span><span class="sxs-lookup"><span data-stu-id="b9b59-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="b9b59-110">Bu genişletici özelliklerinin, istemci JavaScript kodunu kullanarak da değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="b9b59-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="b9b59-111">Adımlar</span><span class="sxs-lookup"><span data-stu-id="b9b59-111">Steps</span></span>

<span data-ttu-id="b9b59-112">Kod, bazı satırlık metin içeren bir paneli ile başlar:</span><span class="sxs-lookup"><span data-stu-id="b9b59-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="b9b59-113">İlişkili CSS sınıfının Masası iyi arka plan rengi verir:</span><span class="sxs-lookup"><span data-stu-id="b9b59-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="b9b59-114">`DropShadowExtender` Panel gölge etkisi, % 50 olarak belirlenmiş opaklık ile genişletmek için eklenir:</span><span class="sxs-lookup"><span data-stu-id="b9b59-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="b9b59-115">Ardından, ASP.NET AJAX `ScriptManager` denetimi çalışmak Denetim Araç Seti sağlar:</span><span class="sxs-lookup"><span data-stu-id="b9b59-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="b9b59-116">Başka bir panel gölge opaklığını ayarlamak için iki JavaScript bağlantılar içeriyor: eksi bağlantı gölgenin opaklık azaltır, artı bağlantıyı da artar.</span><span class="sxs-lookup"><span data-stu-id="b9b59-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="b9b59-117">JavaScript işlevinin `changeOpacity()` sonra ilk bulmalıdır `DropShadowExtender` sayfasında denetimi.</span><span class="sxs-lookup"><span data-stu-id="b9b59-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="b9b59-118">ASP.NET AJAX tanımlar `$find()` tam olarak bu görev için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b9b59-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="b9b59-119">Ardından, `get_Opacity()` yöntemi geçerli opaklığını alır `set_Opacity()` yöntemini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b9b59-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="b9b59-120">JavaScript kodu ardından geçerli opaklık değerini geçirir `<label>` öğe:</span><span class="sxs-lookup"><span data-stu-id="b9b59-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="b9b59-121">[![İstemci tarafında opaklık değiştirilir](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b9b59-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="b9b59-122">İstemci tarafında opaklık değiştirildiğinde ([tam boyutlu görüntüyü görmek için tıklatın](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b9b59-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b9b59-123">[Önceki](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [İleri](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b9b59-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
