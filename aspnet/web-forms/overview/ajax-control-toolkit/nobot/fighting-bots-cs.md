---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Mücadele botları (C#) | Microsoft Docs
author: wenz
description: Otomatik botlar, herhangi bir kullanıcı etkileşimi olmadan açıklama form gönderme istenmeyen posta, Web Günlükleri ve diğer Web siteleri yapıştırmaları. ASP.NET AJAX Con NoBot denetimi...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: 52ed34e7640cd125a3b4c3b50ab760a7c1d713f1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754697"
---
<a name="fighting-bots-c"></a><span data-ttu-id="d3852-104">Mücadele botları (C#)</span><span class="sxs-lookup"><span data-stu-id="d3852-104">Fighting Bots (C#)</span></span>
====================
<span data-ttu-id="d3852-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d3852-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d3852-106">[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d3852-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span></span>

> <span data-ttu-id="d3852-107">Otomatik botlar, herhangi bir kullanıcı etkileşimi olmadan açıklama form gönderme istenmeyen posta, Web Günlükleri ve diğer Web siteleri yapıştırmaları.</span><span class="sxs-lookup"><span data-stu-id="d3852-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="d3852-108">ASP.NET AJAX Denetim Araç Seti NoBot denetimi bu botlar mücadele yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="d3852-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="d3852-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="d3852-109">Overview</span></span>

<span data-ttu-id="d3852-110">Otomatik botlar, herhangi bir kullanıcı etkileşimi olmadan açıklama form gönderme istenmeyen posta, Web Günlükleri ve diğer Web siteleri yapıştırmaları.</span><span class="sxs-lookup"><span data-stu-id="d3852-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="d3852-111">ASP.NET AJAX Denetim Araç Seti NoBot denetimi bu botlar mücadele yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="d3852-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="d3852-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="d3852-112">Steps</span></span>

<span data-ttu-id="d3852-113">Botlar yenmek için kullanılan yaygın bir yaklaşım, bilgisayarlar ve insanlar uzaklıkta CAPTCHAs tamamen otomatik genel Turing test kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="d3852-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="d3852-114">Turing test bir test burada birisi iletişim ortak bir insan veya makine olup olmadığına karar vermek için gereken ilk oluştu.</span><span class="sxs-lookup"><span data-stu-id="d3852-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="d3852-115">Web, görüntü üzerindeki bazı bozuk harflerle CAPTCHA genellikle oluşur.</span><span class="sxs-lookup"><span data-stu-id="d3852-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="d3852-116">OCR algoritmaları başarısız olur ancak yalnızca bir insan görüntü harfler okuyabilirsiniz olur.</span><span class="sxs-lookup"><span data-stu-id="d3852-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="d3852-117">Çeşitli avantajlar ve dezavantajlar Bu yaklaşımın vardır, ancak bu Serileştirmenin olduğundan bu öğreticinin kapsamı dışındadır.</span><span class="sxs-lookup"><span data-stu-id="d3852-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="d3852-118">Yoktur ancak benzer bir yaklaşım sağlayan ASP.NET AJAX Denetim araç denetiminde: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="d3852-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="d3852-119">CAPTCHA üstesinden daha kolaydır, ancak kullanımı çok kolaydır ve son derece iyi burada bunu kabul edilir başarı girişimlerinin çoğu istenmeyen posta durumunda blogları gibi Web Siteleri'nde fares engellenmediğinden, hangi `NoBot` denetim gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3852-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="d3852-120">`NoBot` Bu koşullardan biri karşılanırsa geçerli ASP.NET web formunun geri göndermeyi durdurur:</span><span class="sxs-lookup"><span data-stu-id="d3852-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="d3852-121">Bir JavaScript Bulmacanın çözmek tarayıcı başarısız (örneğin, JavaScript kullanılmazsa)</span><span class="sxs-lookup"><span data-stu-id="d3852-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="d3852-122">Kullanıcı formu hızlı gönderildi</span><span class="sxs-lookup"><span data-stu-id="d3852-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="d3852-123">İstemci IP adresi, çok sık bir belirli zaman aralığında form gönderildi.</span><span class="sxs-lookup"><span data-stu-id="d3852-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="d3852-124">Bu koşulu denetlemek için `NoBot` denetimi gerektirir, bu öznitelikler (tümünün isteğe bağlı):</span><span class="sxs-lookup"><span data-stu-id="d3852-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="d3852-125">`ResponseMinimumDelaySeconds` en düşük Geri göndermeler arasında saniye miktarı</span><span class="sxs-lookup"><span data-stu-id="d3852-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="d3852-126">`CutoffWindowSeconds` bir IP gelen geri göndermeleri ölçüler olduğu süre uzunluğu</span><span class="sxs-lookup"><span data-stu-id="d3852-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="d3852-127">`CutoffMaximumInstances` en uzun süreyi saniye başına zaman aralığı</span><span class="sxs-lookup"><span data-stu-id="d3852-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="d3852-128">Aşağıdaki biçimlendirme taleplerini, en az iki saniye Geri göndermeler arasında geçmesi ve yalnızca beş Geri göndermeler vardır veya 30 saniyelik bir aralıkta içinde daha az:</span><span class="sxs-lookup"><span data-stu-id="d3852-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

<span data-ttu-id="d3852-129">Sonra her zamanki şekilde eklediğinizden emin olun `ScriptManager` sayfasında ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="d3852-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

<span data-ttu-id="d3852-130">Denetimleri çoğunu beri `NoBot` yapıyor ihtiyacınız bu doğrulamaları sonucunu denetlemek sunucu tarafında oluşur.</span><span class="sxs-lookup"><span data-stu-id="d3852-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="d3852-131">Bu çağrı yaparak yapılabilir `NoBot`'s `IsValid()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d3852-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="d3852-132">Bir bağımsız değişkene sahip (olarak bir `out` parametre /`ByRef` parametresi) türünde `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="d3852-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="d3852-133">Denetim başarısız olduğunda, dize gösterimi nedeni içerir ve `Valid` Aksi takdirde.</span><span class="sxs-lookup"><span data-stu-id="d3852-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="d3852-134">Aşağıdaki kod bir ileti göre çıkarır `NoBot`kullanıcının neden:</span><span class="sxs-lookup"><span data-stu-id="d3852-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

<span data-ttu-id="d3852-135">Son olarak, bir form gönderme ve iletinin çıktısını almak için bir etiket öğesi gerekir ve işiniz!</span><span class="sxs-lookup"><span data-stu-id="d3852-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

<span data-ttu-id="d3852-136">Bu betiği çalıştırın ve JavaScript devre dışı veya formun ilk iki saniye içinde gönderdiğinizde veya form gönderme yedi kat otuz saniye içinde bir hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="d3852-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="d3852-137">JavaScript etkin kullanıcılar yalnızca yaklaşık % 90 95 sahip olduğundan, bu nedenle kullanıcı 5-%10 başarısız olur, ancak bu denetim akıllıca kullanmanız `NoBot`kullanıcının test edin.</span><span class="sxs-lookup"><span data-stu-id="d3852-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="d3852-138">[![Bu hata iletisi bir robot tarafından olmuş](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d3852-138">[![This error message could have been caused by a bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span></span>

<span data-ttu-id="d3852-139">Bu hata iletisi bir robot tarafından olmuş ([tam boyutlu görüntüyü görmek için tıklatın](fighting-bots-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d3852-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d3852-140">Next</span><span class="sxs-lookup"><span data-stu-id="d3852-140">Next</span></span>](fighting-bots-vb.md)
