---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: (VB) doğrulama denetimleri ile TextBoxWatermark kullanma | Microsoft Docs
author: wenz
description: Bir metin kutusu içinde gösterilir böylece AJAX Denetim Araç Seti TextBoxWatermark denetiminde metin kutusunu genişletir. Bir kullanıcı kutusuna tıkladığında, ben...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 25a18bf4ad514fbeadd321f50d3b715b38d656cd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376584"
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a><span data-ttu-id="103bd-104">(VB) doğrulama denetimleri ile TextBoxWatermark kullanma</span><span class="sxs-lookup"><span data-stu-id="103bd-104">Using TextBoxWatermark With Validation Controls (VB)</span></span>
====================
<span data-ttu-id="103bd-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="103bd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="103bd-106">[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="103bd-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span></span>

> <span data-ttu-id="103bd-107">Bir metin kutusu içinde gösterilir böylece AJAX Denetim Araç Seti TextBoxWatermark denetiminde metin kutusunu genişletir.</span><span class="sxs-lookup"><span data-stu-id="103bd-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="103bd-108">Kullanıcı kutusuna tıkladığında boşaltılır.</span><span class="sxs-lookup"><span data-stu-id="103bd-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="103bd-109">Kullanıcının metin girmeden kutusu ayrılırsa doldurulmuş metni görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="103bd-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="103bd-110">Bu aynı sayfa üzerinde ASP.NET doğrulama denetimleri ile çakışabilir, ancak bu sorunları gidermek.</span><span class="sxs-lookup"><span data-stu-id="103bd-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="103bd-111">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="103bd-111">Overview</span></span>

<span data-ttu-id="103bd-112">`TextBoxWatermark` AJAX Denetim Araç Seti denetiminde metin kutusuna bir metin kutusu içinde gösterilir böylece genişletir.</span><span class="sxs-lookup"><span data-stu-id="103bd-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="103bd-113">Kullanıcı kutusuna tıkladığında boşaltılır.</span><span class="sxs-lookup"><span data-stu-id="103bd-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="103bd-114">Kullanıcının metin girmeden kutusu ayrılırsa doldurulmuş metni görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="103bd-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="103bd-115">Bu aynı sayfa üzerinde ASP.NET doğrulama denetimleri ile çakışabilir, ancak bu sorunları gidermek.</span><span class="sxs-lookup"><span data-stu-id="103bd-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="103bd-116">Adımlar</span><span class="sxs-lookup"><span data-stu-id="103bd-116">Steps</span></span>

<span data-ttu-id="103bd-117">Temel kurulum örnek aşağıda verilmiştir: bir `TextBox` denetimi Filigran kullanarak bir `TextBoxWatermarkExtender` denetimi.</span><span class="sxs-lookup"><span data-stu-id="103bd-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="103bd-118">Bir düğme, bir geri gönderme tetikler ve daha sonra sayfasında doğrulama denetimlerini tetiklemek için kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="103bd-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="103bd-119">Ayrıca, bir `ScriptManager` denetimi, ASP.NET AJAX başlatmak için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="103bd-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="103bd-120">Şimdi ekleyin bir `RequiredFieldValidator` olup metin alanı form gönderildiğinde denetleyen denetimi.</span><span class="sxs-lookup"><span data-stu-id="103bd-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="103bd-121">`InitialValue` Doğrulayıcı özelliğini ayarlayın, kullanılan aynı değere `TextBoxWatermarkExtender` denetimi: form gönderildiğinde, değişmeyen bir metin kutusu içindeki eşik değerini değeridir:</span><span class="sxs-lookup"><span data-stu-id="103bd-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="103bd-122">Ancak bir sorun var. Bu yaklaşım: JavaScript istemci devre dışı bırakır, metin alanı filigran metni ile bu nedenle doldurulmuş değil `RequiredFieldValidator` bir hata iletisi tetiklemez.</span><span class="sxs-lookup"><span data-stu-id="103bd-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="103bd-123">Bu nedenle, ikinci `RequiredFieldValidator` denetimi gereklidir, bir boş metin kutusunu denetler (atlama `InitialValue` özniteliği).</span><span class="sxs-lookup"><span data-stu-id="103bd-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="103bd-124">Her iki doğrulayıcıları kullandığından `Display` = `"Dynamic"`, son kullanıcının hangi iki doğrulayıcıları harekete geçirildi görsel görünümünü ayırt edemez; bunun yerine, olduğu gibi yalnızca bunlardan birinin arar.</span><span class="sxs-lookup"><span data-stu-id="103bd-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="103bd-125">Son olarak, bir hata iletisi Doğrulayıcı verilen, alandaki metin çıktısını almak için bazı sunucu tarafı kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="103bd-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


<span data-ttu-id="103bd-126">[![Doğrulayıcı alanı metin olduğunu şeklinde hata verir.](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="103bd-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="103bd-127">Doğrulayıcı alanı metin olduğunu şeklinde hata verir ([tam boyutlu görüntüyü görmek için tıklatın](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="103bd-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="103bd-128">Önceki</span><span class="sxs-lookup"><span data-stu-id="103bd-128">Previous</span></span>](using-textboxwatermark-in-a-formview-vb.md)
