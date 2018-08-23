---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Doğrulama denetimleri ile (C#) TextBoxWatermark kullanma | Microsoft Docs
author: wenz
description: Bir metin kutusu içinde gösterilir böylece AJAX Denetim Araç Seti TextBoxWatermark denetiminde metin kutusunu genişletir. Bir kullanıcı kutusuna tıkladığında, ben...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 35dd340ffce7b83303a44a0d6532ce73be5350f0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754116"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="8f1bf-104">Doğrulama denetimleri ile (C#) TextBoxWatermark kullanma</span><span class="sxs-lookup"><span data-stu-id="8f1bf-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>
====================
<span data-ttu-id="8f1bf-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8f1bf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8f1bf-106">[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8f1bf-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="8f1bf-107">Bir metin kutusu içinde gösterilir böylece AJAX Denetim Araç Seti TextBoxWatermark denetiminde metin kutusunu genişletir.</span><span class="sxs-lookup"><span data-stu-id="8f1bf-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="8f1bf-108">Kullanıcı kutusuna tıkladığında boşaltılır.</span><span class="sxs-lookup"><span data-stu-id="8f1bf-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="8f1bf-109">Kullanıcının metin girmeden kutusu ayrılırsa doldurulmuş metni görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8f1bf-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="8f1bf-110">Bu aynı sayfa üzerinde ASP.NET doğrulama denetimleri ile çakışabilir, ancak bu sorunları gidermek.</span><span class="sxs-lookup"><span data-stu-id="8f1bf-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="8f1bf-111">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="8f1bf-111">Overview</span></span>

<span data-ttu-id="8f1bf-112">`TextBoxWatermark` AJAX Denetim Araç Seti denetiminde metin kutusuna bir metin kutusu içinde gösterilir böylece genişletir.</span><span class="sxs-lookup"><span data-stu-id="8f1bf-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="8f1bf-113">Kullanıcı kutusuna tıkladığında boşaltılır.</span><span class="sxs-lookup"><span data-stu-id="8f1bf-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="8f1bf-114">Kullanıcının metin girmeden kutusu ayrılırsa doldurulmuş metni görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8f1bf-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="8f1bf-115">Bu aynı sayfa üzerinde ASP.NET doğrulama denetimleri ile çakışabilir, ancak bu sorunları gidermek.</span><span class="sxs-lookup"><span data-stu-id="8f1bf-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="8f1bf-116">Adımlar</span><span class="sxs-lookup"><span data-stu-id="8f1bf-116">Steps</span></span>

<span data-ttu-id="8f1bf-117">Temel kurulum örnek aşağıda verilmiştir: bir `TextBox` denetimi Filigran kullanarak bir `TextBoxWatermarkExtender` denetimi.</span><span class="sxs-lookup"><span data-stu-id="8f1bf-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="8f1bf-118">Bir düğme, bir geri gönderme tetikler ve daha sonra sayfasında doğrulama denetimlerini tetiklemek için kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="8f1bf-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="8f1bf-119">Ayrıca, bir `ScriptManager` denetimi, ASP.NET AJAX başlatmak için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="8f1bf-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="8f1bf-120">Şimdi ekleyin bir `RequiredFieldValidator` olup metin alanı form gönderildiğinde denetleyen denetimi.</span><span class="sxs-lookup"><span data-stu-id="8f1bf-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="8f1bf-121">`InitialValue` Doğrulayıcı özelliğini ayarlayın, kullanılan aynı değere `TextBoxWatermarkExtender` denetimi: form gönderildiğinde, değişmeyen bir metin kutusu içindeki eşik değerini değeridir:</span><span class="sxs-lookup"><span data-stu-id="8f1bf-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="8f1bf-122">Ancak bir sorun var. Bu yaklaşım: JavaScript istemci devre dışı bırakır, metin alanı filigran metni ile bu nedenle doldurulmuş değil `RequiredFieldValidator` bir hata iletisi tetiklemez.</span><span class="sxs-lookup"><span data-stu-id="8f1bf-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="8f1bf-123">Bu nedenle, ikinci `RequiredFieldValidator` denetimi gereklidir, bir boş metin kutusunu denetler (atlama `InitialValue` özniteliği).</span><span class="sxs-lookup"><span data-stu-id="8f1bf-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="8f1bf-124">Her iki doğrulayıcıları kullandığından `Display` = `"Dynamic"`, son kullanıcının hangi iki doğrulayıcıları harekete geçirildi görsel görünümünü ayırt edemez; bunun yerine, olduğu gibi yalnızca bunlardan birinin arar.</span><span class="sxs-lookup"><span data-stu-id="8f1bf-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="8f1bf-125">Son olarak, bir hata iletisi Doğrulayıcı verilen, alandaki metin çıktısını almak için bazı sunucu tarafı kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8f1bf-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


<span data-ttu-id="8f1bf-126">[![Doğrulayıcı alanı metin olduğunu şeklinde hata verir.](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8f1bf-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="8f1bf-127">Doğrulayıcı alanı metin olduğunu şeklinde hata verir ([tam boyutlu görüntüyü görmek için tıklatın](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8f1bf-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8f1bf-128">[Önceki](using-textboxwatermark-in-a-formview-cs.md)
> [İleri](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8f1bf-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>
