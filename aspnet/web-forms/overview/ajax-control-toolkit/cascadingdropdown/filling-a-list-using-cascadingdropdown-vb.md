---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: CascadingDropDown (VB) kullanarak bir liste doldurma | Microsoft Docs
author: wenz
description: "Böylece bir DropDownList yükleri değişiklikleri anoth değerleri ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 1c5cb23be4366365c73ce4774e6a53e452e2a6c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="e640e-103">CascadingDropDown (VB) kullanarak bir liste doldurma</span><span class="sxs-lookup"><span data-stu-id="e640e-103">Filling a List Using CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="e640e-104">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e640e-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e640e-105">[Kodu indirme](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e640e-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="e640e-106">Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir.</span><span class="sxs-lookup"><span data-stu-id="e640e-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e640e-107">(Örneği için bir liste BİZE durumları bir listesini sağlar ve sonraki liste sonra bu durum önemli şehirlerde ile doldurulur.) Çözmek için ilk testten bu denetimi kullanarak bir açılır liste gerçekte doldurmaktır.</span><span class="sxs-lookup"><span data-stu-id="e640e-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="e640e-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="e640e-108">Overview</span></span>

<span data-ttu-id="e640e-109">Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir.</span><span class="sxs-lookup"><span data-stu-id="e640e-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e640e-110">(Örneği için bir liste BİZE durumları bir listesini sağlar ve sonraki liste sonra bu durum önemli şehirlerde ile doldurulur.) Çözmek için ilk testten bu denetimi kullanarak bir açılır liste gerçekte doldurmaktır.</span><span class="sxs-lookup"><span data-stu-id="e640e-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="e640e-111">Adımlar</span><span class="sxs-lookup"><span data-stu-id="e640e-111">Steps</span></span>

<span data-ttu-id="e640e-112">ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):</span><span class="sxs-lookup"><span data-stu-id="e640e-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="e640e-113">Ardından, bir DropDownList denetimi gereklidir:</span><span class="sxs-lookup"><span data-stu-id="e640e-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="e640e-114">Bu liste için bir CascadingDropDown genişletici eklenir.</span><span class="sxs-lookup"><span data-stu-id="e640e-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="e640e-115">Ardından listede görüntülenecek girişleri bir listesini dönecek bir web hizmetini zaman uyumsuz bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="e640e-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="e640e-116">Bunun çalışması için aşağıdaki CascadingDropDown öznitelikleri ayarlanması gerekir:</span><span class="sxs-lookup"><span data-stu-id="e640e-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="e640e-117">`ServicePath`: Liste girişlerini sunan bir web hizmeti URL'si</span><span class="sxs-lookup"><span data-stu-id="e640e-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="e640e-118">`ServiceMethod`: Liste girişlerini gönderiliyor web yöntemi</span><span class="sxs-lookup"><span data-stu-id="e640e-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="e640e-119">`TargetControlID`: Aşağı açılan listesinin kimliği</span><span class="sxs-lookup"><span data-stu-id="e640e-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="e640e-120">`Category`: Web yöntemi çağrıldığında gönderildi kategori bilgileri</span><span class="sxs-lookup"><span data-stu-id="e640e-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="e640e-121">`PromptText`: Zaman uyumsuz olarak listesi verileri sunucudan yüklenirken görüntülenen metin</span><span class="sxs-lookup"><span data-stu-id="e640e-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="e640e-122">İşaretleme için işte `CascadingDropDown` öğesi.</span><span class="sxs-lookup"><span data-stu-id="e640e-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="e640e-123">C# ve VB arasındaki tek fark ilişkili web hizmeti adıdır:</span><span class="sxs-lookup"><span data-stu-id="e640e-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="e640e-124">' Ten gelen JavaScript kodu `CascadingDropDown` genişletici aşağıdaki imzalı web hizmeti yöntemi çağırır:</span><span class="sxs-lookup"><span data-stu-id="e640e-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="e640e-125">Önemli en boy yöntemi türünde bir dizi döndürmesi gerekir, böylece `CascadingDropDownNameValue` (ASP.NET AJAX Denetim Araç Seti tarafından tanımlanmış).</span><span class="sxs-lookup"><span data-stu-id="e640e-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="e640e-126">İçinde `CascadingDropDownNameValue` contructor, liste girişin metin ve değerini sağlanmalıdır, ilk gibi `<option value="VALUE">NAME</option>` HTML'de yapın.</span><span class="sxs-lookup"><span data-stu-id="e640e-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="e640e-127">Bazı örnek veriler aşağıdadır:</span><span class="sxs-lookup"><span data-stu-id="e640e-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="e640e-128">Tarayıcıda sayfa yüklenirken üç satıcıları ile doldurulacak liste tetikler.</span><span class="sxs-lookup"><span data-stu-id="e640e-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="e640e-129">[![Liste otomatik olarak doldurulur.](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e640e-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="e640e-130">Liste otomatik olarak doldurulur ([tam boyutlu görüntüyü görüntülemek için tıklatın](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e640e-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e640e-131">[Önceki](using-auto-postback-with-cascadingdropdown-cs.md)
[sonraki](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e640e-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>
