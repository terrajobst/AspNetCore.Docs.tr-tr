---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Geri göndermeler ModalPopup (VB) gelen işleme | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Özellikle dikkatli bir pos olduğunda gerçekleştirilecek gerekir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 1aa2c42deb67015dd0b35edf4ba72d8d667ec88c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="278b0-104">Geri göndermeler ModalPopup (VB) gelen işleme</span><span class="sxs-lookup"><span data-stu-id="278b0-104">Handling Postbacks from a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="278b0-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="278b0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="278b0-106">[Kodu indirme](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="278b0-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="278b0-107">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="278b0-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="278b0-108">Geri gönderimin içinde açılan oluşturulduğunda, özellikle dikkatli olunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="278b0-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="278b0-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="278b0-109">Overview</span></span>

<span data-ttu-id="278b0-110">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="278b0-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="278b0-111">Geri gönderimin içinde açılan oluşturulduğunda, özellikle dikkatli olunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="278b0-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="278b0-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="278b0-112">Steps</span></span>

<span data-ttu-id="278b0-113">ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="278b0-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="278b0-114">Ardından, kalıcı açılan hizmet veren bir panel ekleyin.</span><span class="sxs-lookup"><span data-stu-id="278b0-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="278b0-115">Burada, kullanıcı bir ad ve e-posta adresi girebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="278b0-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="278b0-116">Bir düğme açılan kapatın ve bilgileri kaydetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="278b0-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="278b0-117">Unutmayın `OnClick` özniteliği, böylece bu düğmeye tıklandığında geri gönderimin oluştuğu ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="278b0-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="278b0-118">İki etiketlerini tam olarak aynı bilgiler için sayfa oluşur: ad ve e-posta adresi.</span><span class="sxs-lookup"><span data-stu-id="278b0-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="278b0-119">Bir düğme kalıcı açılan tetiklemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="278b0-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="278b0-120">Açılan pencere görünür yapmak için add `ModalPopupExtender` denetim.</span><span class="sxs-lookup"><span data-stu-id="278b0-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="278b0-121">Ayarlama `PopupControlID` özniteliği bölmenin Kimliğine ve `TargetControlID` düğmenin kimliği:</span><span class="sxs-lookup"><span data-stu-id="278b0-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="278b0-122">Artık her `Save` kalıcı açılır içindeki düğmesine tıklandığında, sunucu tarafı `SaveData()` yöntemi yürütüldüğünde.</span><span class="sxs-lookup"><span data-stu-id="278b0-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="278b0-123">Burada, bir veri deposunda girilen veriler kaydedebilir.</span><span class="sxs-lookup"><span data-stu-id="278b0-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="278b0-124">Basitleştirmek amacıyla, yeni verileri yalnızca etiketinde çıktı:</span><span class="sxs-lookup"><span data-stu-id="278b0-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="278b0-125">Ayrıca, textbox denetimleri kalıcı açılır içindeki geçerli bir ad ve e-posta ile doldurulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="278b0-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="278b0-126">Hiçbir geri gönderme oluştuğunda ancak bu yalnızca gereklidir.</span><span class="sxs-lookup"><span data-stu-id="278b0-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="278b0-127">Geri gönderimin ise, ASP.NET viewstate özelliği otomatik olarak metin kutuları uygun değerlerle doldurun.</span><span class="sxs-lookup"><span data-stu-id="278b0-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


<span data-ttu-id="278b0-128">[![Kalıcı açılan geri gönderimin neden olur.](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="278b0-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="278b0-129">Kalıcı açılan geri gönderimin neden olur ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="278b0-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="278b0-130">[Önceki](using-modalpopup-with-a-repeater-control-vb.md)
> [sonraki](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="278b0-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
