---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Dinamik olarak JavaScript kodu (C#) kullanarak denetim doldurma | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve sonuçta elde edilen değerin t hedef denetime doldurur...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: c9fd11ea0348eb7fe9a7634f7b26031339146828
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873189"
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a>JavaScript kodu (C#) kullanarak denetim dinamik olarak doldurma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve çıkan değeri bir sayfa yenileme olmadan sayfasında hedef denetime doldurur. Özel istemci tarafı JavaScript kodu kullanarak popülasyon tetiklemek mümkündür.


## <a name="overview"></a>Genel Bakış

`DynamicPopulate` ASP.NET AJAX Denetim Araç Seti denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve sayfa yenileme olmadan sayfasında hedef denetime sonuç değeri doldurur. Özel istemci tarafı JavaScript kodu kullanarak popülasyon tetiklemek mümkündür.

## <a name="steps"></a>Adımlar

Öncelikle, ASP.NET Web hizmeti çağrılacak yöntemin uygulayan gereksinim `DynamicPopulateExtender` denetim. Web hizmeti yöntemi uygulayan `getDate()` adlı dize türünde bir bağımsız değişken bekler `contextKey`, bu yana `DynamicPopulate` denetim bağlam bilgilerini bir parçasını her web hizmeti çağrısı ile gönderir. Kod aşağıdaki gibidir (dosya `DynamicPopulate.cs.asmx`) üç biçimlerden birinde geçerli tarihi alır:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

Sonraki adım, yeni bir ASP.NET sitesi oluşturmak ve ASP.NET AJAX ScriptManager denetimi ile başlatın:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Ardından, bir etiket denetimi ekleyin (örneği için aynı ada sahip bir HTML denetimini kullanarak veya `<asp:Label />` web denetimi), daha sonra gösterir web hizmeti çağrısı sonucu.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Ardından, içeren bir `DynamicPopulateExtender` denetlemek ve web hizmeti bilgileri, hedef denetimi, ancak özel JavaScript kullanarak bu yapılır daha sonra popülasyon tetikler denetim adını girin!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Şimdi için JavaScript bölümü. `$find()` ASP.NET AJAX kitaplığı tarafından tanımlanan işlevi, ASP.NET AJAX Denetim Araç Seti sunucu tarafı nesnelerin başvuru gibi döndürür `DynamicPopulateExtender`. Geçerli dosyasındaki `$find("dpe")` bir başvuru döndürür `DynamicPopulateExtender` sayfasında denetimi. Adlı bir yöntemi gösterir `populate()` dinamik doldurma işlemini tetikler. `populate()` Yöntemi bir bağımsız değişken gerektirir: bağımsız değişken olarak hizmet verecektir içerik anahtarı `getDate()` web yöntemi. Bu nedenle örneğin `$find("dpe").populate("format1")` ay gün yıl biçiminde geçerli tarihi etiketle doldurmak.

Örnek biraz daha esnek hale getirmek için kullanıcı artık çeşitli tarih biçimleri arasında tercih edebilirsiniz. Bunların her biri için bir radyo düğmesi görüntülenir. Bir kez kullanıcı tıklama radyo düğmesine JavaScript kodu seçili tarih biçimiyle etiketi dinamik olarak doldurur. Bu radyo düğmeleri şunlardır:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Radyo düğmesi, JavaScript ifadesi bağlamında unutmayın `this.value` tam olarak aynı bilgiler için geçerli düğmesini değerine başvuruyor `getDate()` yöntemi ile çalışabilir.


[![Düğme tıklama sunucusundan belirtilen biçimde tarihi alır.](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Düğme tıklama sunucusundan belirtilen biçimde tarihi alır ([tam boyutlu görüntüyü görüntülemek için tıklatın](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](dynamically-populating-a-control-cs.md)
> [sonraki](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
