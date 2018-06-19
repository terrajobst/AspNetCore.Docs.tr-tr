---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Geri göndermeler bir ModalPopup (C#) gelen işleme | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Özellikle dikkatli bir pos olduğunda gerçekleştirilecek gerekir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 183725db62ba8b4037f368ed9d87d5059e3f1bcb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873738"
---
<a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="5b103-104">İşleme Geri göndermeler gelen ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="5b103-104">Handling Postbacks from a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="5b103-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5b103-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5b103-106">[Kodu indirme](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5b103-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="5b103-107">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="5b103-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="5b103-108">Geri gönderimin içinde açılan oluşturulduğunda, özellikle dikkatli olunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5b103-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="5b103-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="5b103-109">Overview</span></span>

<span data-ttu-id="5b103-110">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="5b103-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="5b103-111">Geri gönderimin içinde açılan oluşturulduğunda, özellikle dikkatli olunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5b103-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="5b103-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="5b103-112">Steps</span></span>

<span data-ttu-id="5b103-113">ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="5b103-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="5b103-114">Ardından, kalıcı açılan hizmet veren bir panel ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5b103-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="5b103-115">Burada, kullanıcı bir ad ve e-posta adresi girebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5b103-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="5b103-116">Bir düğme açılan kapatın ve bilgileri kaydetmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5b103-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="5b103-117">Unutmayın `OnClick` özniteliği, böylece bu düğmeye tıklandığında geri gönderimin oluştuğu ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="5b103-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="5b103-118">İki etiketlerini tam olarak aynı bilgiler için sayfa oluşur: ad ve e-posta adresi.</span><span class="sxs-lookup"><span data-stu-id="5b103-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="5b103-119">Bir düğme kalıcı açılan tetiklemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="5b103-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="5b103-120">Açılan pencere görünür yapmak için add `ModalPopupExtender` denetim.</span><span class="sxs-lookup"><span data-stu-id="5b103-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="5b103-121">Ayarlama `PopupControlID` özniteliği bölmenin Kimliğine ve `TargetControlID` düğmenin kimliği:</span><span class="sxs-lookup"><span data-stu-id="5b103-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="5b103-122">Artık her `Save` kalıcı açılır içindeki düğmesine tıklandığında, sunucu tarafı `SaveData()` yöntemi yürütüldüğünde.</span><span class="sxs-lookup"><span data-stu-id="5b103-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="5b103-123">Burada, bir veri deposunda girilen veriler kaydedebilir.</span><span class="sxs-lookup"><span data-stu-id="5b103-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="5b103-124">Basitleştirmek amacıyla, yeni verileri yalnızca etiketinde çıktı:</span><span class="sxs-lookup"><span data-stu-id="5b103-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="5b103-125">Ayrıca, textbox denetimleri kalıcı açılır içindeki geçerli bir ad ve e-posta ile doldurulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5b103-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="5b103-126">Hiçbir geri gönderme oluştuğunda ancak bu yalnızca gereklidir.</span><span class="sxs-lookup"><span data-stu-id="5b103-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="5b103-127">Geri gönderimin ise, ASP.NET viewstate özelliği otomatik olarak metin kutuları uygun değerlerle doldurun.</span><span class="sxs-lookup"><span data-stu-id="5b103-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


<span data-ttu-id="5b103-128">[![Kalıcı açılan geri gönderimin neden olur.](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5b103-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="5b103-129">Kalıcı açılan geri gönderimin neden olur ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5b103-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5b103-130">[Önceki](using-modalpopup-with-a-repeater-control-cs.md)
> [sonraki](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="5b103-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
