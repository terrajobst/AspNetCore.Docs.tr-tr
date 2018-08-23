---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: (VB) CascadingDropDown ile otomatik geri gönderme kullanma | Microsoft Docs
author: wenz
description: Bir DropDownList yükleri değişiklikleri anoth değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 31f24374714371f102a1e6bc7e8977713876b228
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757373"
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a><span data-ttu-id="e024d-103">(VB) CascadingDropDown ile otomatik geri gönderme kullanma</span><span class="sxs-lookup"><span data-stu-id="e024d-103">Using Auto-Postback with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="e024d-104">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e024d-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e024d-105">[Kodu indir](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e024d-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span></span>

> <span data-ttu-id="e024d-106">Bir DropDownList yükleri değişiklikleri başka bir DropDownList değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir.</span><span class="sxs-lookup"><span data-stu-id="e024d-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e024d-107">Ancak, ASP CascadingDropDown denetim kullanırken. Zaman uyumsuz olarak listesine veri yükleme bir (gereksiz) geri gönderme kendisini oluşturur sonra NET'in DropDownList denetimin AutoPostBack özelliği çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="e024d-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="e024d-108">Bu etkiyi miktar JavaScript kodu ile önlenebilir.</span><span class="sxs-lookup"><span data-stu-id="e024d-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="e024d-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="e024d-109">Overview</span></span>

<span data-ttu-id="e024d-110">Bir DropDownList yükleri değişiklikleri başka bir DropDownList değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir.</span><span class="sxs-lookup"><span data-stu-id="e024d-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e024d-111">(Örneği için BİZE durumları listesini bir liste sağlar ve sonraki listesi, bu durumda bulunan büyük şehirlerin ile doldurulur.) Ancak, ASP CascadingDropDown denetim kullanırken. Zaman uyumsuz olarak listesine veri yükleme bir (gereksiz) geri gönderme kendisini oluşturur sonra NET'in DropDownList denetimin AutoPostBack özelliği çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="e024d-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="e024d-112">Bu etkiyi miktar JavaScript kodu ile önlenebilir.</span><span class="sxs-lookup"><span data-stu-id="e024d-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="e024d-113">Adımlar</span><span class="sxs-lookup"><span data-stu-id="e024d-113">Steps</span></span>

<span data-ttu-id="e024d-114">ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde &lt; `form` &gt; öğesi):</span><span class="sxs-lookup"><span data-stu-id="e024d-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="e024d-115">Ardından, bir DropDownList denetimi gereklidir:</span><span class="sxs-lookup"><span data-stu-id="e024d-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="e024d-116">Bu liste için web hizmeti URL'sini ve yöntemi bilgilerini sağlama CascadingDropDown genişletici eklenir:</span><span class="sxs-lookup"><span data-stu-id="e024d-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="e024d-117">CascadingDropDown genişletici daha sonra bir web hizmeti aşağıdaki yöntem imzasını ile zaman uyumsuz olarak çağırır:</span><span class="sxs-lookup"><span data-stu-id="e024d-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="e024d-118">Yöntem CascadingDropDown değer türünde bir dizi döndürür.</span><span class="sxs-lookup"><span data-stu-id="e024d-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="e024d-119">Liste girişin açıklamalı alt yazı ve değer türün yapıcı ilk bekliyor (HTML `value` özniteliği).</span><span class="sxs-lookup"><span data-stu-id="e024d-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="e024d-120">Tarayıcı sayfa yükleme üç satıcılarla açılan listede seçilmiş ikinci bir doldurur.</span><span class="sxs-lookup"><span data-stu-id="e024d-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="e024d-121">Ayrıca, ASP.NET tanımlar `__doPostBack()` JavaScript yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e024d-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="e024d-122">Sayfası yüklendikten sonra bu JavaScript çağrı yalnızca olup olmadığını öğeleri içinde ancak açılan listesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="e024d-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="e024d-123">Listede bir öğe varsa, JavaScript kodunu bir zaman aşımı kullanır ve yarım saniye içinde yeniden dener Denetim Araç Seti şu anda bunları yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="e024d-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

<span data-ttu-id="e024d-124">Bu şekilde, bir geri gönderme yalnızca listede, aslında öğe ve bir giriş kullanıcının seçtiği yürütülür.</span><span class="sxs-lookup"><span data-stu-id="e024d-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="e024d-125">[![Bir liste öğesinin seçerek geri göndermeye neden olur](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e024d-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="e024d-126">Bir liste öğesinin seçerek geri göndermeye neden olur ([tam boyutlu görüntüyü görmek için tıklatın](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e024d-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e024d-127">Önceki</span><span class="sxs-lookup"><span data-stu-id="e024d-127">Previous</span></span>](presetting-list-entries-with-cascadingdropdown-vb.md)
