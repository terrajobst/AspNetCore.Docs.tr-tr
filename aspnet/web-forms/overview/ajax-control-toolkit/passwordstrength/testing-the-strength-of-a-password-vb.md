---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Bir parola (VB) gücünü sınama | Microsoft Docs
author: wenz
description: Parolalar, neredeyse her yerden, böylece yavaş kullanıcıları ayırmak kolay olan basit parolalar seçmesini eğilimindedir gereklidir. ASP PasswordStrength denetiminde. N...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 1d46026535f3f5cf82944359599464e8a4725280
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879458"
---
<a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="18d87-104">Bir parola (VB) gücünü test etme</span><span class="sxs-lookup"><span data-stu-id="18d87-104">Testing the Strength of a Password (VB)</span></span>
====================
<span data-ttu-id="18d87-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="18d87-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="18d87-106">[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="18d87-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="18d87-107">Parolalar, neredeyse her yerden, böylece yavaş kullanıcıları ayırmak kolay olan basit parolalar seçmesini eğilimindedir gereklidir.</span><span class="sxs-lookup"><span data-stu-id="18d87-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="18d87-108">ASP.NET AJAX Denetim Araç Seti PasswordStrength denetiminde ne kadar iyi bir paroladır kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="18d87-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="18d87-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="18d87-109">Overview</span></span>

<span data-ttu-id="18d87-110">Parolalar, neredeyse her yerden, böylece yavaş kullanıcıları ayırmak kolay olan basit parolalar seçmesini eğilimindedir gereklidir.</span><span class="sxs-lookup"><span data-stu-id="18d87-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="18d87-111">`PasswordStrength` ASP.NET AJAX Denetim Araç Seti denetiminde ne kadar iyi bir paroladır kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="18d87-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="18d87-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="18d87-112">Steps</span></span>

<span data-ttu-id="18d87-113">`PasswordStrength` Denetim metin kutusu genişletir ve parola yeterince iyi olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="18d87-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="18d87-114">Bol miktarda öznitelikleri aracılığıyla seçenekleri sunar. bunları yalnızca bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="18d87-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="18d87-115">`MinimumNumericCharacters` Parolada kullanılması gereken sayı karakteri sayısının alt sınırı</span><span class="sxs-lookup"><span data-stu-id="18d87-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="18d87-116">`MinimumSymbolCharacters` Parolada kullanılması gereken en az sayıda simge karakteri (harfler ve sayılar değil)</span><span class="sxs-lookup"><span data-stu-id="18d87-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="18d87-117">`PreferredPasswordLength` Minimum parola uzunluğu</span><span class="sxs-lookup"><span data-stu-id="18d87-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="18d87-118">`RequiresUpperAndLowerCaseCharacters` olup büyük ve küçük harf karakterler kullanmak parola gerekiyor</span><span class="sxs-lookup"><span data-stu-id="18d87-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="18d87-119">`StrengthIndicatorType` Metin olarak parola gücünü sunmak nasıl bilgiler sunar (değer `"Text"`) veya bir ilerleme çubuğu tür olarak (değer `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="18d87-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="18d87-120">İçinde `DisplayPosition` özniteliği, yapılandırma bilgileri göründüğü.</span><span class="sxs-lookup"><span data-stu-id="18d87-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="18d87-121">ASP.NET AJAX dahil olmak üzere tam bir örnek, işte `ScriptManager` denetimi `PasswordStrength` denetimi ve tabi ki burada kullanıcı girebilirsiniz parola metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="18d87-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="18d87-122">Geliştirme sırasında yazmakta olduğunuz görebilmeniz için tanıtım amacıyla, ikinci form bir normal metin alanı ve parola alanı alanıdır.</span><span class="sxs-lookup"><span data-stu-id="18d87-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="18d87-123">Sayfayı çalıştırın ve hemen yazın: yalnızca küçük harfler, büyük harfler, rakamlar ve semboller girdikten sonra parola olarak kesilemeyen kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="18d87-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="18d87-124">[![Parola (oldukça) iyi sunulmuştur](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="18d87-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="18d87-125">Parola (oldukça) iyi şimdi ([tam boyutlu görüntüyü görüntülemek için tıklatın](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="18d87-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="18d87-126">Önceki</span><span class="sxs-lookup"><span data-stu-id="18d87-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
