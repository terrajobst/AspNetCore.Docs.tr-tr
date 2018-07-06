---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Mücadele botları (VB) | Microsoft Docs
author: wenz
description: Otomatik botlar, herhangi bir kullanıcı etkileşimi olmadan açıklama form gönderme istenmeyen posta, Web Günlükleri ve diğer Web siteleri yapıştırmaları. ASP.NET AJAX Con NoBot denetimi...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: e79a973f721c1feeddb00ecbf9d6a76786afb4bb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833449"
---
<a name="fighting-bots-vb"></a><span data-ttu-id="1ea6d-104">Mücadele botları (VB)</span><span class="sxs-lookup"><span data-stu-id="1ea6d-104">Fighting Bots (VB)</span></span>
====================
<span data-ttu-id="1ea6d-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1ea6d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1ea6d-106">[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="1ea6d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span></span>

> <span data-ttu-id="1ea6d-107">Otomatik botlar, herhangi bir kullanıcı etkileşimi olmadan açıklama form gönderme istenmeyen posta, Web Günlükleri ve diğer Web siteleri yapıştırmaları.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="1ea6d-108">ASP.NET AJAX Denetim Araç Seti NoBot denetimi bu botlar mücadele yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="1ea6d-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="1ea6d-109">Overview</span></span>

<span data-ttu-id="1ea6d-110">Otomatik botlar, herhangi bir kullanıcı etkileşimi olmadan açıklama form gönderme istenmeyen posta, Web Günlükleri ve diğer Web siteleri yapıştırmaları.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="1ea6d-111">ASP.NET AJAX Denetim Araç Seti NoBot denetimi bu botlar mücadele yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="1ea6d-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="1ea6d-112">Steps</span></span>

<span data-ttu-id="1ea6d-113">Botlar yenmek için kullanılan yaygın bir yaklaşım, bilgisayarlar ve insanlar uzaklıkta CAPTCHAs tamamen otomatik genel Turing test kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="1ea6d-114">Turing test bir test burada birisi iletişim ortak bir insan veya makine olup olmadığına karar vermek için gereken ilk oluştu.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="1ea6d-115">Web, görüntü üzerindeki bazı bozuk harflerle CAPTCHA genellikle oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="1ea6d-116">OCR algoritmaları başarısız olur ancak yalnızca bir insan görüntü harfler okuyabilirsiniz olur.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="1ea6d-117">Çeşitli avantajlar ve dezavantajlar Bu yaklaşımın vardır, ancak bu Serileştirmenin olduğundan bu öğreticinin kapsamı dışındadır.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="1ea6d-118">Yoktur ancak benzer bir yaklaşım sağlayan ASP.NET AJAX Denetim araç denetiminde: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="1ea6d-119">CAPTCHA üstesinden daha kolaydır, ancak kullanımı çok kolaydır ve son derece iyi burada bunu kabul edilir başarı girişimlerinin çoğu istenmeyen posta durumunda blogları gibi Web Siteleri'nde fares engellenmediğinden, hangi `NoBot` denetim gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="1ea6d-120">`NoBot` Bu koşullardan biri karşılanırsa geçerli ASP.NET web formunun geri göndermeyi durdurur:</span><span class="sxs-lookup"><span data-stu-id="1ea6d-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="1ea6d-121">Bir JavaScript Bulmacanın çözmek tarayıcı başarısız (örneğin, JavaScript kullanılmazsa)</span><span class="sxs-lookup"><span data-stu-id="1ea6d-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="1ea6d-122">Kullanıcı formu hızlı gönderildi</span><span class="sxs-lookup"><span data-stu-id="1ea6d-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="1ea6d-123">İstemci IP adresi, çok sık bir belirli zaman aralığında form gönderildi.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="1ea6d-124">Bu koşulu denetlemek için `NoBot` denetimi gerektirir, bu öznitelikler (tümünün isteğe bağlı):</span><span class="sxs-lookup"><span data-stu-id="1ea6d-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="1ea6d-125">`ResponseMinimumDelaySeconds` en düşük Geri göndermeler arasında saniye miktarı</span><span class="sxs-lookup"><span data-stu-id="1ea6d-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="1ea6d-126">`CutoffWindowSeconds` bir IP gelen geri göndermeleri ölçüler olduğu süre uzunluğu</span><span class="sxs-lookup"><span data-stu-id="1ea6d-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="1ea6d-127">`CutoffMaximumInstances` en uzun süreyi saniye başına zaman aralığı</span><span class="sxs-lookup"><span data-stu-id="1ea6d-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="1ea6d-128">Aşağıdaki biçimlendirme taleplerini, en az iki saniye Geri göndermeler arasında geçmesi ve yalnızca beş Geri göndermeler vardır veya 30 saniyelik bir aralıkta içinde daha az:</span><span class="sxs-lookup"><span data-stu-id="1ea6d-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

<span data-ttu-id="1ea6d-129">Sonra her zamanki şekilde eklediğinizden emin olun `ScriptManager` sayfasında ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="1ea6d-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

<span data-ttu-id="1ea6d-130">Denetimleri çoğunu beri `NoBot` yapıyor ihtiyacınız bu doğrulamaları sonucunu denetlemek sunucu tarafında oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="1ea6d-131">Bu çağrı yaparak yapılabilir `NoBot`'s `IsValid()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="1ea6d-132">Bir bağımsız değişkene sahip (olarak bir `out` parametre /`ByRef` parametresi) türünde `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="1ea6d-133">Denetim başarısız olduğunda, dize gösterimi nedeni içerir ve `Valid` Aksi takdirde.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="1ea6d-134">Aşağıdaki kod bir ileti göre çıkarır `NoBot`kullanıcının neden:</span><span class="sxs-lookup"><span data-stu-id="1ea6d-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

<span data-ttu-id="1ea6d-135">Son olarak, bir form gönderme ve iletinin çıktısını almak için bir etiket öğesi gerekir ve işiniz!</span><span class="sxs-lookup"><span data-stu-id="1ea6d-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

<span data-ttu-id="1ea6d-136">Bu betiği çalıştırın ve JavaScript devre dışı veya formun ilk iki saniye içinde gönderdiğinizde veya form gönderme yedi kat otuz saniye içinde bir hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="1ea6d-137">JavaScript etkin kullanıcılar yalnızca yaklaşık % 90 95 sahip olduğundan, bu nedenle kullanıcı 5-%10 başarısız olur, ancak bu denetim akıllıca kullanmanız `NoBot`kullanıcının test edin.</span><span class="sxs-lookup"><span data-stu-id="1ea6d-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="1ea6d-138">[![Bu hata iletisi bir robot tarafından olmuş](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1ea6d-138">[![This error message could have been caused by a bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span></span>

<span data-ttu-id="1ea6d-139">Bu hata iletisi bir robot tarafından olmuş ([tam boyutlu görüntüyü görmek için tıklatın](fighting-bots-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1ea6d-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1ea6d-140">Önceki</span><span class="sxs-lookup"><span data-stu-id="1ea6d-140">Previous</span></span>](fighting-bots-cs.md)
