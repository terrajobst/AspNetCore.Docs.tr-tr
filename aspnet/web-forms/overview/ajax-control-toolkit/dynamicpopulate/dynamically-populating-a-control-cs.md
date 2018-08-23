---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Bir denetimi (C#) dinamik olarak doldurma | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetimi web hizmetini (veya sayfa yöntemi) çağırır ve t hedef denetime sonuç değerini doldurur...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 1f37a3a41c7b83738f97c0daacd781c52b55abc8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755921"
---
<a name="dynamically-populating-a-control-c"></a>Bir denetimi (C#) dinamik olarak doldurma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)

> ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetimi, bir web hizmeti (veya sayfa yöntemi) çağırır ve bir hedef denetimine sayfasında, bir sayfa yenileme olmadan sonuç değerini doldurur.


## <a name="overview"></a>Genel Bakış

`DynamicPopulate` ASP.NET AJAX Denetim Araç Seti denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve bir hedef denetimine sayfasında, bir sayfa yenileme olmadan sonuç değerini doldurur. Bu öğretici, bu ayarlama işlemi gösterilmektedir.

## <a name="steps"></a>Adımlar

İlk olarak bir ASP.NET Web hizmeti çağrılacak yöntemin uygulayan ihtiyacınız `DynamicPopulate`. Web hizmeti sınıfı gerektirir `ScriptService` içinde tanımlanan öznitelik `Microsoft.Web.Script.Services`; Aksi takdirde, ASP.NET AJAX sırayla tarafından gerekli olan web hizmeti için istemci tarafı JavaScript proxy'si oluşturulamıyor `DynamicPopulate`.

Web yöntemi adı verilen dize türünde bir bağımsız değişken bekler gerekir `contextKey`, bu yana `DynamicPopulate` denetim bağlam bilgilerini tek bir parçası olan her web hizmeti çağrısı gönderir. Aşağıdaki web hizmeti tarafından temsil edilen bir biçiminde geçerli tarihi döndürür `contextKey` bağımsız değişkeni:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

Web hizmeti olarak kaydedilir `DynamicPopulate.cs.asmx`. Alternatif olarak, uygulayabileceğine `getDate()` yöntemi ile gerçek ASP.NET sayfası içinde bir sayfa yöntemi olarak `DynamicPopulate` denetimi.

Sonraki adımda, yeni bir ASP.NET dosyası oluşturun. Her zaman ilk adımı eklemek için olduğundan `ScriptManager` geçerli sayfadaki ASP.NET AJAX Kitaplığı'nı yüklemek için ve Denetim Araç Seti çalışmasını sağlamak için:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

Ardından, bir etiket denetimi ekleyin (örneğin aynı ada sahip bir HTML denetimini kullanarak veya &lt; `asp:Label`  / &gt; web denetimi), daha sonra gösterir web hizmeti çağrısı sonucunu.

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

Bir HTML düğmesi (olarak beri bir geri gönderme sunucuya gerektirmeyen bir HTML denetimini), ardından dinamik popülasyon tetiklemek için kullanılır:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

Son olarak, ihtiyacımız `DynamicPopulateExtender` kablo işlemleri için denetim. Aşağıdaki öznitelikler ayarlayın (belirgin olanlar dışında `ID` ve `runat` = `"server"`):

- `TargetControlID` web hizmeti çağrısından sonucu yerleştirileceği yeri
- `ServicePath` web hizmetinin yolunu (sayfası yöntemi kullanmak istiyorsanız atla)
- `ServiceMethod` web yöntemi veya sayfası yöntemi adı
- `ContextKey` web hizmetine gönderilmesini bağlam bilgileri.
- `PopulateTriggerControlID` web hizmeti çağrısı tetikleyen öğesi
- `ClearContentsDuringUpdate` web hizmeti çağrısı sırasında hedef öğe boş verilip verilmeyeceğini

Gördüğünüz gibi denetimi bazı bilgiler gerektirir ancak her şeyi yere koymak oldukça rahatça. İçin biçimlendirme şöyledir `DynamicPopulateExtender` denetimi Geçerli senaryoda:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

ASP.NET sayfasını tarayıcıda çalıştırın ve düğmesine tıklayın. ay gün yıl biçiminde geçerli tarihi alır.


[![Düğmenin click sunucudan tarihi alır.](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)

Düğmenin click sunucudan tarihi alır ([tam boyutlu görüntüyü görmek için tıklatın](dynamically-populating-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](dynamically-populating-a-control-using-javascript-code-cs.md)
