---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: (C#) CascadingDropDown kullanarak liste doldurma | Microsoft Docs
author: wenz
description: Bir DropDownList yükleri değişiklikleri anoth değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 12a4271b2697df8e24fca5f7ff30797b1e4e077a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385427"
---
<a name="filling-a-list-using-cascadingdropdown-c"></a>(C#) CascadingDropDown kullanarak liste doldurma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> Bir DropDownList yükleri değişiklikleri başka bir DropDownList değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. (Örneği için BİZE durumları listesini bir liste sağlar ve sonraki listesi, bu durumda bulunan büyük şehirlerin ile doldurulur.) Çözmek için ilk testten bu denetimi kullanarak bir açılan listedeki gerçekten doldurmaktır.


## <a name="overview"></a>Genel Bakış

Bir DropDownList yükleri değişiklikleri başka bir DropDownList değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. (Örneği için BİZE durumları listesini bir liste sağlar ve sonraki listesi, bu durumda bulunan büyük şehirlerin ile doldurulur.) Çözmek için ilk testten bu denetimi kullanarak bir açılan listedeki gerçekten doldurmaktır.

## <a name="steps"></a>Adımlar

ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Ardından, bir DropDownList denetimi gereklidir:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Bu liste için CascadingDropDown genişletici eklenir. Ardından listede görüntülenecek girişlerinin listesini döndürür bir web hizmetini zaman uyumsuz bir istek gönderir. Bunun işe yaraması için aşağıdaki CascadingDropDown öznitelikleri ayarlanması gerekir:

- `ServicePath`: Liste girişlerini sunan bir web hizmeti URL'si
- `ServiceMethod`: Liste girişlerini sunma web metodu
- `TargetControlID`: Açılan listenin kimliği
- `Category`: Web yöntemi çağrıldığında gönderilen kategori bilgileri
- `PromptText`: Zaman uyumsuz olarak listesi verileri sunucudan yüklenirken görüntülenen metin

İçin biçimlendirme şöyledir `CascadingDropDown` öğesi. C# ve VB arasındaki tek fark ilişkili web hizmeti adıdır:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

Gelen JavaScript kodu `CascadingDropDown` genişletici imzayla bir web hizmeti yöntemi çağırır:

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

Önemli bir yönüdür yöntemi türünde bir dizi döndürmek gerekir, bu nedenle `CascadingDropDownNameValue` (ASP.NET AJAX Denetim Araç Seti tarafından tanımlanır). İçinde `CascadingDropDownNameValue` Oluşturucu, ilk liste girdinin metin ve değerini sağlanmalıdır, gibi `<option value="VALUE">NAME</option>` HTML yapabilirsiniz. Bazı örnek veriler şu şekildedir:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Tarayıcı sayfa yükleme üç satıcıları ile doldurulacak liste tetikler.


[![Liste otomatik olarak doldurulur.](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

Liste otomatik olarak doldurulur ([tam boyutlu görüntüyü görmek için tıklatın](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-cascadingdropdown-with-a-database-cs.md)
