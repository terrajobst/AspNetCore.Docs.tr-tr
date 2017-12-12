---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: "Liste girişlerini CascadingDropDown (VB) ile önceden ayarlama | Microsoft Docs"
author: wenz
description: "Böylece bir DropDownList yükleri değişiklikleri anoth değerleri ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: c28c7893c39d9ba9f828c34da7ffdce525ee248e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a><span data-ttu-id="60133-103">Liste girişlerini CascadingDropDown (VB) ile önceden belirleme</span><span class="sxs-lookup"><span data-stu-id="60133-103">Presetting List Entries with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="60133-104">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="60133-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="60133-105">[Kodu indirme](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="60133-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span></span>

> <span data-ttu-id="60133-106">Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir.</span><span class="sxs-lookup"><span data-stu-id="60133-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="60133-107">Biraz kod ile verileri dinamik olarak yüklendikten sonra bir liste öğesinin seçilmiş mümkündür.</span><span class="sxs-lookup"><span data-stu-id="60133-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="60133-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="60133-108">Overview</span></span>

<span data-ttu-id="60133-109">Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir.</span><span class="sxs-lookup"><span data-stu-id="60133-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="60133-110">(Örneği için bir liste BİZE durumları bir listesini sağlar ve sonraki liste sonra bu durum önemli şehirlerde ile doldurulur.) Biraz kod ile verileri dinamik olarak yüklendikten sonra bir liste öğesinin seçilmiş mümkündür.</span><span class="sxs-lookup"><span data-stu-id="60133-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="60133-111">Adımlar</span><span class="sxs-lookup"><span data-stu-id="60133-111">Steps</span></span>

<span data-ttu-id="60133-112">ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="60133-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="60133-113">Ardından, bir DropDownList denetimi gereklidir:</span><span class="sxs-lookup"><span data-stu-id="60133-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="60133-114">Bu liste, web hizmeti URL'si ve yöntem bilgileri sağlayan bir CascadingDropDown genişletici eklenir:</span><span class="sxs-lookup"><span data-stu-id="60133-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="60133-115">CascadingDropDown genişletici daha sonra bir web hizmetini aşağıdaki yöntemi imzası ile zaman uyumsuz olarak çağırır:</span><span class="sxs-lookup"><span data-stu-id="60133-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="60133-116">Yöntem CascadingDropDown değer türünde bir dizi döndürür.</span><span class="sxs-lookup"><span data-stu-id="60133-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="60133-117">Tür kurucu ilk liste girişin başlık ve değer Bekliyor (HTML `value` özniteliği).</span><span class="sxs-lookup"><span data-stu-id="60133-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="60133-118">Üçüncü bağımsız değişkeni ayarlanmışsa, true olarak liste öğesi tarayıcıda otomatik olarak seçilir.</span><span class="sxs-lookup"><span data-stu-id="60133-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="60133-119">Yükleme sayfasını tarayıcıda açılır listenin üç satıcılarla seçilmiş ikinci bir doldurur.</span><span class="sxs-lookup"><span data-stu-id="60133-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="60133-120">[![Liste doldurulur ve otomatik olarak seçilmiş](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="60133-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="60133-121">Liste doldurulur ve otomatik olarak seçilmiş ([tam boyutlu görüntüyü görüntülemek için tıklatın](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="60133-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="60133-122">[Önceki](using-cascadingdropdown-with-a-database-vb.md)
[sonraki](using-auto-postback-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="60133-122">[Previous](using-cascadingdropdown-with-a-database-vb.md)
[Next](using-auto-postback-with-cascadingdropdown-vb.md)</span></span>
