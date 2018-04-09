---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Bot (VB) mücadele | Microsoft Docs
author: wenz
description: Otomatik bot, herhangi bir kullanıcı etkileşimi olmadan yorum form gönderme istenmeyen posta, Web Günlükleri ve diğer Web siteleri yapıştırmaları. ASP.NET AJAX Con NoBot denetiminde...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: 35d5984ac7ff3422bab07a759c93fef3914a22f7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="fighting-bots-vb"></a><span data-ttu-id="dec18-104">Mücadele aracılarını (VB)</span><span class="sxs-lookup"><span data-stu-id="dec18-104">Fighting Bots (VB)</span></span>
====================
<span data-ttu-id="dec18-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="dec18-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="dec18-106">[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="dec18-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span></span>

> <span data-ttu-id="dec18-107">Otomatik bot, herhangi bir kullanıcı etkileşimi olmadan yorum form gönderme istenmeyen posta, Web Günlükleri ve diğer Web siteleri yapıştırmaları.</span><span class="sxs-lookup"><span data-stu-id="dec18-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="dec18-108">ASP.NET AJAX Denetim Araç Seti NoBot denetiminde bu aracılarını mücadele yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="dec18-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="dec18-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="dec18-109">Overview</span></span>

<span data-ttu-id="dec18-110">Otomatik bot, herhangi bir kullanıcı etkileşimi olmadan yorum form gönderme istenmeyen posta, Web Günlükleri ve diğer Web siteleri yapıştırmaları.</span><span class="sxs-lookup"><span data-stu-id="dec18-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="dec18-111">ASP.NET AJAX Denetim Araç Seti NoBot denetiminde bu aracılarını mücadele yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="dec18-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="dec18-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="dec18-112">Steps</span></span>

<span data-ttu-id="dec18-113">Bot üstesinden gelmek için ortak bir yaklaşım, bilgisayarlar ve insanlar birbirinden bildirmek için CAPTCHAs tamamen otomatik ortak Turing test kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="dec18-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="dec18-114">Turing test burada birisi iletişimi ortak bir insan veya bir makine olup olmadığına karar vermek için gereken bir test başlangıçta oluştu.</span><span class="sxs-lookup"><span data-stu-id="dec18-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="dec18-115">Web, bir güvenlik kodu genellikle, bozuk bazı harflerin görüntüyle oluşur.</span><span class="sxs-lookup"><span data-stu-id="dec18-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="dec18-116">OCR algoritmaları başarısız olur ancak yalnızca bir insan görüntü harfler okuyabildiğini olur.</span><span class="sxs-lookup"><span data-stu-id="dec18-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="dec18-117">Çeşitli avantajları ve dezavantajları Bu yaklaşımın vardır, ancak bu tartışması Bu öğretici kapsamında değildir.</span><span class="sxs-lookup"><span data-stu-id="dec18-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="dec18-118">Yoktur ancak benzer bir yaklaşım sağlayan ASP.NET AJAX Denetim araç denetiminde: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="dec18-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="dec18-119">Bir güvenlik kodu üstesinden daha kolaydır, ancak kullanımı çok kolaydır ve çok iyi nerede kabul başarı çoğu denemeleri istenmeyen posta durumunda bloglar gibi sitelerindeki fares engellenmediğinden, hangi `NoBot` denetim yapabilir.</span><span class="sxs-lookup"><span data-stu-id="dec18-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="dec18-120">`NoBot` Bu koşullardan biri karşılandığında geçerli ASP.NET web formunun geri gönderme karşılar:</span><span class="sxs-lookup"><span data-stu-id="dec18-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="dec18-121">JavaScript Bulmacanın çözmek tarayıcı başarısız (örneğin, JavaScript kullanılmazsa)</span><span class="sxs-lookup"><span data-stu-id="dec18-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="dec18-122">Kullanıcı formu hızlı gönderildi</span><span class="sxs-lookup"><span data-stu-id="dec18-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="dec18-123">İstemci IP adresi çok sık bir belirli süre içinde form gönderildi.</span><span class="sxs-lookup"><span data-stu-id="dec18-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="dec18-124">Bu koşullar için denetlemek için `NoBot` denetimi gerektirir bu öznitelikler (bunların tümü isteğe bağlı):</span><span class="sxs-lookup"><span data-stu-id="dec18-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="dec18-125">`ResponseMinimumDelaySeconds` az miktarda Geri göndermeler arasındaki saniye</span><span class="sxs-lookup"><span data-stu-id="dec18-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="dec18-126">`CutoffWindowSeconds` bir IP adresinden Geri göndermeler ölçüleri olduğu zaman aralığı uzunluğu</span><span class="sxs-lookup"><span data-stu-id="dec18-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="dec18-127">`CutoffMaximumInstances` Maksimum saniyede bir zaman aralığı</span><span class="sxs-lookup"><span data-stu-id="dec18-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="dec18-128">Geri göndermeler arasında aşağıdaki biçimlendirme taleplerini bu en az iki saniye geçtikten ve yalnızca beş Geri göndermeler vardır veya 30 saniyelik bir aralıkta içinde daha az:</span><span class="sxs-lookup"><span data-stu-id="dec18-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

<span data-ttu-id="dec18-129">Sonra her zamanki gibi eklediğinizden emin olun `ScriptManager` sayfasındaki ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="dec18-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

<span data-ttu-id="dec18-130">Denetimleri çoğunu itibaren `NoBot` yaptığını ihtiyacınız bu doğrulama sonucu denetlemek sunucu tarafında oluşur.</span><span class="sxs-lookup"><span data-stu-id="dec18-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="dec18-131">Bu çağırarak yapılabilir `NoBot`'s `IsValid()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="dec18-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="dec18-132">Bir bağımsız değişkene sahip (olarak bir `out` parametre /`ByRef` parametresi) türündeki `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="dec18-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="dec18-133">Denetim başarısız olduğunda, dize gösterimi nedeni içerir ve `Valid` Aksi takdirde.</span><span class="sxs-lookup"><span data-stu-id="dec18-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="dec18-134">Aşağıdaki kod bir ileti göre çıkarır `NoBot`kullanıcının neden:</span><span class="sxs-lookup"><span data-stu-id="dec18-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

<span data-ttu-id="dec18-135">Son olarak, bir form gönderme ve ileti çıktısını almak için bir etiket öğesi gerekir ve tamamladığınızda!</span><span class="sxs-lookup"><span data-stu-id="dec18-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

<span data-ttu-id="dec18-136">Bu komut dosyasını çalıştırın ve JavaScript devre dışı bırakma veya ilk iki saniye içinde formu göndermeden veya yedi defa otuz saniye içinde form gönderme sırasında bir hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="dec18-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="dec18-137">Ancak bu denetim akıllıca kullanmanız, kullanıcılar yalnızca yaklaşık % 90 95 JavaScript etkinleştirilmiş olduğundan, bu nedenle kullanıcı 5-%10 başarısız olur `NoBot`kullanıcının sınayın.</span><span class="sxs-lookup"><span data-stu-id="dec18-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="dec18-138">[![Bu hata iletisini tarafından bot olmuş](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dec18-138">[![This error message could have been caused by a bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span></span>

<span data-ttu-id="dec18-139">Bu hata iletisini tarafından bot olmuş ([tam boyutlu görüntüyü görüntülemek için tıklatın](fighting-bots-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dec18-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="dec18-140">Önceki</span><span class="sxs-lookup"><span data-stu-id="dec18-140">Previous</span></span>](fighting-bots-cs.md)
