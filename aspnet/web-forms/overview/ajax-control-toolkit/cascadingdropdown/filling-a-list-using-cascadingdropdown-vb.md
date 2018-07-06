---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: (VB) CascadingDropDown kullanarak liste doldurma | Microsoft Docs
author: wenz
description: Bir DropDownList yükleri değişiklikleri anoth değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 4a2e2440c0b93fc3945c466a2c60c6560c9e57a4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834183"
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="9cd4b-103">(VB) CascadingDropDown kullanarak liste doldurma</span><span class="sxs-lookup"><span data-stu-id="9cd4b-103">Filling a List Using CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="9cd4b-104">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9cd4b-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9cd4b-105">[Kodu indir](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9cd4b-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="9cd4b-106">Bir DropDownList yükleri değişiklikleri başka bir DropDownList değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir.</span><span class="sxs-lookup"><span data-stu-id="9cd4b-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="9cd4b-107">(Örneği için BİZE durumları listesini bir liste sağlar ve sonraki listesi, bu durumda bulunan büyük şehirlerin ile doldurulur.) Çözmek için ilk testten bu denetimi kullanarak bir açılan listedeki gerçekten doldurmaktır.</span><span class="sxs-lookup"><span data-stu-id="9cd4b-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="9cd4b-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="9cd4b-108">Overview</span></span>

<span data-ttu-id="9cd4b-109">Bir DropDownList yükleri değişiklikleri başka bir DropDownList değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir.</span><span class="sxs-lookup"><span data-stu-id="9cd4b-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="9cd4b-110">(Örneği için BİZE durumları listesini bir liste sağlar ve sonraki listesi, bu durumda bulunan büyük şehirlerin ile doldurulur.) Çözmek için ilk testten bu denetimi kullanarak bir açılan listedeki gerçekten doldurmaktır.</span><span class="sxs-lookup"><span data-stu-id="9cd4b-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="9cd4b-111">Adımlar</span><span class="sxs-lookup"><span data-stu-id="9cd4b-111">Steps</span></span>

<span data-ttu-id="9cd4b-112">ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="9cd4b-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="9cd4b-113">Ardından, bir DropDownList denetimi gereklidir:</span><span class="sxs-lookup"><span data-stu-id="9cd4b-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="9cd4b-114">Bu liste için CascadingDropDown genişletici eklenir.</span><span class="sxs-lookup"><span data-stu-id="9cd4b-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="9cd4b-115">Ardından listede görüntülenecek girişlerinin listesini döndürür bir web hizmetini zaman uyumsuz bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="9cd4b-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="9cd4b-116">Bunun işe yaraması için aşağıdaki CascadingDropDown öznitelikleri ayarlanması gerekir:</span><span class="sxs-lookup"><span data-stu-id="9cd4b-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="9cd4b-117">`ServicePath`: Liste girişlerini sunan bir web hizmeti URL'si</span><span class="sxs-lookup"><span data-stu-id="9cd4b-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="9cd4b-118">`ServiceMethod`: Liste girişlerini sunma web metodu</span><span class="sxs-lookup"><span data-stu-id="9cd4b-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="9cd4b-119">`TargetControlID`: Açılan listenin kimliği</span><span class="sxs-lookup"><span data-stu-id="9cd4b-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="9cd4b-120">`Category`: Web yöntemi çağrıldığında gönderilen kategori bilgileri</span><span class="sxs-lookup"><span data-stu-id="9cd4b-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="9cd4b-121">`PromptText`: Zaman uyumsuz olarak listesi verileri sunucudan yüklenirken görüntülenen metin</span><span class="sxs-lookup"><span data-stu-id="9cd4b-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="9cd4b-122">İçin biçimlendirme şöyledir `CascadingDropDown` öğesi.</span><span class="sxs-lookup"><span data-stu-id="9cd4b-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="9cd4b-123">C# ve VB arasındaki tek fark ilişkili web hizmeti adıdır:</span><span class="sxs-lookup"><span data-stu-id="9cd4b-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="9cd4b-124">Gelen JavaScript kodu `CascadingDropDown` genişletici imzayla bir web hizmeti yöntemi çağırır:</span><span class="sxs-lookup"><span data-stu-id="9cd4b-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="9cd4b-125">Önemli bir yönüdür yöntemi türünde bir dizi döndürmek gerekir, bu nedenle `CascadingDropDownNameValue` (ASP.NET AJAX Denetim Araç Seti tarafından tanımlanır).</span><span class="sxs-lookup"><span data-stu-id="9cd4b-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="9cd4b-126">İçinde `CascadingDropDownNameValue` Oluşturucu, ilk liste girdinin metin ve değerini sağlanmalıdır, gibi `<option value="VALUE">NAME</option>` HTML yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9cd4b-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="9cd4b-127">Bazı örnek veriler şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="9cd4b-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="9cd4b-128">Tarayıcı sayfa yükleme üç satıcıları ile doldurulacak liste tetikler.</span><span class="sxs-lookup"><span data-stu-id="9cd4b-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="9cd4b-129">[![Liste otomatik olarak doldurulur.](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9cd4b-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="9cd4b-130">Liste otomatik olarak doldurulur ([tam boyutlu görüntüyü görmek için tıklatın](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9cd4b-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9cd4b-131">[Önceki](using-auto-postback-with-cascadingdropdown-cs.md)
> [İleri](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9cd4b-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>
