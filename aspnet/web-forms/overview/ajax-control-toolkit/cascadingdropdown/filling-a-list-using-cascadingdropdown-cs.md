---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: CascadingDropDown (C#) kullanarak bir liste doldurma | Microsoft Docs
author: wenz
description: "Böylece bir DropDownList yükleri değişiklikleri anoth değerleri ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: e5e0f11a815632aff9e17dc0f783f7eba2753995
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="filling-a-list-using-cascadingdropdown-c"></a>CascadingDropDown (C#) kullanarak bir liste doldurma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. (Örneği için bir liste BİZE durumları bir listesini sağlar ve sonraki liste sonra bu durum önemli şehirlerde ile doldurulur.) Çözmek için ilk testten bu denetimi kullanarak bir açılır liste gerçekte doldurmaktır.


## <a name="overview"></a>Genel Bakış

Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. (Örneği için bir liste BİZE durumları bir listesini sağlar ve sonraki liste sonra bu durum önemli şehirlerde ile doldurulur.) Çözmek için ilk testten bu denetimi kullanarak bir açılır liste gerçekte doldurmaktır.

## <a name="steps"></a>Adımlar

ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Ardından, bir DropDownList denetimi gereklidir:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Bu liste için bir CascadingDropDown genişletici eklenir. Ardından listede görüntülenecek girişleri bir listesini dönecek bir web hizmetini zaman uyumsuz bir istek gönderir. Bunun çalışması için aşağıdaki CascadingDropDown öznitelikleri ayarlanması gerekir:

- `ServicePath`: Liste girişlerini sunan bir web hizmeti URL'si
- `ServiceMethod`: Liste girişlerini gönderiliyor web yöntemi
- `TargetControlID`: Aşağı açılan listesinin kimliği
- `Category`: Web yöntemi çağrıldığında gönderildi kategori bilgileri
- `PromptText`: Zaman uyumsuz olarak listesi verileri sunucudan yüklenirken görüntülenen metin

İşaretleme için işte `CascadingDropDown` öğesi. C# ve VB arasındaki tek fark ilişkili web hizmeti adıdır:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

' Ten gelen JavaScript kodu `CascadingDropDown` genişletici aşağıdaki imzalı web hizmeti yöntemi çağırır:

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

Önemli en boy yöntemi türünde bir dizi döndürmesi gerekir, böylece `CascadingDropDownNameValue` (ASP.NET AJAX Denetim Araç Seti tarafından tanımlanmış). İçinde `CascadingDropDownNameValue` contructor, liste girişin metin ve değerini sağlanmalıdır, ilk gibi `<option value="VALUE">NAME</option>` HTML'de yapın. Bazı örnek veriler aşağıdadır:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Tarayıcıda sayfa yüklenirken üç satıcıları ile doldurulacak liste tetikler.


[![Liste otomatik olarak doldurulur.](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

Liste otomatik olarak doldurulur ([tam boyutlu görüntüyü görüntülemek için tıklatın](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

>[!div class="step-by-step"]
[Sonraki](using-cascadingdropdown-with-a-database-cs.md)
