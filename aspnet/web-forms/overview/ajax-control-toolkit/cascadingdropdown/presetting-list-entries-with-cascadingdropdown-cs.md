---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: Liste girişlerini CascadingDropDown (C#) ile önceden ayarlama | Microsoft Docs
author: wenz
description: Böylece bir DropDownList yükleri değişiklikleri anoth değerleri ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: d87da6c19f6dbdff70eff410ba7573c3e26884fb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868834"
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a>Liste girişlerini CascadingDropDown (C#) ile önceden belirleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)

> Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. Biraz kod ile verileri dinamik olarak yüklendikten sonra bir liste öğesinin seçilmiş mümkündür.


## <a name="overview"></a>Genel Bakış

Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. (Örneği için bir liste BİZE durumları bir listesini sağlar ve sonraki liste sonra bu durum önemli şehirlerde ile doldurulur.) Biraz kod ile verileri dinamik olarak yüklendikten sonra bir liste öğesinin seçilmiş mümkündür.

## <a name="steps"></a>Adımlar

ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

Ardından, bir DropDownList denetimi gereklidir:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

Bu liste, web hizmeti URL'si ve yöntem bilgileri sağlayan bir CascadingDropDown genişletici eklenir:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

CascadingDropDown genişletici daha sonra bir web hizmetini aşağıdaki yöntemi imzası ile zaman uyumsuz olarak çağırır:

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

Yöntem CascadingDropDown değer türünde bir dizi döndürür. Tür kurucu ilk liste girişin başlık ve değer Bekliyor (HTML `value` özniteliği). Üçüncü bağımsız değişkeni ayarlanmışsa, true olarak liste öğesi tarayıcıda otomatik olarak seçilir.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

Yükleme sayfasını tarayıcıda açılır listenin üç satıcılarla seçilmiş ikinci bir doldurur.


[![Liste doldurulur ve otomatik olarak seçilmiş](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)

Liste doldurulur ve otomatik olarak seçilmiş ([tam boyutlu görüntüyü görüntülemek için tıklatın](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](using-cascadingdropdown-with-a-database-cs.md)
> [sonraki](using-auto-postback-with-cascadingdropdown-cs.md)
