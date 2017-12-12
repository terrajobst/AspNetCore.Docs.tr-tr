---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: "Otomatik geri gönderme CascadingDropDown ile (C#) kullanarak | Microsoft Docs"
author: wenz
description: "Böylece bir DropDownList yükleri değişiklikleri anoth değerleri ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: cd103283f46223d5158e58227bb53c00c74bc7d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a>Otomatik geri gönderme kullanarak CascadingDropDown ile (C#)
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)

> Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. Ancak CascadingDropDown denetimi, ASP kullanırken. Zaman uyumsuz olarak listeye verileri yüklenirken bir (gereksiz) geri gönderme kendisini oluşturur beri NET'in DropDownList denetimin AutoPostBack özelliği çalışmıyor. Bazı JavaScript kodu ile Bu etkiyi önlenebilir.


## <a name="overview"></a>Genel Bakış

Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. (Örneği için bir liste BİZE durumları bir listesini sağlar ve sonraki liste sonra bu durum önemli şehirlerde ile doldurulur.) Ancak CascadingDropDown denetimi, ASP kullanırken. Zaman uyumsuz olarak listeye verileri yüklenirken bir (gereksiz) geri gönderme kendisini oluşturur beri NET'in DropDownList denetimin AutoPostBack özelliği çalışmıyor. Bazı JavaScript kodu ile Bu etkiyi önlenebilir.

## <a name="steps"></a>Adımlar

ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde &lt; `form` &gt; öğesi):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

Ardından, bir DropDownList denetimi gereklidir:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

Bu liste, web hizmeti URL'si ve yöntem bilgileri sağlayan bir CascadingDropDown genişletici eklenir:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

CascadingDropDown genişletici daha sonra bir web hizmetini aşağıdaki yöntemi imzası ile zaman uyumsuz olarak çağırır:

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

Yöntem CascadingDropDown değer türünde bir dizi döndürür. Tür kurucu ilk liste girişin başlık ve değer Bekliyor (HTML `value` özniteliği).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

Yükleme sayfasını tarayıcıda açılır listenin üç satıcılarla seçilmiş ikinci bir doldurur. Ayrıca, ASP.NET tanımlar `__doPostBack()` JavaScript yöntemi. Sayfa yüklendikten sonra bu JavaScript çağrısı yalnızca yoksa öğe içinde ancak açılır listeye eklenir. Listeden bir öğe varsa, böylece JavaScript kodu zaman aşımı kullanır ve bir yarım saniye içinde yeniden dener Denetim Araç Seti şu anda bunları yüklüyor.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

Bu şekilde geri gönderimin yalnızca listede gerçekte öğeleri yok ve kullanıcı bir giriş seçerse sonra yürütülür.


[![Bir liste öğesinin seçerek geri gönderimin neden olur.](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)

Bir liste öğesinin belirlenmesi geri gönderimin neden olur ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))

>[!div class="step-by-step"]
[Önceki](presetting-list-entries-with-cascadingdropdown-cs.md)
[sonraki](filling-a-list-using-cascadingdropdown-vb.md)
