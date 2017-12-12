---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: "Web hizmeti arka uç ile (C#) bir sayısal yukarı/aşağı denetim oluşturma | Microsoft Docs"
author: wenz
description: "Bir onay kutusuna bir değer yazın kullanıcının yapmasına izin vermek yerine bir sayısal yukarı/aşağı (Windows ve diğer işletim sistemleri mevcut) denetimi olarak daha fazla c kanıtlamak..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: 0cce9aa215c2b4480e845326f69cad4679ecf847
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a>Web hizmeti arka uç ile (C#) sayısal yukarı/aşağı denetimi oluşturma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)

> Bir onay kutusuna bir değer yazın kullanıcının yapmasına izin vermek yerine bir sayısal yukarı/aşağı (Windows ve diğer işletim sistemleri mevcut) denetimi olarak daha fazla kanıtlamak rahat. Varsayılan olarak, NumericUpDown denetimi her zaman artırır veya bir değer 1 ile azaltır, ancak daha fazla esneklik bir web hizmeti kanıtlar.


## <a name="overview"></a>Genel Bakış

Bir onay kutusuna bir değer yazın kullanıcının yapmasına izin vermek yerine bir sayısal yukarı/aşağı (Windows ve diğer işletim sistemleri mevcut) denetimi olarak daha fazla kanıtlamak rahat. Varsayılan olarak, `NumericUpDown` denetimi her zaman artırır veya bir değer 1 ile azaltır, ancak daha fazla esneklik bir web hizmeti kanıtlar.

## <a name="steps"></a>Adımlar

ASP.NET AJAX Denetim Araç Seti içeren `NumericUpDown` otomatik olarak bir metin kutusu iki düğme ekleyen genişletici: kendi değeri, bunu azaltmak için bir tane artırmak için bir tane. Ancak denetim bir web hizmeti çağrısı (veya sayfa yöntem çağrısı) destekler. Her yukarı veya aşağı düğmesine tıklandığında, JavaScript kodu web sunucusuna bağlanır ve bir yöntemi yürütür. Yöntem imzası şu olur:

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

`current` Bağımsız değişkeni metin kutusundaki; geçerli değeridir `tag` özniteliktir özelliği olarak ayarlanabilir ek bağlam verileri `NumericUpDown` genişletici (ancak gerekli değildir).

Bu örnek için yukarı/aşağı Denetim sayısal yalnızca iki tabanların olan değerleri izin: 1, 2, 4, 8, 16, 32, 64 ve benzeri. Bu nedenle, kullanıcı değeri artırmak istediğinde yürütülen yöntemi çift eski değer gerekir; başka bir yöntem değer iki tarafından bölmek gerekir. Bu nedenle tam web hizmeti şöyledir:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

Son olarak, yeni bir ASP.NET sayfası oluşturun. Her zamanki gibi ihtiyacınız bir `ScriptManager` denetimi, bir `TextBox` denetim ve `NumericUpDownExtender` denetim. İkincisi, web hizmeti bilgileri sağlamak için gerekenler:

- `ServiceDownMethod`Aşağı adını web yöntemi veya sayfa yöntemi
- `ServiceDownPath`Aşağı hizmet yöntemi web hizmetiyle yolu; bir sayfa yöntemini kullanıyorsanız atlayın
- `ServiceUpMethod`Yukarı adını web yöntemi veya sayfa yöntemi
- `ServiceUpPath`Yukarı hizmet yöntemi web hizmetiyle yolu; bir sayfa yöntemini kullanıyorsanız atlayın

Sayfa için tam biçimlendirme şöyledir:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

Sayfa çalıştırırsanız, üst düğmeyi tıklatın ve daha düşük düğmeyi tıkladığınızda yarıya nasıl metin kutusundaki değeri her zaman iki katına çıkar dikkat edin.


[![2'in olan sayılar görünür](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)

Yalnızca 2'in sayılar görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))

>[!div class="step-by-step"]
[Sonraki](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
