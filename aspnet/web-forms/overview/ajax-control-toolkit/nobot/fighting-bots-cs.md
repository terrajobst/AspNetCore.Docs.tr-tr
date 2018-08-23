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
<a name="fighting-bots-c"></a>Mücadele botları (C#)
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)

> Otomatik botlar, herhangi bir kullanıcı etkileşimi olmadan açıklama form gönderme istenmeyen posta, Web Günlükleri ve diğer Web siteleri yapıştırmaları. ASP.NET AJAX Denetim Araç Seti NoBot denetimi bu botlar mücadele yardımcı olabilir.


## <a name="overview"></a>Genel Bakış

Otomatik botlar, herhangi bir kullanıcı etkileşimi olmadan açıklama form gönderme istenmeyen posta, Web Günlükleri ve diğer Web siteleri yapıştırmaları. ASP.NET AJAX Denetim Araç Seti NoBot denetimi bu botlar mücadele yardımcı olabilir.

## <a name="steps"></a>Adımlar

Botlar yenmek için kullanılan yaygın bir yaklaşım, bilgisayarlar ve insanlar uzaklıkta CAPTCHAs tamamen otomatik genel Turing test kullanmaktır. Turing test bir test burada birisi iletişim ortak bir insan veya makine olup olmadığına karar vermek için gereken ilk oluştu. Web, görüntü üzerindeki bazı bozuk harflerle CAPTCHA genellikle oluşur. OCR algoritmaları başarısız olur ancak yalnızca bir insan görüntü harfler okuyabilirsiniz olur.

Çeşitli avantajlar ve dezavantajlar Bu yaklaşımın vardır, ancak bu Serileştirmenin olduğundan bu öğreticinin kapsamı dışındadır. Yoktur ancak benzer bir yaklaşım sağlayan ASP.NET AJAX Denetim araç denetiminde: `NoBot`. CAPTCHA üstesinden daha kolaydır, ancak kullanımı çok kolaydır ve son derece iyi burada bunu kabul edilir başarı girişimlerinin çoğu istenmeyen posta durumunda blogları gibi Web Siteleri'nde fares engellenmediğinden, hangi `NoBot` denetim gerçekleştirebilirsiniz.

`NoBot` Bu koşullardan biri karşılanırsa geçerli ASP.NET web formunun geri göndermeyi durdurur:

- Bir JavaScript Bulmacanın çözmek tarayıcı başarısız (örneğin, JavaScript kullanılmazsa)
- Kullanıcı formu hızlı gönderildi
- İstemci IP adresi, çok sık bir belirli zaman aralığında form gönderildi.

Bu koşulu denetlemek için `NoBot` denetimi gerektirir, bu öznitelikler (tümünün isteğe bağlı):

- `ResponseMinimumDelaySeconds` en düşük Geri göndermeler arasında saniye miktarı
- `CutoffWindowSeconds` bir IP gelen geri göndermeleri ölçüler olduğu süre uzunluğu
- `CutoffMaximumInstances` en uzun süreyi saniye başına zaman aralığı

Aşağıdaki biçimlendirme taleplerini, en az iki saniye Geri göndermeler arasında geçmesi ve yalnızca beş Geri göndermeler vardır veya 30 saniyelik bir aralıkta içinde daha az:

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

Sonra her zamanki şekilde eklediğinizden emin olun `ScriptManager` sayfasında ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

Denetimleri çoğunu beri `NoBot` yapıyor ihtiyacınız bu doğrulamaları sonucunu denetlemek sunucu tarafında oluşur. Bu çağrı yaparak yapılabilir `NoBot`'s `IsValid()` yöntemi. Bir bağımsız değişkene sahip (olarak bir `out` parametre /`ByRef` parametresi) türünde `NoBotState`. Denetim başarısız olduğunda, dize gösterimi nedeni içerir ve `Valid` Aksi takdirde. Aşağıdaki kod bir ileti göre çıkarır `NoBot`kullanıcının neden:

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

Son olarak, bir form gönderme ve iletinin çıktısını almak için bir etiket öğesi gerekir ve işiniz!

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

Bu betiği çalıştırın ve JavaScript devre dışı veya formun ilk iki saniye içinde gönderdiğinizde veya form gönderme yedi kat otuz saniye içinde bir hata iletisi alırsınız. JavaScript etkin kullanıcılar yalnızca yaklaşık % 90 95 sahip olduğundan, bu nedenle kullanıcı 5-%10 başarısız olur, ancak bu denetim akıllıca kullanmanız `NoBot`kullanıcının test edin.


[![Bu hata iletisi bir robot tarafından olmuş](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)

Bu hata iletisi bir robot tarafından olmuş ([tam boyutlu görüntüyü görmek için tıklatın](fighting-bots-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](fighting-bots-vb.md)
