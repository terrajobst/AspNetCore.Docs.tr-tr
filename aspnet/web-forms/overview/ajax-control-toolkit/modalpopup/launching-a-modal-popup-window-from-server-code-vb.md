---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: (VB) sunucu kodundan kalıcı açılan pencere başlatma | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Ancak bazı senaryolarda bu t gerektirir...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 79c7281032d8b36d0e4f4b62dcbbad2ab5ebbb77
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756552"
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="ce025-104">(VB) sunucu kodundan kalıcı açılan pencere başlatma</span><span class="sxs-lookup"><span data-stu-id="ce025-104">Launching a Modal Popup Window from Server Code (VB)</span></span>
====================
<span data-ttu-id="ce025-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ce025-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ce025-106">[Kodu indir](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ce025-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="ce025-107">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="ce025-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="ce025-108">Ancak bazı senaryolar kalıcı açılan açılmasına sunucu tarafında tetiklenir gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ce025-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="ce025-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="ce025-109">Overview</span></span>

<span data-ttu-id="ce025-110">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="ce025-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="ce025-111">Ancak bazı senaryolar kalıcı açılan açılmasına sunucu tarafında tetiklenir gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ce025-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="ce025-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="ce025-112">Steps</span></span>

<span data-ttu-id="ce025-113">İlk olarak bir ASP.NET düğme web denetimi ModalPopup denetiminin nasıl çalıştığını göstermek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ce025-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="ce025-114">Böyle bir düğme içinde ekleyin &lt;form&gt; öğe yeni bir sayfa üzerinde:</span><span class="sxs-lookup"><span data-stu-id="ce025-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="ce025-115">Ardından, oluşturmak istediğiniz popup için işaretleme gerekir.</span><span class="sxs-lookup"><span data-stu-id="ce025-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="ce025-116">Olarak tanımlayan bir `<asp:Panel>` denetlemek ve bir düğme denetimi içerdiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="ce025-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="ce025-117">ModalPopup denetim açılan böyle bir düğme yakın hale getirmek için işlevsellik sunar. Aksi takdirde, kaybolur izin vermek için kolay bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="ce025-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="ce025-118">ASP.NET AJAX araç setinin sonraki sayfasına ModalPopup denetim ekleme.</span><span class="sxs-lookup"><span data-stu-id="ce025-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="ce025-119">Denetim yükleyen düğmesi, kayboluyor getiren düğmesi ve gerçek açılan kimliği özelliklerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ce025-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="ce025-120">ASP.NET AJAX üzerinde göre tüm web sayfalarıyla olarak; Betik Manager, farklı tarayıcılar için gerekli JavaScript kitaplıklarını yüklemek için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="ce025-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="ce025-121">Örneğin, tarayıcıda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ce025-121">Run the example in the browser.</span></span> <span data-ttu-id="ce025-122">Düğmeye tıkladığınızda, kalıcı açılan pencere görünür.</span><span class="sxs-lookup"><span data-stu-id="ce025-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="ce025-123">Sunucu tarafı kod kullanarak aynı etkiyi elde etmek için yeni bir düğme gereklidir:</span><span class="sxs-lookup"><span data-stu-id="ce025-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="ce025-124">Gördüğünüz gibi bir düğmeye tıklayarak bir geri gönderme oluşturur ve yürütür `ServerButton_Click()` sunucuda yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ce025-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="ce025-125">Bu yöntemde bir JavaScript işlevi olarak adlandırılan `launchModal()` yürütülür sayfası yüklendikten sonra tam olması için JavaScript işlevinin yürütülecek:</span><span class="sxs-lookup"><span data-stu-id="ce025-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="ce025-126">İşin `launchModal()` ModalPopup görüntülemektir.</span><span class="sxs-lookup"><span data-stu-id="ce025-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="ce025-127">`launchModal()` İşlevi tam HTML sayfası yüklendikten sonra yürütülür.</span><span class="sxs-lookup"><span data-stu-id="ce025-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="ce025-128">Bu arada, Bununla birlikte, ASP.NET AJAX framework henüz tam olarak yüklenmemiş.</span><span class="sxs-lookup"><span data-stu-id="ce025-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="ce025-129">Bu nedenle, `launchModal()` işlevi yalnızca ModalPopup denetim daha sonra gösterilecek bir değişken ayarlar:</span><span class="sxs-lookup"><span data-stu-id="ce025-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="ce025-130">`pageLoad()` JavaScript işlevidir ASP.NET AJAX tamamen yüklendikten sonra yürütülen özel bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ce025-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="ce025-131">Kod ModalPopup denetimidir, ancak yalnızca göstermek için bu işlevi bu nedenle eklediğimiz `launchModal()` önce çağrıldı:</span><span class="sxs-lookup"><span data-stu-id="ce025-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="ce025-132">`$find()` İşlevi adlandırılmış bir öğeyi sayfada arayışındadır ve sunucu tarafı kimliği bir parametre bekliyor.</span><span class="sxs-lookup"><span data-stu-id="ce025-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="ce025-133">Bu nedenle, `$find("mpe")` ModalPopup denetimi istemci gösterimini döndürür; `show()` görünen açılan yöntemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="ce025-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="ce025-134">[![Kalıcı açılan görüntülenir ya da düğme tıklandığında](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ce025-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="ce025-135">Kalıcı açılan görüntülenir ya da düğme tıklandığında ([tam boyutlu görüntüyü görmek için tıklatın](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ce025-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ce025-136">[Önceki](positioning-a-modalpopup-cs.md)
> [İleri](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ce025-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>
