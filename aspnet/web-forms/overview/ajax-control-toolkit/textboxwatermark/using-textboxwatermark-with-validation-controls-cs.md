---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Doğrulama denetimleri ile (C#) TextBoxWatermark kullanma | Microsoft Docs
author: wenz
description: Bir metin kutusu içinde gösterilir böylece AJAX Denetim Araç Seti TextBoxWatermark denetiminde metin kutusunu genişletir. Bir kullanıcı kutusuna tıkladığında, ben...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: fe28f169e5e714ba07d8a71bb33d0f62608d688b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380697"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>Doğrulama denetimleri ile (C#) TextBoxWatermark kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> Bir metin kutusu içinde gösterilir böylece AJAX Denetim Araç Seti TextBoxWatermark denetiminde metin kutusunu genişletir. Kullanıcı kutusuna tıkladığında boşaltılır. Kullanıcının metin girmeden kutusu ayrılırsa doldurulmuş metni görüntülenir. Bu aynı sayfa üzerinde ASP.NET doğrulama denetimleri ile çakışabilir, ancak bu sorunları gidermek.


## <a name="overview"></a>Genel Bakış

`TextBoxWatermark` AJAX Denetim Araç Seti denetiminde metin kutusuna bir metin kutusu içinde gösterilir böylece genişletir. Kullanıcı kutusuna tıkladığında boşaltılır. Kullanıcının metin girmeden kutusu ayrılırsa doldurulmuş metni görüntülenir. Bu aynı sayfa üzerinde ASP.NET doğrulama denetimleri ile çakışabilir, ancak bu sorunları gidermek.

## <a name="steps"></a>Adımlar

Temel kurulum örnek aşağıda verilmiştir: bir `TextBox` denetimi Filigran kullanarak bir `TextBoxWatermarkExtender` denetimi. Bir düğme, bir geri gönderme tetikler ve daha sonra sayfasında doğrulama denetimlerini tetiklemek için kullanılacaktır. Ayrıca, bir `ScriptManager` denetimi, ASP.NET AJAX başlatmak için gereklidir:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Şimdi ekleyin bir `RequiredFieldValidator` olup metin alanı form gönderildiğinde denetleyen denetimi. `InitialValue` Doğrulayıcı özelliğini ayarlayın, kullanılan aynı değere `TextBoxWatermarkExtender` denetimi: form gönderildiğinde, değişmeyen bir metin kutusu içindeki eşik değerini değeridir:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

Ancak bir sorun var. Bu yaklaşım: JavaScript istemci devre dışı bırakır, metin alanı filigran metni ile bu nedenle doldurulmuş değil `RequiredFieldValidator` bir hata iletisi tetiklemez. Bu nedenle, ikinci `RequiredFieldValidator` denetimi gereklidir, bir boş metin kutusunu denetler (atlama `InitialValue` özniteliği).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Her iki doğrulayıcıları kullandığından `Display` = `"Dynamic"`, son kullanıcının hangi iki doğrulayıcıları harekete geçirildi görsel görünümünü ayırt edemez; bunun yerine, olduğu gibi yalnızca bunlardan birinin arar.

Son olarak, bir hata iletisi Doğrulayıcı verilen, alandaki metin çıktısını almak için bazı sunucu tarafı kodu ekleyin:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![Doğrulayıcı alanı metin olduğunu şeklinde hata verir.](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

Doğrulayıcı alanı metin olduğunu şeklinde hata verir ([tam boyutlu görüntüyü görmek için tıklatın](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](using-textboxwatermark-in-a-formview-cs.md)
> [İleri](using-textboxwatermark-in-a-formview-vb.md)
