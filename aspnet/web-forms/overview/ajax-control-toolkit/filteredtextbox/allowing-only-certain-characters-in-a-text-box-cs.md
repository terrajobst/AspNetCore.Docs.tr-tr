---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Yalnızca belirli bir metin kutusu (C#) karakter izin verme | Microsoft Docs
author: wenz
description: ASP.NET doğrulama denetimleri, yalnızca belirli karakterler uygulamasında kullanıcı girdisi izin verildiğini emin olabilirsiniz. Ancak bu yazarak kullanıcıların geçersiz hala engellemez...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: d2ffc4b741bd0c7f9c456b6e76017f5350ab6378
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a>Yalnızca belirli bir metin kutusu (C#) karakter izin verme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)

> ASP.NET doğrulama denetimleri, yalnızca belirli karakterler uygulamasında kullanıcı girdisi izin verildiğini emin olabilirsiniz. Ancak bu kullanıcıların geçersiz karakterler yazarak ve form göndermeye devam engellemez.


## <a name="overview"></a>Genel Bakış

ASP.NET doğrulama denetimleri, yalnızca belirli karakterler uygulamasında kullanıcı girdisi izin verildiğini emin olabilirsiniz. Ancak bu kullanıcıların geçersiz karakterler yazarak ve form göndermeye devam engellemez.

## <a name="steps"></a>Adımlar

ASP.NET AJAX Denetim Araç Seti içeren `FilteredTextBox` bir metin kutusu genişleten denetim. Etkinleştirilen sonra yalnızca belirli sayıda karakter alanına girilen.

Bunun çalışması için önce her zamanki gibi ASP.NET AJAX ihtiyacımız `ScriptManager` , ASP.NET AJAX Denetim Araç Seti tarafından da kullanılan JavaScript kitaplıklarını yükler:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

Ardından, bir metin kutusu gerekir:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

Son olarak, `FilteredTextBoxExtender` denetim türü için kullanıcı izin karakter kısıtlama mvc'deki. Öncelikle, ayarlamış `TargetControlID` özniteliğini `ID` , `TextBox` denetim. Ardından, kullanılabilir birini `FilterType` değerler:

- `Custom` Varsayılan; Geçerli karakter listesi sağlamak zorunda
- `LowercaseLetters` yalnızca küçük harfler
- `Numbers` yalnızca rakam
- `UppercaseLetters` yalnızca büyük harfler

Varsa `Custom FilterType` kullanılan `ValidChars` özelliği ayarlanmış ve yazılı karakterlerin listesini sağlar. Bu arada: metin kutusuna metni yapıştırmayı deneyin, tüm geçersiz karakter kaldırılır.

İçin biçimlendirme işte `FilteredTextBoxExtender` yalnızca rakam sağlayan denetimi (Ayrıca mümkün olması gereken bir şey `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

JavaScript etkinse, bir harf girmeyi deneyin ve sayfa Çalıştır çalışmaz; basamak ancak sayfasında görüntülenir. Unutmayın ancak koruma `FilteredTextBox` sağlar madde işareti kanıt değil: varsa JavaScript etkinse, ek doğrulama anlamına gelir, yani ASP kullanmak zorunda şekilde herhangi bir veri metin kutusuna girilen. NET'in doğrulama denetimleri.


[![Yalnızca rakam girilebilir](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)

Yalnızca rakam girilebilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](allowing-only-certain-characters-in-a-text-box-vb.md)
