---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Bot (C#) mücadele | Microsoft Docs
author: wenz
description: Otomatik bot, herhangi bir kullanıcı etkileşimi olmadan yorum form gönderme istenmeyen posta, Web Günlükleri ve diğer Web siteleri yapıştırmaları. ASP.NET AJAX Con NoBot denetiminde...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: 1ea3aaa5461c2f58a927ae975568f18a34a4729b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="fighting-bots-c"></a>Mücadele aracılarını (C#)
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)

> Otomatik bot, herhangi bir kullanıcı etkileşimi olmadan yorum form gönderme istenmeyen posta, Web Günlükleri ve diğer Web siteleri yapıştırmaları. ASP.NET AJAX Denetim Araç Seti NoBot denetiminde bu aracılarını mücadele yardımcı olabilir.


## <a name="overview"></a>Genel Bakış

Otomatik bot, herhangi bir kullanıcı etkileşimi olmadan yorum form gönderme istenmeyen posta, Web Günlükleri ve diğer Web siteleri yapıştırmaları. ASP.NET AJAX Denetim Araç Seti NoBot denetiminde bu aracılarını mücadele yardımcı olabilir.

## <a name="steps"></a>Adımlar

Bot üstesinden gelmek için ortak bir yaklaşım, bilgisayarlar ve insanlar birbirinden bildirmek için CAPTCHAs tamamen otomatik ortak Turing test kullanmaktır. Turing test burada birisi iletişimi ortak bir insan veya bir makine olup olmadığına karar vermek için gereken bir test başlangıçta oluştu. Web, bir güvenlik kodu genellikle, bozuk bazı harflerin görüntüyle oluşur. OCR algoritmaları başarısız olur ancak yalnızca bir insan görüntü harfler okuyabildiğini olur.

Çeşitli avantajları ve dezavantajları Bu yaklaşımın vardır, ancak bu tartışması Bu öğretici kapsamında değildir. Yoktur ancak benzer bir yaklaşım sağlayan ASP.NET AJAX Denetim araç denetiminde: `NoBot`. Bir güvenlik kodu üstesinden daha kolaydır, ancak kullanımı çok kolaydır ve çok iyi nerede kabul başarı çoğu denemeleri istenmeyen posta durumunda bloglar gibi sitelerindeki fares engellenmediğinden, hangi `NoBot` denetim yapabilir.

`NoBot` Bu koşullardan biri karşılandığında geçerli ASP.NET web formunun geri gönderme karşılar:

- JavaScript Bulmacanın çözmek tarayıcı başarısız (örneğin, JavaScript kullanılmazsa)
- Kullanıcı formu hızlı gönderildi
- İstemci IP adresi çok sık bir belirli süre içinde form gönderildi.

Bu koşullar için denetlemek için `NoBot` denetimi gerektirir bu öznitelikler (bunların tümü isteğe bağlı):

- `ResponseMinimumDelaySeconds` az miktarda Geri göndermeler arasındaki saniye
- `CutoffWindowSeconds` bir IP adresinden Geri göndermeler ölçüleri olduğu zaman aralığı uzunluğu
- `CutoffMaximumInstances` Maksimum saniyede bir zaman aralığı

Geri göndermeler arasında aşağıdaki biçimlendirme taleplerini bu en az iki saniye geçtikten ve yalnızca beş Geri göndermeler vardır veya 30 saniyelik bir aralıkta içinde daha az:

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

Sonra her zamanki gibi eklediğinizden emin olun `ScriptManager` sayfasındaki ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

Denetimleri çoğunu itibaren `NoBot` yaptığını ihtiyacınız bu doğrulama sonucu denetlemek sunucu tarafında oluşur. Bu çağırarak yapılabilir `NoBot`'s `IsValid()` yöntemi. Bir bağımsız değişkene sahip (olarak bir `out` parametre /`ByRef` parametresi) türündeki `NoBotState`. Denetim başarısız olduğunda, dize gösterimi nedeni içerir ve `Valid` Aksi takdirde. Aşağıdaki kod bir ileti göre çıkarır `NoBot`kullanıcının neden:

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

Son olarak, bir form gönderme ve ileti çıktısını almak için bir etiket öğesi gerekir ve tamamladığınızda!

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

Bu komut dosyasını çalıştırın ve JavaScript devre dışı bırakma veya ilk iki saniye içinde formu göndermeden veya yedi defa otuz saniye içinde form gönderme sırasında bir hata iletisi alırsınız. Ancak bu denetim akıllıca kullanmanız, kullanıcılar yalnızca yaklaşık % 90 95 JavaScript etkinleştirilmiş olduğundan, bu nedenle kullanıcı 5-%10 başarısız olur `NoBot`kullanıcının sınayın.


[![Bu hata iletisini tarafından bot olmuş](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)

Bu hata iletisini tarafından bot olmuş ([tam boyutlu görüntüyü görüntülemek için tıklatın](fighting-bots-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](fighting-bots-vb.md)
