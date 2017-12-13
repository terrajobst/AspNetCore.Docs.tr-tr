---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: "Otomatik geri gönderme CascadingDropDown ile (C#) kullanarak | Microsoft Docs"
author: wenz
description: "Böylece bir DropDownList yükleri değişiklikleri anoth değerleri ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: cd103283f46223d5158e58227bb53c00c74bc7d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a><span data-ttu-id="43bcc-103">Otomatik geri gönderme kullanarak CascadingDropDown ile (C#)</span><span class="sxs-lookup"><span data-stu-id="43bcc-103">Using Auto-Postback with CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="43bcc-104">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="43bcc-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="43bcc-105">[Kodu indirme](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="43bcc-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span></span>

> <span data-ttu-id="43bcc-106">Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir.</span><span class="sxs-lookup"><span data-stu-id="43bcc-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="43bcc-107">Ancak CascadingDropDown denetimi, ASP kullanırken. Zaman uyumsuz olarak listeye verileri yüklenirken bir (gereksiz) geri gönderme kendisini oluşturur beri NET'in DropDownList denetimin AutoPostBack özelliği çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="43bcc-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="43bcc-108">Bazı JavaScript kodu ile Bu etkiyi önlenebilir.</span><span class="sxs-lookup"><span data-stu-id="43bcc-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="43bcc-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="43bcc-109">Overview</span></span>

<span data-ttu-id="43bcc-110">Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir.</span><span class="sxs-lookup"><span data-stu-id="43bcc-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="43bcc-111">(Örneği için bir liste BİZE durumları bir listesini sağlar ve sonraki liste sonra bu durum önemli şehirlerde ile doldurulur.) Ancak CascadingDropDown denetimi, ASP kullanırken. Zaman uyumsuz olarak listeye verileri yüklenirken bir (gereksiz) geri gönderme kendisini oluşturur beri NET'in DropDownList denetimin AutoPostBack özelliği çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="43bcc-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="43bcc-112">Bazı JavaScript kodu ile Bu etkiyi önlenebilir.</span><span class="sxs-lookup"><span data-stu-id="43bcc-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="43bcc-113">Adımlar</span><span class="sxs-lookup"><span data-stu-id="43bcc-113">Steps</span></span>

<span data-ttu-id="43bcc-114">ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde &lt; `form` &gt; öğesi):</span><span class="sxs-lookup"><span data-stu-id="43bcc-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="43bcc-115">Ardından, bir DropDownList denetimi gereklidir:</span><span class="sxs-lookup"><span data-stu-id="43bcc-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="43bcc-116">Bu liste, web hizmeti URL'si ve yöntem bilgileri sağlayan bir CascadingDropDown genişletici eklenir:</span><span class="sxs-lookup"><span data-stu-id="43bcc-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="43bcc-117">CascadingDropDown genişletici daha sonra bir web hizmetini aşağıdaki yöntemi imzası ile zaman uyumsuz olarak çağırır:</span><span class="sxs-lookup"><span data-stu-id="43bcc-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="43bcc-118">Yöntem CascadingDropDown değer türünde bir dizi döndürür.</span><span class="sxs-lookup"><span data-stu-id="43bcc-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="43bcc-119">Tür kurucu ilk liste girişin başlık ve değer Bekliyor (HTML `value` özniteliği).</span><span class="sxs-lookup"><span data-stu-id="43bcc-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="43bcc-120">Yükleme sayfasını tarayıcıda açılır listenin üç satıcılarla seçilmiş ikinci bir doldurur.</span><span class="sxs-lookup"><span data-stu-id="43bcc-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="43bcc-121">Ayrıca, ASP.NET tanımlar `__doPostBack()` JavaScript yöntemi.</span><span class="sxs-lookup"><span data-stu-id="43bcc-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="43bcc-122">Sayfa yüklendikten sonra bu JavaScript çağrısı yalnızca yoksa öğe içinde ancak açılır listeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="43bcc-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="43bcc-123">Listeden bir öğe varsa, böylece JavaScript kodu zaman aşımı kullanır ve bir yarım saniye içinde yeniden dener Denetim Araç Seti şu anda bunları yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="43bcc-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

<span data-ttu-id="43bcc-124">Bu şekilde geri gönderimin yalnızca listede gerçekte öğeleri yok ve kullanıcı bir giriş seçerse sonra yürütülür.</span><span class="sxs-lookup"><span data-stu-id="43bcc-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="43bcc-125">[![Bir liste öğesinin seçerek geri gönderimin neden olur.](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="43bcc-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="43bcc-126">Bir liste öğesinin belirlenmesi geri gönderimin neden olur ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="43bcc-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="43bcc-127">[Önceki](presetting-list-entries-with-cascadingdropdown-cs.md)
[sonraki](filling-a-list-using-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="43bcc-127">[Previous](presetting-list-entries-with-cascadingdropdown-cs.md)
[Next](filling-a-list-using-cascadingdropdown-vb.md)</span></span>
