---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: CascadingDropDown (VB) kullanarak bir liste doldurma | Microsoft Docs
author: wenz
description: "Böylece bir DropDownList yükleri değişiklikleri anoth değerleri ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 1c5cb23be4366365c73ce4774e6a53e452e2a6c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a>CascadingDropDown (VB) kullanarak bir liste doldurma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)

> Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. (Örneği için bir liste BİZE durumları bir listesini sağlar ve sonraki liste sonra bu durum önemli şehirlerde ile doldurulur.) Çözmek için ilk testten bu denetimi kullanarak bir açılır liste gerçekte doldurmaktır.


## <a name="overview"></a>Genel Bakış

Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. (Örneği için bir liste BİZE durumları bir listesini sağlar ve sonraki liste sonra bu durum önemli şehirlerde ile doldurulur.) Çözmek için ilk testten bu denetimi kullanarak bir açılır liste gerçekte doldurmaktır.

## <a name="steps"></a>Adımlar

ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

Ardından, bir DropDownList denetimi gereklidir:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

Bu liste için bir CascadingDropDown genişletici eklenir. Ardından listede görüntülenecek girişleri bir listesini dönecek bir web hizmetini zaman uyumsuz bir istek gönderir. Bunun çalışması için aşağıdaki CascadingDropDown öznitelikleri ayarlanması gerekir:

- `ServicePath`: Liste girişlerini sunan bir web hizmeti URL'si
- `ServiceMethod`: Liste girişlerini gönderiliyor web yöntemi
- `TargetControlID`: Aşağı açılan listesinin kimliği
- `Category`: Web yöntemi çağrıldığında gönderildi kategori bilgileri
- `PromptText`: Zaman uyumsuz olarak listesi verileri sunucudan yüklenirken görüntülenen metin

İşaretleme için işte `CascadingDropDown` öğesi. C# ve VB arasındaki tek fark ilişkili web hizmeti adıdır:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

' Ten gelen JavaScript kodu `CascadingDropDown` genişletici aşağıdaki imzalı web hizmeti yöntemi çağırır:

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

Önemli en boy yöntemi türünde bir dizi döndürmesi gerekir, böylece `CascadingDropDownNameValue` (ASP.NET AJAX Denetim Araç Seti tarafından tanımlanmış). İçinde `CascadingDropDownNameValue` contructor, liste girişin metin ve değerini sağlanmalıdır, ilk gibi `<option value="VALUE">NAME</option>` HTML'de yapın. Bazı örnek veriler aşağıdadır:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

Tarayıcıda sayfa yüklenirken üç satıcıları ile doldurulacak liste tetikler.


[![Liste otomatik olarak doldurulur.](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)

Liste otomatik olarak doldurulur ([tam boyutlu görüntüyü görüntülemek için tıklatın](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))

>[!div class="step-by-step"]
[Önceki](using-auto-postback-with-cascadingdropdown-cs.md)
[sonraki](using-cascadingdropdown-with-a-database-vb.md)
