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
<a name="testing-the-strength-of-a-password-vb"></a>Bir parola (VB) gücünü test etme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)

> Parolalar, neredeyse her yerden, böylece yavaş kullanıcıları ayırmak kolay olan basit parolalar seçmesini eğilimindedir gereklidir. ASP.NET AJAX Denetim Araç Seti PasswordStrength denetiminde ne kadar iyi bir paroladır kontrol edebilirsiniz.


## <a name="overview"></a>Genel Bakış

Parolalar, neredeyse her yerden, böylece yavaş kullanıcıları ayırmak kolay olan basit parolalar seçmesini eğilimindedir gereklidir. `PasswordStrength` ASP.NET AJAX Denetim Araç Seti denetiminde ne kadar iyi bir paroladır kontrol edebilirsiniz.

## <a name="steps"></a>Adımlar

`PasswordStrength` Denetim metin kutusu genişletir ve parola yeterince iyi olup olmadığını denetler. Bol miktarda öznitelikleri aracılığıyla seçenekleri sunar. bunları yalnızca bazıları şunlardır:

- `MinimumNumericCharacters` Parolada kullanılması gereken sayı karakteri sayısının alt sınırı
- `MinimumSymbolCharacters` Parolada kullanılması gereken en az sayıda simge karakteri (harfler ve sayılar değil)
- `PreferredPasswordLength` Minimum parola uzunluğu
- `RequiresUpperAndLowerCaseCharacters` olup büyük ve küçük harf karakterler kullanmak parola gerekiyor

`StrengthIndicatorType` Metin olarak parola gücünü sunmak nasıl bilgiler sunar (değer `"Text"`) veya bir ilerleme çubuğu tür olarak (değer `"BarIndicator"`). İçinde `DisplayPosition` özniteliği, yapılandırma bilgileri göründüğü. ASP.NET AJAX dahil olmak üzere tam bir örnek, işte `ScriptManager` denetimi `PasswordStrength` denetimi ve tabi ki burada kullanıcı girebilirsiniz parola metin kutusu. Geliştirme sırasında yazmakta olduğunuz görebilmeniz için tanıtım amacıyla, ikinci form bir normal metin alanı ve parola alanı alanıdır.

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

Sayfayı çalıştırın ve hemen yazın: yalnızca küçük harfler, büyük harfler, rakamlar ve semboller girdikten sonra parola olarak kesilemeyen kabul edilir.


[![Parola (oldukça) iyi sunulmuştur](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)

Parola (oldukça) iyi şimdi ([tam boyutlu görüntüyü görüntülemek için tıklatın](testing-the-strength-of-a-password-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](testing-the-strength-of-a-password-cs.md)
