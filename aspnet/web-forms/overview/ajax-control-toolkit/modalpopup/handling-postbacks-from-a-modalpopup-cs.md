---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Bir modalpopup'ı (C#) gelen geri göndermeleri işleme | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Bir pos olduğunda özel dikkatli olunması gerekir...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 9bcb1ad058b800f3f957cafff07a5a54b44178a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756356"
---
<a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="e9642-104">Bir modalpopup'ı (C#) gelen geri göndermeleri işleme</span><span class="sxs-lookup"><span data-stu-id="e9642-104">Handling Postbacks from a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="e9642-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e9642-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e9642-106">[Kodu indir](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e9642-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="e9642-107">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="e9642-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="e9642-108">Bir geri gönderme içinde açılan oluşturulduğunda, özel dikkatli olunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e9642-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="e9642-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="e9642-109">Overview</span></span>

<span data-ttu-id="e9642-110">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="e9642-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="e9642-111">Bir geri gönderme içinde açılan oluşturulduğunda, özel dikkatli olunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e9642-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="e9642-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="e9642-112">Steps</span></span>

<span data-ttu-id="e9642-113">ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="e9642-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="e9642-114">Ardından, kalıcı açılan hizmet veren bir panel ekleme.</span><span class="sxs-lookup"><span data-stu-id="e9642-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="e9642-115">Burada, kullanıcı bir ad ve e-posta adresini girebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9642-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="e9642-116">Bir düğme, açılan kapatın ve bilgileri kaydetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e9642-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="e9642-117">Unutmayın `OnClick` özniteliği, bu düğmeye tıklandığında bir geri gönderme gerçekleşmesi ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="e9642-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="e9642-118">Sayfa için tam olarak aynı bilgilerin iki etiket oluşur: ad ve e-posta adresi.</span><span class="sxs-lookup"><span data-stu-id="e9642-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="e9642-119">Bir düğme kalıcı açılan tetiklemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="e9642-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="e9642-120">Açılan pencere görünür hale getirmek için ekleme `ModalPopupExtender` denetimi.</span><span class="sxs-lookup"><span data-stu-id="e9642-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="e9642-121">Ayarlama `PopupControlID` özniteliği bölmenin Kimliğine ve `TargetControlID` düğmenin kimliği:</span><span class="sxs-lookup"><span data-stu-id="e9642-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="e9642-122">Artık her `Save` içinde kalıcı açılan düğmesine tıklandığında, sunucu tarafı `SaveData()` yöntemi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="e9642-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="e9642-123">Burada, girilen verileri bir veri deposunda tasarruf sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9642-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="e9642-124">Basitleştirmek amacıyla, yeni verilerin yalnızca etikette çıktı:</span><span class="sxs-lookup"><span data-stu-id="e9642-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="e9642-125">Ayrıca, textbox denetimi içinde kalıcı açılan geçerli bir ad ve e-posta ile doldurulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e9642-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="e9642-126">Hiçbir geri gönderme gerçekleştiğinde Ancak bu yalnızca gereklidir.</span><span class="sxs-lookup"><span data-stu-id="e9642-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="e9642-127">Bir geri gönderme varsa, ASP.NET viewstate özellik metin kutuları uygun değerlerle otomatik olarak doldurur.</span><span class="sxs-lookup"><span data-stu-id="e9642-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


<span data-ttu-id="e9642-128">[![Kalıcı açılan geri göndermeye neden olur.](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e9642-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="e9642-129">Kalıcı açılan geri göndermeye neden olur ([tam boyutlu görüntüyü görmek için tıklatın](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e9642-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e9642-130">[Önceki](using-modalpopup-with-a-repeater-control-cs.md)
> [İleri](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e9642-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
