---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: JavaScript kodu (C#) kullanarak bir denetimi dinamik olarak doldurma | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetimi web hizmetini (veya sayfa yöntemi) çağırır ve t hedef denetime sonuç değerini doldurur...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 6999dee71966c4362254d9f601e55fdb75d0b292
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394080"
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a>JavaScript kodu (C#) kullanarak bir denetimi dinamik olarak doldurma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetimi, bir web hizmeti (veya sayfa yöntemi) çağırır ve bir hedef denetimine sayfasında, bir sayfa yenileme olmadan sonuç değerini doldurur. Özel istemci tarafı JavaScript kodu kullanarak popülasyon tetiklemek mümkündür.


## <a name="overview"></a>Genel Bakış

`DynamicPopulate` ASP.NET AJAX Denetim Araç Seti denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve bir hedef denetimine sayfasında, bir sayfa yenileme olmadan sonuç değerini doldurur. Özel istemci tarafı JavaScript kodu kullanarak popülasyon tetiklemek mümkündür.

## <a name="steps"></a>Adımlar

İlk olarak bir ASP.NET Web hizmeti çağrılacak yöntemin uygulayan ihtiyacınız `DynamicPopulateExtender` denetimi. Web hizmeti yöntemini uygulayan `getDate()` adlı dize türündeki bir bağımsız değişken bekliyor `contextKey`, bu yana `DynamicPopulate` denetim bağlam bilgilerini tek bir parçası olan her web hizmeti çağrısı gönderir. Kodu (dosya `DynamicPopulate.cs.asmx`) geçerli tarihi kullanılan üç biçimlerden birini alır:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

Sonraki adımda, yeni bir ASP.NET sitesi oluşturma ve ASP.NET AJAX ScriptManager denetimi ile başlayın:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Ardından, bir etiket denetimi ekleyin (örneğin aynı ada sahip bir HTML denetimini kullanarak veya `<asp:Label />` web denetimi), daha sonra gösterir web hizmeti çağrısı sonucunu.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Ardından, içeren bir `DynamicPopulateExtender` denetleyebilir ve web hizmeti bilgileri, hedef denetime, ancak özel JavaScript kullanarak bu yapılır daha sonra popülasyon tetikler denetim adını değil sağlayın!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Artık JavaScript bölümü. `$find()` İşlevi, ASP.NET AJAX kitaplığı tarafından tanımlanan sunucu tarafı ASP.NET AJAX Denetim Araç Seti nesnelerin başvurusu gibi döndürür `DynamicPopulateExtender`. Geçerli dosyadaki `$find("dpe")` bir başvuru döndürür `DynamicPopulateExtender` sayfasındaki denetimi. Adlı bir yöntem sunan `populate()` dinamik doldurma işlemi tetikler. `populate()` Yöntemi bir bağımsız değişken gerektirir: bağımsız değişken olarak davranacak içerik anahtarı `getDate()` web yöntemini. Örneğin, `$find("dpe").populate("format1")` ay gün yıl biçiminde geçerli tarihi etiketle doldurulması.

Örnek biraz daha esnek hale getirmek için kullanıcı artık çeşitli tarih biçimleri arasında tercih edebilirsiniz. Bunların her biri için bir radyo düğmesi görüntülenir. Bir kez kullanıcı tıklama radyo düğmesine JavaScript kodu seçilen tarih biçimiyle etiketi dinamik olarak doldurur. Bu radyo düğmeleri şunlardır:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Bir radyo düğmesi JavaScript ifadesi bağlamında unutmayın `this.value` tam olarak aynı bilgileri özelleştirmede geçerli düğme değerine başvuran `getDate()` yöntemi ile çalışabilir.


[![Bir düğmeye tıklayın, sunucudan belirtilen biçimde tarihi alır.](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Düğmenin click sunucudan belirtilen biçimde tarihi alır ([tam boyutlu görüntüyü görmek için tıklatın](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](dynamically-populating-a-control-cs.md)
> [İleri](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
