---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: "Geri göndermeler bir UpdatePanel (VB) ile açılan kontrolü işleme | Microsoft Docs"
author: wenz
description: "AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar. Özellikle dikkat edilmelidir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 4445437f25925429d240b7fe2d019855afc52fe0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a><span data-ttu-id="391e9-104">Geri göndermeler bir UpdatePanel (VB) ile açılan kontrolü işleme</span><span class="sxs-lookup"><span data-stu-id="391e9-104">Handling Postbacks from A Popup Control With an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="391e9-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="391e9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="391e9-106">[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="391e9-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span></span>

> <span data-ttu-id="391e9-107">AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="391e9-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="391e9-108">Bu tür bir açılan içinde geri gönderimin oluştuğu olduğunda gerçekleştirilecek özellikle dikkat edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="391e9-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="391e9-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="391e9-109">Overview</span></span>

<span data-ttu-id="391e9-110">AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="391e9-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="391e9-111">Bu tür bir açılan içinde geri gönderimin oluştuğu olduğunda gerçekleştirilecek özellikle dikkat edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="391e9-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="391e9-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="391e9-112">Steps</span></span>

<span data-ttu-id="391e9-113">Kullanırken bir `PopupControl` bir geri gönderme ile bir `UpdatePanel` tarafından geri gönderme neden sayfa yenileme engelleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="391e9-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="391e9-114">Aşağıdaki biçimlendirmede birkaç önemli öğe tanımlar:</span><span class="sxs-lookup"><span data-stu-id="391e9-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="391e9-115">A `ScriptManager` ASP.NET AJAX Denetim Araç Seti çalışmasını denetleme</span><span class="sxs-lookup"><span data-stu-id="391e9-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="391e9-116">İki `TextBox` her ikisi de popup tetikleyecek denetimleri</span><span class="sxs-lookup"><span data-stu-id="391e9-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="391e9-117">A `Panel` açılan hizmet verecektir denetimi</span><span class="sxs-lookup"><span data-stu-id="391e9-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="391e9-118">Paneli içindeki bir `Calendar` içinde denetim katıştırılmış bir `UpdatePanel` denetimi</span><span class="sxs-lookup"><span data-stu-id="391e9-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="391e9-119">İki `PopupControlExtender` paneli metin kutuları atamak denetimleri</span><span class="sxs-lookup"><span data-stu-id="391e9-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="391e9-120">Unutmayın `OnSelectionChanged` özniteliği `Calendar` denetim ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="391e9-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="391e9-121">Kullanıcının Takvim içindeki bir tarih seçtiğinde geri gönderimin oluşmaz ve sunucu tarafı yöntemi `c1_SelectionChanged()` yürütülür.</span><span class="sxs-lookup"><span data-stu-id="391e9-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="391e9-122">Bu yöntem içinde geçerli tarih alınan ve metin kutusuna geri yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="391e9-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="391e9-123">Söz dizimi aşağıdaki gibidir: öncelikle, bir proxy nesnesi `PopupControlExtender` sayfada oluşturulmuş olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="391e9-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="391e9-124">ASP.NET AJAX Denetim Araç Seti sunar `GetProxyForCurrentPopup()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="391e9-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="391e9-125">Bu yöntem nesnesi destekler `Commit()` yöntemi bir değer (yöntem çağrısının tetikleyen denetimin değil!) açılan tetiklenen denetim geri gönderir.</span><span class="sxs-lookup"><span data-stu-id="391e9-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="391e9-126">Seçilen tarih için bağımsız değişken olarak aşağıdaki kod sağlar `Commit()` yöntemi, seçilen tarihten geri metin kutusuna yazmak için kodu neden:</span><span class="sxs-lookup"><span data-stu-id="391e9-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="391e9-127">Seçilen tarih ilişkili metin kutusunda görünür bir takvim tarihi tıklattığınızda şimdi bir tarih seçici denetimi oluşturma şu anda birçok Web sitelerinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="391e9-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="391e9-128">[![Takvim kullanıcı metin kutusuna tıkladığında görüntülenir](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="391e9-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="391e9-129">Takvim kullanıcı metin kutusuna tıklattığında görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="391e9-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="391e9-130">[![Bir tarihte tıklatarak metin kutusuna yerleştirir](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="391e9-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="391e9-131">Bir tarihte tıklatarak koyar, metin kutusuna ([tam boyutlu görüntüyü görüntülemek için tıklatın](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="391e9-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="391e9-132">[Önceki](using-multiple-popup-controls-vb.md)
[sonraki](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="391e9-132">[Previous](using-multiple-popup-controls-vb.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span></span>
