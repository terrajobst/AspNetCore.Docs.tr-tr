---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Geri göndermeler ReorderList (VB) ile kullanma | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti, ReorderList denetimi sürükle ve bırak aracılığıyla kullanıcı tarafından sıralanabilir bir liste sağlar. Listenin kaldırılmasında her bir SAS...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: ef43471f7d8cc94c1a82a368e27acef05f474f81
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="using-postbacks-with-reorderlist-vb"></a><span data-ttu-id="1ad22-104">Geri göndermeler ReorderList (VB) ile kullanma</span><span class="sxs-lookup"><span data-stu-id="1ad22-104">Using Postbacks with ReorderList (VB)</span></span>
====================
<span data-ttu-id="1ad22-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1ad22-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1ad22-106">[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="1ad22-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span></span>

> <span data-ttu-id="1ad22-107">AJAX Denetim Araç Seti, ReorderList denetimi sürükle ve bırak aracılığıyla kullanıcı tarafından sıralanabilir bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="1ad22-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="1ad22-108">Listenin kaldırılmasında her geri gönderimin sunucunun değişikliği bildirin.</span><span class="sxs-lookup"><span data-stu-id="1ad22-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="1ad22-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="1ad22-109">Overview</span></span>

<span data-ttu-id="1ad22-110">`ReorderList` AJAX Denetim Araç Seti denetiminde sürükle ve bırak aracılığıyla kullanıcı tarafından sıralanabilir bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="1ad22-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="1ad22-111">Listenin kaldırılmasında her geri gönderimin sunucunun değişikliği bildirin.</span><span class="sxs-lookup"><span data-stu-id="1ad22-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="1ad22-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="1ad22-112">Steps</span></span>

<span data-ttu-id="1ad22-113">Birkaç olası veri kaynakları için vardır `ReorderList` denetim.</span><span class="sxs-lookup"><span data-stu-id="1ad22-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="1ad22-114">Biridir kullanmak için bir `XmlDataSource` denetimi:</span><span class="sxs-lookup"><span data-stu-id="1ad22-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="1ad22-115">Bu XML bağlamak amacıyla bir `ReorderList` denetimi ve etkinleştir Geri göndermeler, aşağıdaki öznitelikler ayarlanmalıdır:</span><span class="sxs-lookup"><span data-stu-id="1ad22-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="1ad22-116">`DataSourceID`: Veri kaynağı kimliği</span><span class="sxs-lookup"><span data-stu-id="1ad22-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="1ad22-117">`SortOrderField`: Sıralama ölçütü özelliği</span><span class="sxs-lookup"><span data-stu-id="1ad22-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="1ad22-118">`AllowReorder`: Liste öğelerini yeniden sıralamak kullanıcı izin verilip verilmeyeceğini</span><span class="sxs-lookup"><span data-stu-id="1ad22-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="1ad22-119">`PostBackOnReorder`: Liste düzenlenmeyecek her geri gönderimin oluşturulup oluşturulmayacağını</span><span class="sxs-lookup"><span data-stu-id="1ad22-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="1ad22-120">Denetim için uygun biçimlendirme aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="1ad22-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="1ad22-121">İçinde `ReorderList` denetimi, veri kaynağındaki belirli veri bağlı kullanarak `Eval()` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1ad22-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

<span data-ttu-id="1ad22-122">Son yeniden sıralama oluştuğunda sayfasında rasgele konumunda bir etiket bilgileri tutun:</span><span class="sxs-lookup"><span data-stu-id="1ad22-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="1ad22-123">Bu etiket metin geri gönderme işleme sunucu tarafı kodu olarak girilir:</span><span class="sxs-lookup"><span data-stu-id="1ad22-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

<span data-ttu-id="1ad22-124">Son olarak, ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` sayfada denetim koyun:</span><span class="sxs-lookup"><span data-stu-id="1ad22-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


<span data-ttu-id="1ad22-125">[![Her yeniden sıralama geri gönderimin tetikler](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1ad22-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="1ad22-126">Her yeniden sıralama geri gönderimin tetikler ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-postbacks-with-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1ad22-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1ad22-127">[Önceki](drag-and-drop-via-reorderlist-cs.md)
> [sonraki](drag-and-drop-via-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1ad22-127">[Previous](drag-and-drop-via-reorderlist-cs.md)
[Next](drag-and-drop-via-reorderlist-vb.md)</span></span>
