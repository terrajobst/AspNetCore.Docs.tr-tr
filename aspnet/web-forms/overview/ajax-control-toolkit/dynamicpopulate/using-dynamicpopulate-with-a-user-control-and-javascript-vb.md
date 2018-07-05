---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Kullanıcı denetimi ve JavaScript (VB) ile dynamicpopulate kullanma | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetimi web hizmetini (veya sayfa yöntemi) çağırır ve t hedef denetime sonuç değerini doldurur...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 6ab979347a3f412e3225a58a133ae63fcae0a11a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365600"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a>Kullanıcı denetimi ve JavaScript (VB) ile dynamicpopulate kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)

> ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetimi, bir web hizmeti (veya sayfa yöntemi) çağırır ve bir hedef denetimine sayfasında, bir sayfa yenileme olmadan sonuç değerini doldurur. Özel istemci tarafı JavaScript kodu kullanarak popülasyon tetiklemek mümkündür. Ancak özel dikkat genişletici bir kullanıcı denetiminde bulunduğunda alınması gerekir.


## <a name="overview"></a>Genel Bakış

`DynamicPopulate` ASP.NET AJAX Denetim Araç Seti denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve bir hedef denetimine sayfasında, bir sayfa yenileme olmadan sonuç değerini doldurur. Özel istemci tarafı JavaScript kodu kullanarak popülasyon tetiklemek mümkündür. Ancak özel dikkat genişletici bir kullanıcı denetiminde bulunduğunda alınması gerekir.

## <a name="steps"></a>Adımlar

İlk olarak bir ASP.NET Web hizmeti çağrılacak yöntemin uygulayan ihtiyacınız `DynamicPopulateExtender` denetimi. Web hizmeti yöntemini uygulayan `getDate()` adlı dize türündeki bir bağımsız değişken bekliyor `contextKey`, bu yana `DynamicPopulate` denetim bağlam bilgilerini tek bir parçası olan her web hizmeti çağrısı gönderir. Kodu (dosyaları `DynamicPopulate.vb.asmx`) geçerli tarihi kullanılan üç biçimlerden birini alır:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

Sonraki adımda, yeni bir kullanıcı denetimi oluşturma (`.ascx` dosyası), ilk satırında, aşağıdaki bildirimi tarafından başlar:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

A &lt; `label` &gt; öğesi, sunucudan gelen verileri görüntülemek için kullanılır.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

Ayrıca kullanıcı denetimi dosyasında kullanacağız üç radyo düğmeleri, web hizmeti tarafından desteklenen üç olası tarih biçimlerinden birini temsil eden her biri. Kullanıcı için bir radyo düğmeleri tıkladığında tarayıcı, şuna benzer bir JavaScript kodu yürütür:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

Bu kod erişen `DynamicPopulateExtender` (garip kimliği hakkında endişe duymamanızı henüz, bunu daha sonra ele alınacak) ve dinamik nüfus verileri tetikler. Geçerli bir radyo düğmesi bağlamında `this.value` olan değerine başvuran `format1`, `format2` veya `format3` tam olarak hangi web yöntemini bekliyor.

Kullanıcı denetiminde henüz eksik gereken tek şey `DynamicPopulateExtender` radyo düğmeleri web hizmetine bağlayan denetimi.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

Denetimde kullanılan garip kimliği yeniden fark edebilir: `mcd1$myDate` yerine `myDate`. Daha önce kullanılan JavaScript kodu `mcd1_dpe1` erişimi `DynamicPopulateExtender` yerine `dpe1`. Bu adlandırma stratejisi kullanılırken özel bir gereksinim olan `DynamicPopulateExtender` içinde bir kullanıcı denetimi. Ayrıca, tüm çalışması için belirli bir şekilde kullanıcı öne katıştırmak vardır. Yeni bir ASP.NET sayfası oluşturmak ve bir etiket öneki yalnızca uyguladıysanız kullanıcı denetimi kaydedin:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

Ardından, ASP.NET AJAX dahil `ScriptManager` yeni sayfada denetimi:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

Son olarak, kullanıcı denetimi sayfasına ekleyin. Yalnızca ayarlamak zorunda kendi `ID` özniteliği (ve `runat="server"`, Elbette), ancak belirli bir adına ayarlamak de: `mcd1` olduğundan bu kullanıcı denetimi içinde JavaScript kullanarak erişmek için kullanılan önek.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

Ve İşte bu kadar! Sayfa beklendiği gibi davranır: bir kullanıcı radyo düğmelerinden birini tıklarsa, araç seti denetimi web hizmetini çağıran ve istenen biçiminde geçerli tarihi görüntüler.


[![Radyo düğmelerinin kullanıcı denetiminde yer alır.](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)

Radyo düğmelerinin kullanıcı denetiminde bulunan ([tam boyutlu görüntüyü görmek için tıklatın](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](dynamically-populating-a-control-using-javascript-code-vb.md)
