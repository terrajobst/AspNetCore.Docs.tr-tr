---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: CascadingDropDown ile (C#) otomatik geri gönderme kullanma | Microsoft Docs
author: wenz
description: Bir DropDownList yükleri değişiklikleri anoth değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: bece4f78b46c44db988e04e0e987450d94c37aab
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819386"
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a><span data-ttu-id="71187-103">CascadingDropDown ile (C#) otomatik geri gönderme kullanma</span><span class="sxs-lookup"><span data-stu-id="71187-103">Using Auto-Postback with CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="71187-104">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="71187-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="71187-105">[Kodu indir](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="71187-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span></span>

> <span data-ttu-id="71187-106">Bir DropDownList yükleri değişiklikleri başka bir DropDownList değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir.</span><span class="sxs-lookup"><span data-stu-id="71187-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="71187-107">Ancak, ASP CascadingDropDown denetim kullanırken. Zaman uyumsuz olarak listesine veri yükleme bir (gereksiz) geri gönderme kendisini oluşturur sonra NET'in DropDownList denetimin AutoPostBack özelliği çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="71187-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="71187-108">Bu etkiyi miktar JavaScript kodu ile önlenebilir.</span><span class="sxs-lookup"><span data-stu-id="71187-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="71187-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="71187-109">Overview</span></span>

<span data-ttu-id="71187-110">Bir DropDownList yükleri değişiklikleri başka bir DropDownList değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir.</span><span class="sxs-lookup"><span data-stu-id="71187-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="71187-111">(Örneği için BİZE durumları listesini bir liste sağlar ve sonraki listesi, bu durumda bulunan büyük şehirlerin ile doldurulur.) Ancak, ASP CascadingDropDown denetim kullanırken. Zaman uyumsuz olarak listesine veri yükleme bir (gereksiz) geri gönderme kendisini oluşturur sonra NET'in DropDownList denetimin AutoPostBack özelliği çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="71187-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="71187-112">Bu etkiyi miktar JavaScript kodu ile önlenebilir.</span><span class="sxs-lookup"><span data-stu-id="71187-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="71187-113">Adımlar</span><span class="sxs-lookup"><span data-stu-id="71187-113">Steps</span></span>

<span data-ttu-id="71187-114">ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde &lt; `form` &gt; öğesi):</span><span class="sxs-lookup"><span data-stu-id="71187-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="71187-115">Ardından, bir DropDownList denetimi gereklidir:</span><span class="sxs-lookup"><span data-stu-id="71187-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="71187-116">Bu liste için web hizmeti URL'sini ve yöntemi bilgilerini sağlama CascadingDropDown genişletici eklenir:</span><span class="sxs-lookup"><span data-stu-id="71187-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="71187-117">CascadingDropDown genişletici daha sonra bir web hizmeti aşağıdaki yöntem imzasını ile zaman uyumsuz olarak çağırır:</span><span class="sxs-lookup"><span data-stu-id="71187-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="71187-118">Yöntem CascadingDropDown değer türünde bir dizi döndürür.</span><span class="sxs-lookup"><span data-stu-id="71187-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="71187-119">Liste girişin açıklamalı alt yazı ve değer türün yapıcı ilk bekliyor (HTML `value` özniteliği).</span><span class="sxs-lookup"><span data-stu-id="71187-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="71187-120">Tarayıcı sayfa yükleme üç satıcılarla açılan listede seçilmiş ikinci bir doldurur.</span><span class="sxs-lookup"><span data-stu-id="71187-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="71187-121">Ayrıca, ASP.NET tanımlar `__doPostBack()` JavaScript yöntemi.</span><span class="sxs-lookup"><span data-stu-id="71187-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="71187-122">Sayfası yüklendikten sonra bu JavaScript çağrı yalnızca olup olmadığını öğeleri içinde ancak açılan listesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="71187-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="71187-123">Listede bir öğe varsa, JavaScript kodunu bir zaman aşımı kullanır ve yarım saniye içinde yeniden dener Denetim Araç Seti şu anda bunları yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="71187-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

<span data-ttu-id="71187-124">Bu şekilde, bir geri gönderme yalnızca listede, aslında öğe ve bir giriş kullanıcının seçtiği yürütülür.</span><span class="sxs-lookup"><span data-stu-id="71187-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="71187-125">[![Bir liste öğesinin seçerek geri göndermeye neden olur](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="71187-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="71187-126">Bir liste öğesinin seçerek geri göndermeye neden olur ([tam boyutlu görüntüyü görmek için tıklatın](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="71187-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="71187-127">[Önceki](presetting-list-entries-with-cascadingdropdown-cs.md)
> [İleri](filling-a-list-using-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="71187-127">[Previous](presetting-list-entries-with-cascadingdropdown-cs.md)
[Next](filling-a-list-using-cascadingdropdown-vb.md)</span></span>
