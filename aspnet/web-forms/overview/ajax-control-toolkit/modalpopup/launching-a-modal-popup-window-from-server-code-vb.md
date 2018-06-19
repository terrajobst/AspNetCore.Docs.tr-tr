---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Sunucu kodu (VB) kalıcı açılır penceresinden başlatma | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar. Ancak bazı senaryolar bu t iste...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 46554dae60ad9cd13e97e5755e95cb2125d1fed9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872877"
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="e98e1-104">Sunucu kodu (VB) kalıcı açılır penceresinden başlatma</span><span class="sxs-lookup"><span data-stu-id="e98e1-104">Launching a Modal Popup Window from Server Code (VB)</span></span>
====================
<span data-ttu-id="e98e1-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e98e1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e98e1-106">[Kodu indirme](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e98e1-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="e98e1-107">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="e98e1-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="e98e1-108">Ancak bazı senaryolar kalıcı açılan açılmasını sunucu tarafında tetiklenir gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e98e1-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="e98e1-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="e98e1-109">Overview</span></span>

<span data-ttu-id="e98e1-110">AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı popup oluşturmak için basit bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="e98e1-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="e98e1-111">Ancak bazı senaryolar kalıcı açılan açılmasını sunucu tarafında tetiklenir gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e98e1-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="e98e1-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="e98e1-112">Steps</span></span>

<span data-ttu-id="e98e1-113">Öncelikle, bir ASP.NET düğme web denetimi ModalPopup denetiminin nasıl çalıştığını göstermek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="e98e1-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="e98e1-114">İçinde bu tür bir düğme ekleme &lt;form&gt; öğe yeni bir sayfa üzerinde:</span><span class="sxs-lookup"><span data-stu-id="e98e1-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="e98e1-115">Ardından, oluşturmak istediğiniz açılan için işaretleme gerekir.</span><span class="sxs-lookup"><span data-stu-id="e98e1-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="e98e1-116">Olarak tanımlayan bir `<asp:Panel>` denetlemek ve düğme denetimi içerdiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="e98e1-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="e98e1-117">ModalPopup denetim böyle bir düğmeyi Kapat açılan hale getirmek için işlevselliği sunar; Aksi takdirde, kaybolur izin vermek için kolay bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="e98e1-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="e98e1-118">Sonraki ModalPopup denetimi ASP.NET AJAX araç setinin sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e98e1-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="e98e1-119">Denetim yükleyen düğmesi, kayboluyor kılan düğmesi ve gerçek açılan Kimliğini özelliklerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="e98e1-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="e98e1-120">Gibi ASP.NET AJAX tabanlı tüm web sayfalarıyla; komut dosyası Yöneticisi'ni, farklı bir hedef tarayıcılar için gerekli JavaScript kitaplıklarını yüklemek için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="e98e1-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="e98e1-121">Örneğin tarayıcıda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e98e1-121">Run the example in the browser.</span></span> <span data-ttu-id="e98e1-122">Düğmeyi tıkladığınızda, kalıcı açılan görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e98e1-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="e98e1-123">Sunucu tarafı kodu kullanarak aynı sonucu elde etmek için yeni bir düğme gereklidir:</span><span class="sxs-lookup"><span data-stu-id="e98e1-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="e98e1-124">Gördüğünüz gibi bir düğmeye tıklayın geri gönderimin oluşturur ve çalıştırır `ServerButton_Click()` sunucuda yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e98e1-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="e98e1-125">Bu yöntemde, JavaScript işlevini çağırdı `launchModal()` yürütülür Sayfa yüklendikten sonra tam olması için JavaScript işlevinin yürütülecek:</span><span class="sxs-lookup"><span data-stu-id="e98e1-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="e98e1-126">İş `launchModal()` ModalPopup görüntülemektir.</span><span class="sxs-lookup"><span data-stu-id="e98e1-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="e98e1-127">`launchModal()` İşlevi tam HTML sayfası yüklendikten sonra yürütülür.</span><span class="sxs-lookup"><span data-stu-id="e98e1-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="e98e1-128">O anda ancak, ASP.NET AJAX framework henüz tam olarak yüklendi değil.</span><span class="sxs-lookup"><span data-stu-id="e98e1-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="e98e1-129">Bu nedenle, `launchModal()` işlevi yalnızca ModalPopup denetim daha sonra gösterilecek bir değişken ayarlar:</span><span class="sxs-lookup"><span data-stu-id="e98e1-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="e98e1-130">`pageLoad()` JavaScript işlevi, ASP.NET AJAX tam yüklenmeden silindikten sonra yürütülen özel bir işlev değil.</span><span class="sxs-lookup"><span data-stu-id="e98e1-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="e98e1-131">Kod ModalPopup denetimi, ancak yalnızca göstermek için bu işleve bu nedenle eklediğimiz `launchModal()` önce çağrılır:</span><span class="sxs-lookup"><span data-stu-id="e98e1-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="e98e1-132">`$find()` İşlevi bir adlandırılmış öğeyi sayfada aranıyor ve bir parametre olarak sunucu tarafı kodu bekliyor.</span><span class="sxs-lookup"><span data-stu-id="e98e1-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="e98e1-133">Bu nedenle, `$find("mpe")` ModalPopup denetim istemci gösterimini döndürür; kendi `show()` yöntemi görünür açılan olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="e98e1-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="e98e1-134">[![Kalıcı açılan ne zaman görünür düğmelerin ya da tıklandığında](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e98e1-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="e98e1-135">Kalıcı açılan ne zaman görünür düğmelerin ya da tıklandığında ([tam boyutlu görüntüyü görüntülemek için tıklatın](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e98e1-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e98e1-136">[Önceki](positioning-a-modalpopup-cs.md)
> [sonraki](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e98e1-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>
