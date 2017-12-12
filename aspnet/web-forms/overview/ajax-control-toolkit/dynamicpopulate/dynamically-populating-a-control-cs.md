---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Bir denetim (C#) dinamik olarak doldurma | Microsoft Docs
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve sonuçta elde edilen değerin t hedef denetime doldurur..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a1868a0e4cec4a95d4175ce255fea2e200692075
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-c"></a>Bir denetim (C#) dinamik olarak doldurma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)

> ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve çıkan değeri bir sayfa yenileme olmadan sayfasında hedef denetime doldurur.


## <a name="overview"></a>Genel Bakış

`DynamicPopulate` ASP.NET AJAX Denetim Araç Seti denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve sayfa yenileme olmadan sayfasında hedef denetime sonuç değeri doldurur. Bu öğreticide bunu nasıl ayarlanacağını gösterir.

## <a name="steps"></a>Adımlar

Öncelikle, ASP.NET Web hizmeti çağrılacak yöntemin uygulayan gereksinim `DynamicPopulate`. Web hizmeti sınıfı gerektirir `ScriptService` içinde tanımlanan özniteliği `Microsoft.Web.Script.Services`; Aksi takdirde ASP.NET AJAX sırayla gerekli web hizmeti için istemci tarafı JavaScript proxy oluşturulamıyor `DynamicPopulate`.

Web yöntemi adı verilen dize türünde bir bağımsız değişken bekler gerekir `contextKey`, bu yana `DynamicPopulate` denetim bağlam bilgilerini bir parçasını her web hizmeti çağrısı ile gönderir. Aşağıdaki web hizmeti tarafından temsil edilen bir biçiminde geçerli tarihi döndürür `contextKey` bağımsız değişkeni:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

Web hizmeti olarak kaydedilir `DynamicPopulate.cs.asmx`. Alternatif olarak, uygulamanız `getDate()` yöntemi ile gerçek ASP.NET sayfa içinde sayfa yöntemi olarak `DynamicPopulate` denetim.

Sonraki adımda, yeni bir ASP.NET dosyası oluşturun. İlk adım dahil etmek için her zaman olduğu gibi `ScriptManager` geçerli sayfada ASP.NET AJAX kitaplığı yüklenemiyor ve Denetim Araç Seti çalışmasını sağlamak için:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

Ardından, bir etiket denetimi ekleyin (örneği için aynı ada sahip bir HTML denetimini kullanarak veya &lt; `asp:Label`  / &gt; web denetimi), daha sonra gösterir web hizmeti çağrısı sonucu.

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

Bir HTML düğmesi (olarak sunucuya geri gönderimin gerektirmeyen bu yana bir HTML denetimini), ardından dinamik popülasyon tetiklemek için kullanılacak:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

Son olarak, ihtiyacımız `DynamicPopulateExtender` kablo işlemleri için denetim. Aşağıdaki öznitelikler ayarlayın (belirgin olanlar dışında `ID` ve `runat` = `"server"`):

- `TargetControlID`web hizmeti çağrısından sonucu bulunacağı yer
- `ServicePath`web hizmeti yoluna (sayfa yöntemi kullanmak istiyorsanız, atla)
- `ServiceMethod`web yöntemi veya sayfa yöntemi adı
- `ContextKey`web hizmetine gönderilmesini bağlam bilgileri
- `PopulateTriggerControlID`web hizmeti çağrısı tetikler öğesi
- `ClearContentsDuringUpdate`web hizmeti çağrısı sırasında hedef öğe boş verilip

Gördüğünüz gibi bazı bilgiler denetimi gerektiriyor, ancak her şeyi yerine koyma oldukça düz ilet. İşaretleme için işte `DynamicPopulateExtender` Geçerli senaryoda denetimi:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

ASP.NET sayfasını tarayıcıda çalışmasına ve düğmeyi tıklatın; ay gün yıl biçiminde geçerli tarihi alır.


[![Düğme tıklama tarihi sunucudan alır.](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)

Düğme tıklama sunucudan tarihi alır ([tam boyutlu görüntüyü görüntülemek için tıklatın](dynamically-populating-a-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[Sonraki](dynamically-populating-a-control-using-javascript-code-cs.md)
