---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: "Bir kullanıcı denetimi ve JavaScript (C#) ile DynamicPopulate kullanarak | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve sonuçta elde edilen değerin t hedef denetime doldurur..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 3f78da3898d44cc2cf1db6623ebcaf6a73ebbf3e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>DynamicPopulate kullanarak bir kullanıcı denetimi ve JavaScript (C#)
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve çıkan değeri bir sayfa yenileme olmadan sayfasında hedef denetime doldurur. Özel istemci tarafı JavaScript kodu kullanarak popülasyon tetiklemek mümkündür. Ancak, bir kullanıcı denetimi genişletici bulunduğu olduğunda gerçekleştirilecek özellikle dikkat edilmelidir.


## <a name="overview"></a>Genel Bakış

`DynamicPopulate` ASP.NET AJAX Denetim Araç Seti denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve sayfa yenileme olmadan sayfasında hedef denetime sonuç değeri doldurur. Özel istemci tarafı JavaScript kodu kullanarak popülasyon tetiklemek mümkündür. Ancak, bir kullanıcı denetimi genişletici bulunduğu olduğunda gerçekleştirilecek özellikle dikkat edilmelidir.

## <a name="steps"></a>Adımlar

Öncelikle, ASP.NET Web hizmeti çağrılacak yöntemin uygulayan gereksinim `DynamicPopulateExtender` denetim. Web hizmeti yöntemi uygulayan `getDate()` adlı dize türünde bir bağımsız değişken bekler `contextKey`, bu yana `DynamicPopulate` denetim bağlam bilgilerini bir parçasını her web hizmeti çağrısı ile gönderir. Kod aşağıdaki gibidir (dosya `DynamicPopulate.cs.asmx`) üç biçimlerden birinde geçerli tarihi alır:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

Sonraki adımda, yeni bir kullanıcı denetimi oluşturma (`.ascx` dosyası), ilk satırı aşağıdaki bildiriminde tarafından başlar:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

A &lt; `label` &gt; öğesi, sunucudan gelen verileri görüntülemek için kullanılır.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

Ayrıca kullanıcı denetimi dosyasında kullanacağız üç radyo düğmeleri, web hizmeti tarafından desteklenen üç olası tarih biçimlerinden birini temsil eden her biri. Kullanıcı radyo düğmeleri birinde tıkladığında tarayıcı şöyle JavaScript kodunu yürütün:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

Bu kod erişen `DynamicPopulateExtender` (garip kimliği hakkında endişe etmeyin henüz, bunu daha sonra ele alınacaktır) ve verilerle dinamik popülasyon tetikler. Geçerli radyo düğmesi bağlamında `this.value` olan değerine başvuruyor `format1`, `format2` veya `format3` tam olarak hangi web yöntemi bekliyor.

Kullanıcı denetimi henüz eksik tek şey `DynamicPopulateExtender` radyo düğmeleri web hizmetine bağlayan denetim.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

Yeniden denetiminde kullanılan garip Kimliğini Not: `mcd1$myDate` yerine `myDate`. Daha önce kullanılan JavaScript kodu `mcd1_dpe1` erişimi `DynamicPopulateExtender` yerine `dpe1`. Bu adlandırma stratejisi özel gereksinim kullanıldığında `DynamicPopulateExtender` bir kullanıcı denetimi içinde. Ayrıca, tüm çalışması için özel bir biçimde kullanıcı öne katıştırmak vardır. Yeni bir ASP.NET sayfası oluşturun ve yalnızca uyguladık kullanıcı denetimi için etiket öneki kaydedin:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

Ardından, ASP.NET AJAX dahil `ScriptManager` yeni sayfasında denetimi:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

Son olarak, kullanıcı denetimi sayfasına ekleyin. Yalnızca ayarlamanız gerekir, `ID` özniteliği (ve `runat="server"`, Elbette), ancak belirli bir ad ayarlamak de: `mcd1` bu içinde kullanıcı denetimini JavaScript kullanarak erişmek için kullanılan önek olduğundan.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

Ve bu kadar! Sayfa beklendiği gibi davranır: bir kullanıcı üzerinde bulunan radyo düğmelerinin tıklarsa, araç seti, denetimi web hizmeti çağrıları ve istenen biçiminde geçerli tarihi görüntüler.


[![Bir kullanıcı denetimi radyo düğmeleri bulunur](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

Bir kullanıcı denetimi radyo düğmeleri bulunur ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

>[!div class="step-by-step"]
[Önceki](dynamically-populating-a-control-using-javascript-code-cs.md)
[sonraki](dynamically-populating-a-control-vb.md)
