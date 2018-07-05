---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Bir modalpopup'ı (C#) konumlandırma | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Ancak denetim teklif değil bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: d76b74cf1cef1196298e5dbab2b1a6acaf7e5a04
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388470"
---
<a name="positioning-a-modalpopup-c"></a><span data-ttu-id="53c3a-104">Bir modalpopup'ı (C#) konumlandırma</span><span class="sxs-lookup"><span data-stu-id="53c3a-104">Positioning a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="53c3a-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="53c3a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="53c3a-106">[Kodu indir](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="53c3a-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span></span>

> <span data-ttu-id="53c3a-107">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="53c3a-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="53c3a-108">Ancak denetim açılan konumlandırmak için yerleşik bir işlevsellik sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="53c3a-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="53c3a-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="53c3a-109">Overview</span></span>

<span data-ttu-id="53c3a-110">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="53c3a-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="53c3a-111">Ancak denetim açılan konumlandırmak için yerleşik bir işlevsellik sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="53c3a-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="53c3a-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="53c3a-112">Steps</span></span>

<span data-ttu-id="53c3a-113">ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="53c3a-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="53c3a-114">Denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="53c3a-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="53c3a-115">Ardından, kalıcı açılan hizmet veren bir panel ekleme.</span><span class="sxs-lookup"><span data-stu-id="53c3a-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="53c3a-116">Bir düğme, açılan kapatmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="53c3a-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="53c3a-117">Açılan gösterilen zaman, belirli bir sayfayı yerde konumlandırılmış.</span><span class="sxs-lookup"><span data-stu-id="53c3a-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="53c3a-118">Bu görev için bir istemci tarafı JavaScript işlevi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="53c3a-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="53c3a-119">Paneline erişmek önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="53c3a-119">It first tries to access the panel.</span></span> <span data-ttu-id="53c3a-120">Başarılı olursa, bölmenin konumu, CSS ve JavaScript (olur açılan pencerenin konumunu değiştir) kullanılarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="53c3a-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="53c3a-121">Ancak `ModalPopupExtender` denetim açılan konumlandırmak de çalışır.</span><span class="sxs-lookup"><span data-stu-id="53c3a-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="53c3a-122">Bu nedenle, JavaScript kodu tekrar tekrar açılan, saniyenin her onda yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="53c3a-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

<span data-ttu-id="53c3a-123">Gördüğünüz gibi dönüş değeri `setTimeout()` JavaScript yöntemi, bir genel değişken kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="53c3a-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="53c3a-124">İsteğe bağlı olarak, açılan yinelenen konumlandırma durdurmak için böylece kullanarak `clearTimeout()` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="53c3a-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

<span data-ttu-id="53c3a-125">Şimdi yapılması gereken şey bu işlevler, uygun olduğunda çağrı tarayıcısı yapmak için.</span><span class="sxs-lookup"><span data-stu-id="53c3a-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="53c3a-126">`movePanel()` Paneli tetikleyen düğmesine tıklandığında, JavaScript işlevi çağrılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="53c3a-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

<span data-ttu-id="53c3a-127">Ve `stopMoving()` işlevi, bu açılır penceresi kapatıldığında oyuna gelir tetiklenen içinde `ModalPopupExtender` denetimi:</span><span class="sxs-lookup"><span data-stu-id="53c3a-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


<span data-ttu-id="53c3a-128">[![Belirtilen konumda kalıcı açılan pencere görünür](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="53c3a-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="53c3a-129">Belirtilen konumda kalıcı açılan pencere görünür ([tam boyutlu görüntüyü görmek için tıklatın](positioning-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="53c3a-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="53c3a-130">[Önceki](handling-postbacks-from-a-modalpopup-cs.md)
> [İleri](launching-a-modal-popup-window-from-server-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="53c3a-130">[Previous](handling-postbacks-from-a-modalpopup-cs.md)
[Next](launching-a-modal-popup-window-from-server-code-vb.md)</span></span>
