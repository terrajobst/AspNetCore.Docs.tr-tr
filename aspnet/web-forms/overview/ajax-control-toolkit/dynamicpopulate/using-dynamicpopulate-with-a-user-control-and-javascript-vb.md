---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Bir kullanıcı denetimi ve JavaScript (VB) ile DynamicPopulate kullanarak | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve sonuçta elde edilen değerin t hedef denetime doldurur...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 715973ef4923e635ec2a860d00d55f13a5f8b2d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873007"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a>Bir kullanıcı denetimi ve JavaScript (VB) ile DynamicPopulate kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)

> ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve çıkan değeri bir sayfa yenileme olmadan sayfasında hedef denetime doldurur. Özel istemci tarafı JavaScript kodu kullanarak popülasyon tetiklemek mümkündür. Ancak, bir kullanıcı denetimi genişletici bulunduğu olduğunda gerçekleştirilecek özellikle dikkat edilmelidir.


## <a name="overview"></a>Genel Bakış

`DynamicPopulate` ASP.NET AJAX Denetim Araç Seti denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve sayfa yenileme olmadan sayfasında hedef denetime sonuç değeri doldurur. Özel istemci tarafı JavaScript kodu kullanarak popülasyon tetiklemek mümkündür. Ancak, bir kullanıcı denetimi genişletici bulunduğu olduğunda gerçekleştirilecek özellikle dikkat edilmelidir.

## <a name="steps"></a>Adımlar

Öncelikle, ASP.NET Web hizmeti çağrılacak yöntemin uygulayan gereksinim `DynamicPopulateExtender` denetim. Web hizmeti yöntemi uygulayan `getDate()` adlı dize türünde bir bağımsız değişken bekler `contextKey`, bu yana `DynamicPopulate` denetim bağlam bilgilerini bir parçasını her web hizmeti çağrısı ile gönderir. Kod aşağıdaki gibidir (dosyaları `DynamicPopulate.vb.asmx`) üç biçimlerden birinde geçerli tarihi alır:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

Sonraki adımda, yeni bir kullanıcı denetimi oluşturma (`.ascx` dosyası), ilk satırı aşağıdaki bildiriminde tarafından başlar:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

A &lt; `label` &gt; öğesi, sunucudan gelen verileri görüntülemek için kullanılır.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

Ayrıca kullanıcı denetimi dosyasında kullanacağız üç radyo düğmeleri, web hizmeti tarafından desteklenen üç olası tarih biçimlerinden birini temsil eden her biri. Kullanıcı radyo düğmeleri birinde tıkladığında tarayıcı şöyle JavaScript kodunu yürütün:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

Bu kod erişen `DynamicPopulateExtender` (garip kimliği hakkında endişe etmeyin henüz, bunu daha sonra ele alınacaktır) ve verilerle dinamik popülasyon tetikler. Geçerli radyo düğmesi bağlamında `this.value` olan değerine başvuruyor `format1`, `format2` veya `format3` tam olarak hangi web yöntemi bekliyor.

Kullanıcı denetimi henüz eksik tek şey `DynamicPopulateExtender` radyo düğmeleri web hizmetine bağlayan denetim.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

Yeniden denetiminde kullanılan garip Kimliğini Not: `mcd1$myDate` yerine `myDate`. Daha önce kullanılan JavaScript kodu `mcd1_dpe1` erişimi `DynamicPopulateExtender` yerine `dpe1`. Bu adlandırma stratejisi özel gereksinim kullanıldığında `DynamicPopulateExtender` bir kullanıcı denetimi içinde. Ayrıca, tüm çalışması için özel bir biçimde kullanıcı öne katıştırmak vardır. Yeni bir ASP.NET sayfası oluşturun ve yalnızca uyguladık kullanıcı denetimi için etiket öneki kaydedin:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

Ardından, ASP.NET AJAX dahil `ScriptManager` yeni sayfasında denetimi:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

Son olarak, kullanıcı denetimi sayfasına ekleyin. Yalnızca ayarlamanız gerekir, `ID` özniteliği (ve `runat="server"`, Elbette), ancak belirli bir ad ayarlamak de: `mcd1` bu içinde kullanıcı denetimini JavaScript kullanarak erişmek için kullanılan önek olduğundan.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

Ve bu kadar! Sayfa beklendiği gibi davranır: kullanıcı radyo düğmeleri birinde tıklarsa, araç seti, denetimi web hizmeti çağrıları ve istenen biçiminde geçerli tarihi görüntüler.


[![Bir kullanıcı denetimi radyo düğmeleri bulunur](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)

Bir kullanıcı denetimi radyo düğmeleri bulunur ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](dynamically-populating-a-control-using-javascript-code-vb.md)
