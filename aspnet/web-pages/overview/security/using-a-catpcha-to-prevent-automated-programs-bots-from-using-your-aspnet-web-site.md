---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Bot, ASP.NET Web Razor kullanmasını önlemek için bir güvenlik kodu kullanarak) Site | Microsoft Docs
author: microsoft
description: Bu makalede bir ASP.NET Web Pages'da (Razor) görevler önleyecek otomatik programları (aracılarını) ReCaptcha (bir güvenlik önlemi) kullanımı açıklanmaktadır biz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573282"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="73ee0-103">Site bot, ASP.NET Web Razor kullanmasını önlemek için bir güvenlik kodu kullanarak)</span><span class="sxs-lookup"><span data-stu-id="73ee0-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>
====================
<span data-ttu-id="73ee0-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="73ee0-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="73ee0-105">Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesi görevleri önleyecek otomatik programları (aracılarını) ReCaptcha (bir güvenlik önlemi) kullanımı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="73ee0-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="73ee0-106">**Öğrenecekleriniz:**</span><span class="sxs-lookup"><span data-stu-id="73ee0-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="73ee0-107">Bir güvenlik kodu test sitenize ekleme.</span><span class="sxs-lookup"><span data-stu-id="73ee0-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="73ee0-108">Bu makalede sunulan ASP.NET özellikleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="73ee0-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="73ee0-109">`ReCaptcha` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="73ee0-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="73ee0-110">Bu makaledeki bilgileri, ASP.NET Web sayfaları 1.0 ve Web Pages 2 için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="73ee0-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>


## <a name="about-captchas"></a><span data-ttu-id="73ee0-111">CAPTCHAs hakkında</span><span class="sxs-lookup"><span data-stu-id="73ee0-111">About CAPTCHAs</span></span>

<span data-ttu-id="73ee0-112">Kaydetme kullanıcıların, sitenizde veya hatta yalnızca izin vermek istediğiniz zaman bir adı ve URL (like blog yorum için) girin, sahte adlarının sel alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="73ee0-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="73ee0-113">Bunlar genellikle bulabilirsiniz her Web sitesinin URL'leri bırakmayı deneyin otomatik programları (aracılarını) tarafından bırakılır.</span><span class="sxs-lookup"><span data-stu-id="73ee0-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="73ee0-114">(Bir ortak motivasyon, satış ürünleri URL'lerini yerleştirmektir.)</span><span class="sxs-lookup"><span data-stu-id="73ee0-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="73ee0-115">Bir kullanıcının gerçek kişinin ve bilgisayar programı kullanarak olduğundan emin olun yardımcı bir *CAPTCHA* kaydetmek veya aksi halde, adlarını ve site girin kullanıcıları doğrulamak için.</span><span class="sxs-lookup"><span data-stu-id="73ee0-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="73ee0-116">CAPTCHA bilgisayarlar ve insanlar birbirinden bildirmek tamamen otomatik ortak Turing test için anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="73ee0-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="73ee0-117">Bir güvenlik kodu olduğu bir *sınama yanıtı* yapmak bir kişi için kolay, ancak sabit bir otomatik programın yapmak, kullanıcı sorulan bir şeyler için test.</span><span class="sxs-lookup"><span data-stu-id="73ee0-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="73ee0-118">CAPTCHA en yaygın türü burada bazı bozuk harf bakın ve bunları yazın istenir biridir.</span><span class="sxs-lookup"><span data-stu-id="73ee0-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="73ee0-119">(Bozulma harfleri geçirilirse aracılarını için sabit yapmak için varsayılır.)</span><span class="sxs-lookup"><span data-stu-id="73ee0-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="73ee0-120">ReCaptcha testine ekleme</span><span class="sxs-lookup"><span data-stu-id="73ee0-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="73ee0-121">ASP.NET sayfaları, kullandığınız `ReCaptcha` ReCaptcha hizmetini temel alan bir güvenlik kodu test oluşturmak için yardımcı ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="73ee0-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="73ee0-122">`ReCaptcha` Yardımcısı, kullanıcılar sayfa doğrulanır önce doğru girmek zorunda iki bozuk sözcükler görüntüsünü görüntüler.</span><span class="sxs-lookup"><span data-stu-id="73ee0-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="73ee0-123">Kullanıcı yanıtı ReCaptcha.Net hizmeti tarafından doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="73ee0-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="73ee0-124">Web sitenizi ReCaptcha.Net adresindeki kayıt ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="73ee0-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="73ee0-125">Kayıt tamamladıktan sonra bir ortak anahtar ve özel anahtarı elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="73ee0-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="73ee0-126">ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.</span><span class="sxs-lookup"><span data-stu-id="73ee0-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="73ee0-127">Henüz yoksa bir  *\_AppStart.cshtml* dosya, bir Web sitesinin kök klasöründe adlı bir dosya oluşturun  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="73ee0-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="73ee0-128">Aşağıdakileri ekleyin `Recaptcha` yardımcı ayarlarında  *\_AppStart.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="73ee0-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="73ee0-129">Ayarlama `PublicKey` ve `PrivateKey` kendi ortak ve özel anahtarları kullanarak özellikleri.</span><span class="sxs-lookup"><span data-stu-id="73ee0-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="73ee0-130">Kaydet  *\_AppStart.cshtml* dosya ve kapatın.</span><span class="sxs-lookup"><span data-stu-id="73ee0-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="73ee0-131">Adlı yeni bir sayfa bir Web sitesinin kök klasörü oluşturmak *Recaptcha.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="73ee0-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="73ee0-132">Varolan içeriği aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="73ee0-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="73ee0-133">Çalıştırma *Recaptcha.cshtml* sayfasını bir tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="73ee0-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="73ee0-134">Varsa `PrivateKey` değeri geçerli, bir düğmeyi ve ReCaptcha denetimi sayfasında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="73ee0-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="73ee0-135">Anahtarları içinde genel olarak olmayan ayarlarsanız  *\_AppStart.html*, sayfaya bir hata görüntüler.</span><span class="sxs-lookup"><span data-stu-id="73ee0-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="73ee0-136">Test için sözcükleri girin.</span><span class="sxs-lookup"><span data-stu-id="73ee0-136">Enter the words for the test.</span></span> <span data-ttu-id="73ee0-137">ReCaptcha testi başarılı olursa, bunu belirten bir ileti görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="73ee0-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="73ee0-138">Aksi takdirde bir hata iletisi görüntülenir ve ReCaptcha denetim görünürler.</span><span class="sxs-lookup"><span data-stu-id="73ee0-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="73ee0-139">Bilgisayarınızı proxy sunucusu kullanan bir etki alanındaysa, yapılandırmanız gerekebilir `defaultproxy` öğesinin *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="73ee0-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="73ee0-140">Aşağıdaki örnekte gösterildiği bir *Web.config* ile dosya `defaultproxy` ReCaptcha hizmetin çalışmasını etkinleştirmek için yapılandırılmış öğesi.</span><span class="sxs-lookup"><span data-stu-id="73ee0-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="73ee0-141">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="73ee0-141">Additional Resources</span></span>


- [<span data-ttu-id="73ee0-142">ASP.NET Web sayfaları siteleri için site genelinde davranışını özelleştirme</span><span class="sxs-lookup"><span data-stu-id="73ee0-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="73ee0-143">ReCaptcha site</span><span class="sxs-lookup"><span data-stu-id="73ee0-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
