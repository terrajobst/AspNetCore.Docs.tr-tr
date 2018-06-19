---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: İstemci kodu (VB) DropShadow özelliklerinden düzenleme | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir. Bu genişletici özelliklerini istemci JavaScrip kullanılarak da değiştirilebilir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b5b024811ea511e67ce180169de9f0b7e3ef51d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870914"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="bd15a-104">İstemci kodu (VB) DropShadow özelliklerini düzenleme</span><span class="sxs-lookup"><span data-stu-id="bd15a-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="bd15a-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bd15a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bd15a-106">[Kodu indirme](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="bd15a-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="bd15a-107">AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir.</span><span class="sxs-lookup"><span data-stu-id="bd15a-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="bd15a-108">Bu genişletici özelliklerini kullanarak istemci JavaScript kodu da değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="bd15a-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="bd15a-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="bd15a-109">Overview</span></span>

<span data-ttu-id="bd15a-110">AJAX Denetim Araç Seti DropShadow denetiminde gölge paneliyle genişletir.</span><span class="sxs-lookup"><span data-stu-id="bd15a-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="bd15a-111">Bu genişletici özelliklerini kullanarak istemci JavaScript kodu da değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="bd15a-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="bd15a-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="bd15a-112">Steps</span></span>

<span data-ttu-id="bd15a-113">Kod bazı satırlık metin içeren bir panel ile başlar:</span><span class="sxs-lookup"><span data-stu-id="bd15a-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="bd15a-114">İlişkili CSS sınıf Masası iyi arka plan rengi sağlar:</span><span class="sxs-lookup"><span data-stu-id="bd15a-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="bd15a-115">`DropShadowExtender` Bir gölge efekti, % 50 olarak belirlenmiş opaklık paneliyle genişletmek için eklendi:</span><span class="sxs-lookup"><span data-stu-id="bd15a-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="bd15a-116">Ardından, ASP.NET AJAX `ScriptManager` denetimi çalışmak Denetim Araç Seti sağlar:</span><span class="sxs-lookup"><span data-stu-id="bd15a-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="bd15a-117">Başka bir panel gölge geçirgenliğini ayarlamak için iki JavaScript bağlantılar içerir: eksi bağlantı gölgenin opaklık azaltır, artı bağlantı da artırır.</span><span class="sxs-lookup"><span data-stu-id="bd15a-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="bd15a-118">JavaScript işlevinin `changeOpacity()` sonra ilk bulmalıdır `DropShadowExtender` sayfasında denetimi.</span><span class="sxs-lookup"><span data-stu-id="bd15a-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="bd15a-119">ASP.NET AJAX tanımlar `$find()` tam olarak bu görev için yöntem.</span><span class="sxs-lookup"><span data-stu-id="bd15a-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="bd15a-120">Ardından, `get_Opacity()` yöntemi alır Geçerli Opaklık `set_Opacity()` yöntemini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="bd15a-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="bd15a-121">JavaScript kodu ardından geçerli geçirgenlik değeri geçirir `<label>` öğe:</span><span class="sxs-lookup"><span data-stu-id="bd15a-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="bd15a-122">[![Opaklık istemci tarafında değiştirilir](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bd15a-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="bd15a-123">Opaklık istemci tarafında değiştirilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bd15a-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bd15a-124">Önceki</span><span class="sxs-lookup"><span data-stu-id="bd15a-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
