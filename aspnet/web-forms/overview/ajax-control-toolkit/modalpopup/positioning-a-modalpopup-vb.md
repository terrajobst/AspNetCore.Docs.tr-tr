---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: "ModalPopup (VB) konumlandırma | Microsoft Docs"
author: wenz
description: "AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Ancak denetim yok satışa bir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b9c0790d1696bcf478bcdea089d4d3d92450369
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="9294f-104">ModalPopup (VB) konumlandırma</span><span class="sxs-lookup"><span data-stu-id="9294f-104">Positioning a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="9294f-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9294f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9294f-106">[Kodu indirme](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9294f-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="9294f-107">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="9294f-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="9294f-108">Ancak denetim açılan konumlandırmak için yerleşik bir işlevselliği sunmaz.</span><span class="sxs-lookup"><span data-stu-id="9294f-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="9294f-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="9294f-109">Overview</span></span>

<span data-ttu-id="9294f-110">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="9294f-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="9294f-111">Ancak denetim açılan konumlandırmak için yerleşik bir işlevselliği sunmaz.</span><span class="sxs-lookup"><span data-stu-id="9294f-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="9294f-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="9294f-112">Steps</span></span>

<span data-ttu-id="9294f-113">ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="9294f-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="9294f-114">Denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="9294f-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="9294f-115">Ardından, kalıcı açılan hizmet veren bir panel ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9294f-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="9294f-116">Bir düğme açılan kapatmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="9294f-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="9294f-117">Açılan gösterilen her sayfada belirli bir yerde konumlandırılmış.</span><span class="sxs-lookup"><span data-stu-id="9294f-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="9294f-118">Bu görev için bir istemci tarafı JavaScript işlevi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9294f-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="9294f-119">Paneline erişmek ilk önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="9294f-119">It first tries to access the panel.</span></span> <span data-ttu-id="9294f-120">Başarılı olursa, CSS ve JavaScript (açılan konumunu olacak değiştirin) kullanarak bölmenin konumu ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="9294f-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="9294f-121">Ancak `ModalPopupExtender` denetimi de dener açılan getirin.</span><span class="sxs-lookup"><span data-stu-id="9294f-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="9294f-122">Bu nedenle, JavaScript kodu art arda açılan, ikinci bir her onuncu yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="9294f-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="9294f-123">Gördüğünüz gibi dönüş değerini `setTimeout()` JavaScript yöntemi genel bir değişkende kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="9294f-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="9294f-124">Bu isteğe bağlı olarak, açılan yinelenen konumlandırma durdurmak için sağlar kullanarak `clearTimeout()` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="9294f-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="9294f-125">Şimdi yapmak için sol olan uygun olduğunda bu işlev çağrısı tarayıcı yapma.</span><span class="sxs-lookup"><span data-stu-id="9294f-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="9294f-126">`movePanel()` Masası tetikleyen düğme tıklatıldığında JavaScript işlevi çağrılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="9294f-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="9294f-127">Ve `stopMoving()` işlevi gelen oyuna açılan bu kapatıldığında tetiklenen `ModalPopupExtender` denetimi:</span><span class="sxs-lookup"><span data-stu-id="9294f-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


<span data-ttu-id="9294f-128">[![Kalıcı açılan belirlenen konumunda görünür](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9294f-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="9294f-129">Kalıcı açılan belirlenen konumunda görünür ([tam boyutlu görüntüyü görüntülemek için tıklatın](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9294f-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="9294f-130">Önceki</span><span class="sxs-lookup"><span data-stu-id="9294f-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
