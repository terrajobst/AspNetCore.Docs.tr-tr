---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: "Yalnızca belirli bir metin kutusu (C#) karakter izin verme | Microsoft Docs"
author: wenz
description: "ASP.NET doğrulama denetimleri, yalnızca belirli karakterler uygulamasında kullanıcı girdisi izin verildiğini emin olabilirsiniz. Ancak bu yazarak kullanıcıların geçersiz hala engellemez..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: 246c3b5dd55ceb0f47ad1f4982ae5b3bf855e747
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="f58e8-104">Yalnızca belirli bir metin kutusu (C#) karakter izin verme</span><span class="sxs-lookup"><span data-stu-id="f58e8-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>
====================
<span data-ttu-id="f58e8-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f58e8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f58e8-106">[Kodu indirme](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f58e8-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="f58e8-107">ASP.NET doğrulama denetimleri, yalnızca belirli karakterler uygulamasında kullanıcı girdisi izin verildiğini emin olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f58e8-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="f58e8-108">Ancak bu kullanıcıların geçersiz karakterler yazarak ve form göndermeye devam engellemez.</span><span class="sxs-lookup"><span data-stu-id="f58e8-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="f58e8-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="f58e8-109">Overview</span></span>

<span data-ttu-id="f58e8-110">ASP.NET doğrulama denetimleri, yalnızca belirli karakterler uygulamasında kullanıcı girdisi izin verildiğini emin olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f58e8-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="f58e8-111">Ancak bu kullanıcıların geçersiz karakterler yazarak ve form göndermeye devam engellemez.</span><span class="sxs-lookup"><span data-stu-id="f58e8-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="f58e8-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="f58e8-112">Steps</span></span>

<span data-ttu-id="f58e8-113">ASP.NET AJAX Denetim Araç Seti içeren `FilteredTextBox` bir metin kutusu genişleten denetim.</span><span class="sxs-lookup"><span data-stu-id="f58e8-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="f58e8-114">Etkinleştirilen sonra yalnızca belirli sayıda karakter alanına girilen.</span><span class="sxs-lookup"><span data-stu-id="f58e8-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="f58e8-115">Bunun çalışması için önce her zamanki gibi ASP.NET AJAX ihtiyacımız `ScriptManager` , ASP.NET AJAX Denetim Araç Seti tarafından da kullanılan JavaScript kitaplıklarını yükler:</span><span class="sxs-lookup"><span data-stu-id="f58e8-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="f58e8-116">Ardından, bir metin kutusu gerekir:</span><span class="sxs-lookup"><span data-stu-id="f58e8-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="f58e8-117">Son olarak, `FilteredTextBoxExtender` denetim türü için kullanıcı izin karakter kısıtlama mvc'deki.</span><span class="sxs-lookup"><span data-stu-id="f58e8-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="f58e8-118">Öncelikle, ayarlamış `TargetControlID` özniteliğini `ID` , `TextBox` denetim.</span><span class="sxs-lookup"><span data-stu-id="f58e8-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="f58e8-119">Ardından, kullanılabilir birini `FilterType` değerler:</span><span class="sxs-lookup"><span data-stu-id="f58e8-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="f58e8-120">`Custom`Varsayılan; Geçerli karakter listesi sağlamak zorunda</span><span class="sxs-lookup"><span data-stu-id="f58e8-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="f58e8-121">`LowercaseLetters`yalnızca küçük harfler</span><span class="sxs-lookup"><span data-stu-id="f58e8-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="f58e8-122">`Numbers`yalnızca rakam</span><span class="sxs-lookup"><span data-stu-id="f58e8-122">`Numbers` digits only</span></span>
- <span data-ttu-id="f58e8-123">`UppercaseLetters`yalnızca büyük harfler</span><span class="sxs-lookup"><span data-stu-id="f58e8-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="f58e8-124">Varsa `Custom FilterType` kullanılan `ValidChars` özelliği ayarlanmış ve yazılı karakterlerin listesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="f58e8-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="f58e8-125">Bu arada: metin kutusuna metni yapıştırmayı deneyin, tüm geçersiz karakter kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="f58e8-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="f58e8-126">İçin biçimlendirme işte `FilteredTextBoxExtender` yalnızca rakam sağlayan denetimi (Ayrıca mümkün olması gereken bir şey `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="f58e8-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="f58e8-127">JavaScript etkinse, bir harf girmeyi deneyin ve sayfa Çalıştır çalışmaz; basamak ancak sayfasında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f58e8-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="f58e8-128">Unutmayın ancak koruma `FilteredTextBox` sağlar madde işareti kanıt değil: varsa JavaScript etkinse, ek doğrulama anlamına gelir, yani ASP kullanmak zorunda şekilde herhangi bir veri metin kutusuna girilen. NET'in doğrulama denetimleri.</span><span class="sxs-lookup"><span data-stu-id="f58e8-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="f58e8-129">[![Yalnızca rakam girilebilir](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f58e8-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="f58e8-130">Yalnızca rakam girilebilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f58e8-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f58e8-131">Sonraki</span><span class="sxs-lookup"><span data-stu-id="f58e8-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
