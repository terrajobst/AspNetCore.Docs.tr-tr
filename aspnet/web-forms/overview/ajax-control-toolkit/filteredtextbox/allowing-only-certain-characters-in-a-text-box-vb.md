---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: "Bir metin kutusunda (VB) yalnızca belirli karakterlere izin verme | Microsoft Docs"
author: wenz
description: "ASP.NET doğrulama denetimleri, yalnızca belirli karakterler uygulamasında kullanıcı girdisi izin verildiğini emin olabilirsiniz. Ancak bu yazarak kullanıcıların geçersiz hala engellemez..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: b41ec1dfda5d85c625026e1f1e1ecd7e190ee3ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a><span data-ttu-id="13c86-104">Bir metin kutusunda (VB) yalnızca belirli karakterlere izin verme</span><span class="sxs-lookup"><span data-stu-id="13c86-104">Allowing Only Certain Characters in a Text Box (VB)</span></span>
====================
<span data-ttu-id="13c86-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="13c86-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="13c86-106">[Kodu indirme](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="13c86-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span></span>

> <span data-ttu-id="13c86-107">ASP.NET doğrulama denetimleri, yalnızca belirli karakterler uygulamasında kullanıcı girdisi izin verildiğini emin olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="13c86-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="13c86-108">Ancak bu kullanıcıların geçersiz karakterler yazarak ve form göndermeye devam engellemez.</span><span class="sxs-lookup"><span data-stu-id="13c86-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="13c86-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="13c86-109">Overview</span></span>

<span data-ttu-id="13c86-110">ASP.NET doğrulama denetimleri, yalnızca belirli karakterler uygulamasında kullanıcı girdisi izin verildiğini emin olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="13c86-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="13c86-111">Ancak bu kullanıcıların geçersiz karakterler yazarak ve form göndermeye devam engellemez.</span><span class="sxs-lookup"><span data-stu-id="13c86-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="13c86-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="13c86-112">Steps</span></span>

<span data-ttu-id="13c86-113">ASP.NET AJAX Denetim Araç Seti içeren `FilteredTextBox` bir metin kutusu genişleten denetim.</span><span class="sxs-lookup"><span data-stu-id="13c86-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="13c86-114">Etkinleştirilen sonra yalnızca belirli sayıda karakter alanına girilen.</span><span class="sxs-lookup"><span data-stu-id="13c86-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="13c86-115">Bunun çalışması için önce her zamanki gibi ASP.NET AJAX ihtiyacımız `ScriptManager` , ASP.NET AJAX Denetim Araç Seti tarafından da kullanılan JavaScript kitaplıklarını yükler:</span><span class="sxs-lookup"><span data-stu-id="13c86-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

<span data-ttu-id="13c86-116">Ardından, bir metin kutusu gerekir:</span><span class="sxs-lookup"><span data-stu-id="13c86-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

<span data-ttu-id="13c86-117">Son olarak, `FilteredTextBoxExtender` denetim türü için kullanıcı izin karakter kısıtlama mvc'deki.</span><span class="sxs-lookup"><span data-stu-id="13c86-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="13c86-118">Öncelikle, ayarlamış `TargetControlID` özniteliğini `ID` , `TextBox` denetim.</span><span class="sxs-lookup"><span data-stu-id="13c86-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="13c86-119">Ardından, kullanılabilir birini `FilterType` değerler:</span><span class="sxs-lookup"><span data-stu-id="13c86-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="13c86-120">`Custom`Varsayılan; Geçerli karakter listesi sağlamak zorunda</span><span class="sxs-lookup"><span data-stu-id="13c86-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="13c86-121">`LowercaseLetters`yalnızca küçük harfler</span><span class="sxs-lookup"><span data-stu-id="13c86-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="13c86-122">`Numbers`yalnızca rakam</span><span class="sxs-lookup"><span data-stu-id="13c86-122">`Numbers` digits only</span></span>
- <span data-ttu-id="13c86-123">`UppercaseLetters`yalnızca büyük harfler</span><span class="sxs-lookup"><span data-stu-id="13c86-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="13c86-124">Varsa `Custom FilterType` kullanılan `ValidChars` özelliği ayarlanmış ve yazılı karakterlerin listesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="13c86-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="13c86-125">Bu arada: metin kutusuna metni yapıştırmayı deneyin, tüm geçersiz karakter kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="13c86-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="13c86-126">İçin biçimlendirme işte `FilteredTextBoxExtender` yalnızca rakam sağlayan denetimi (Ayrıca mümkün olması gereken bir şey `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="13c86-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

<span data-ttu-id="13c86-127">JavaScript etkinse, bir harf girmeyi deneyin ve sayfa Çalıştır çalışmaz; basamak ancak sayfasında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="13c86-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="13c86-128">Unutmayın ancak koruma `FilteredTextBox` sağlar madde işareti kanıt değil: varsa JavaScript etkinse, ek doğrulama anlamına gelir, yani ASP kullanmak zorunda şekilde herhangi bir veri metin kutusuna girilen. NET'in doğrulama denetimleri.</span><span class="sxs-lookup"><span data-stu-id="13c86-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="13c86-129">[![Yalnızca rakam girilebilir](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="13c86-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span></span>

<span data-ttu-id="13c86-130">Yalnızca rakam girilebilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="13c86-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="13c86-131">Önceki</span><span class="sxs-lookup"><span data-stu-id="13c86-131">Previous</span></span>](allowing-only-certain-characters-in-a-text-box-cs.md)
