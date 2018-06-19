---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: TextBoxWatermark doğrulama denetimleri (VB) ile kullanma | Microsoft Docs
author: wenz
description: Böylece bir metin kutusu içinde görüntülenir AJAX Denetim Araç Seti TextBoxWatermark denetiminde bir metin kutusu genişletir. Bir kullanıcı kutunun içine tıkladığında, ı...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ca1a4af62af1d65525e59d0b7bc47245dd01476
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879237"
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a>TextBoxWatermark doğrulama denetimleri (VB) ile kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)

> Böylece bir metin kutusu içinde görüntülenir AJAX Denetim Araç Seti TextBoxWatermark denetiminde bir metin kutusu genişletir. Bir kullanıcı kutunun içine tıkladığında boşaltılır. Kullanıcı kutunun metin girmeden ayrılırsa, doldurulmuş metin görüntülenir. Bu aynı sayfada ASP.NET doğrulama denetimleri ile çakışabilir, ancak bu sorunlarının üstesinden.


## <a name="overview"></a>Genel Bakış

`TextBoxWatermark` AJAX Denetim Araç Seti denetiminde genişleten bir metin kutusu böylece bir metin kutusu içinde görüntülenir. Bir kullanıcı kutunun içine tıkladığında boşaltılır. Kullanıcı kutunun metin girmeden ayrılırsa, doldurulmuş metin görüntülenir. Bu aynı sayfada ASP.NET doğrulama denetimleri ile çakışabilir, ancak bu sorunlarının üstesinden.

## <a name="steps"></a>Adımlar

Örnek temel kurulumu aşağıdaki gibidir: bir `TextBox` denetim Filigran kullanarak bir `TextBoxWatermarkExtender` denetim. Bir düğme geri gönderimin tetikler ve daha sonra sayfada doğrulama denetimleri tetiklemek için kullanılır. Ayrıca, bir `ScriptManager` denetim, ASP.NET AJAX'ı başlatmak için gereklidir:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

Şimdi ekleyin bir `RequiredFieldValidator` olup olmadığını metin alanı form gönderildiğinde denetleyen denetimi. `InitialValue` Doğrulayıcının özelliğini ayarlayın, kullanılan aynı değere `TextBoxWatermarkExtender` denetimi: form gönderildiğinde değiştirilmemiş bir metin kutusu içindeki filigran değeri değeri:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

Ancak bir sorun var. Bu yaklaşım: JavaScript istemci devre dışı bırakır, metin alanı filigran metni ile bu nedenle doldurulmuş değil `RequiredFieldValidator` bir hata iletisi tetiklemez. Bu nedenle, ikinci `RequiredFieldValidator` denetimi gereklidir, boş bir metin kutusu için denetler (atlama `InitialValue` özniteliği).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

Her iki Doğrulayıcıların kullandığından `Display` = `"Dynamic"`, son kullanıcının hangi iki Doğrulayıcıların tetiklenen görsel görünümünü ayırt edemez; oluştu yalnızca bunlardan birinin bunun yerine, görülüyor.

Son olarak, bir hata iletisi Doğrulayıcı verilen olursa metin alanına çıkış için bazı sunucu tarafı kodu ekleyin:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


[![Doğrulayıcı alanı metin yok complains](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)

Doğrulayıcı alanı metin yok complains ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](using-textboxwatermark-in-a-formview-vb.md)
