---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: (VB) CascadingDropDown ile otomatik geri gönderme kullanma | Microsoft Docs
author: wenz
description: Bir DropDownList yükleri değişiklikleri anoth değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 274189184b82734e89a30c9450079d7e07971f3c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378167"
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a>(VB) CascadingDropDown ile otomatik geri gönderme kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)

> Bir DropDownList yükleri değişiklikleri başka bir DropDownList değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. Ancak, ASP CascadingDropDown denetim kullanırken. Zaman uyumsuz olarak listesine veri yükleme bir (gereksiz) geri gönderme kendisini oluşturur sonra NET'in DropDownList denetimin AutoPostBack özelliği çalışmaz. Bu etkiyi miktar JavaScript kodu ile önlenebilir.


## <a name="overview"></a>Genel Bakış

Bir DropDownList yükleri değişiklikleri başka bir DropDownList değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. (Örneği için BİZE durumları listesini bir liste sağlar ve sonraki listesi, bu durumda bulunan büyük şehirlerin ile doldurulur.) Ancak, ASP CascadingDropDown denetim kullanırken. Zaman uyumsuz olarak listesine veri yükleme bir (gereksiz) geri gönderme kendisini oluşturur sonra NET'in DropDownList denetimin AutoPostBack özelliği çalışmaz. Bu etkiyi miktar JavaScript kodu ile önlenebilir.

## <a name="steps"></a>Adımlar

ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde &lt; `form` &gt; öğesi):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

Ardından, bir DropDownList denetimi gereklidir:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

Bu liste için web hizmeti URL'sini ve yöntemi bilgilerini sağlama CascadingDropDown genişletici eklenir:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

CascadingDropDown genişletici daha sonra bir web hizmeti aşağıdaki yöntem imzasını ile zaman uyumsuz olarak çağırır:

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

Yöntem CascadingDropDown değer türünde bir dizi döndürür. Liste girişin açıklamalı alt yazı ve değer türün yapıcı ilk bekliyor (HTML `value` özniteliği).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

Tarayıcı sayfa yükleme üç satıcılarla açılan listede seçilmiş ikinci bir doldurur. Ayrıca, ASP.NET tanımlar `__doPostBack()` JavaScript yöntemi. Sayfası yüklendikten sonra bu JavaScript çağrı yalnızca olup olmadığını öğeleri içinde ancak açılan listesine eklenir. Listede bir öğe varsa, JavaScript kodunu bir zaman aşımı kullanır ve yarım saniye içinde yeniden dener Denetim Araç Seti şu anda bunları yükleniyor.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

Bu şekilde, bir geri gönderme yalnızca listede, aslında öğe ve bir giriş kullanıcının seçtiği yürütülür.


[![Bir liste öğesinin seçerek geri göndermeye neden olur](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)

Bir liste öğesinin seçerek geri göndermeye neden olur ([tam boyutlu görüntüyü görmek için tıklatın](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](presetting-list-entries-with-cascadingdropdown-vb.md)
