---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: "Bir Web hizmeti arka (VB) ile bir sayısal yukarı/aşağı denetim oluşturma | Microsoft Docs"
author: wenz
description: "Bir onay kutusuna bir değer yazın kullanıcının yapmasına izin vermek yerine bir sayısal yukarı/aşağı (Windows ve diğer işletim sistemleri mevcut) denetimi olarak daha fazla c kanıtlamak..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ceefd6c18761c2abe3f3a4298d340642a0951d6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>Bir Web hizmeti arka (VB) ile bir sayısal yukarı/aşağı denetimi oluşturma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> Bir onay kutusuna bir değer yazın kullanıcının yapmasına izin vermek yerine bir sayısal yukarı/aşağı (Windows ve diğer işletim sistemleri mevcut) denetimi olarak daha fazla kanıtlamak rahat. Varsayılan olarak, NumericUpDown denetimi her zaman artırır veya bir değer 1 ile azaltır, ancak daha fazla esneklik bir web hizmeti kanıtlar.


## <a name="overview"></a>Genel Bakış

Bir onay kutusuna bir değer yazın kullanıcının yapmasına izin vermek yerine bir sayısal yukarı/aşağı (Windows ve diğer işletim sistemleri mevcut) denetimi olarak daha fazla kanıtlamak rahat. Varsayılan olarak, `NumericUpDown` denetimi her zaman artırır veya bir değer 1 ile azaltır, ancak daha fazla esneklik bir web hizmeti kanıtlar.

## <a name="steps"></a>Adımlar

ASP.NET AJAX Denetim Araç Seti içeren `NumericUpDown` otomatik olarak bir metin kutusu iki düğme ekleyen genişletici: kendi değeri, bunu azaltmak için bir tane artırmak için bir tane. Ancak denetim bir web hizmeti çağrısı (veya sayfa yöntem çağrısı) destekler. Her yukarı veya aşağı düğmesine tıklandığında, JavaScript kodu web sunucusuna bağlanır ve bir yöntemi yürütür. Yöntem imzası şu olur:

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

`current` Bağımsız değişkeni metin kutusundaki; geçerli değeridir `tag` özniteliktir özelliği olarak ayarlanabilir ek bağlam verileri `NumericUpDown` genişletici (ancak gerekli değildir).

Bu örnek için yukarı/aşağı Denetim sayısal yalnızca iki tabanların olan değerleri izin: 1, 2, 4, 8, 16, 32, 64 ve benzeri. Bu nedenle, kullanıcı değeri artırmak istediğinde yürütülen yöntemi çift eski değer gerekir; başka bir yöntem değer iki tarafından bölmek gerekir. Bu nedenle tam web hizmeti şöyledir:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

Son olarak, yeni bir ASP.NET sayfası oluşturun. Her zamanki gibi ihtiyacınız bir `ScriptManager` denetimi, bir `TextBox` denetim ve `NumericUpDownExtender` denetim. İkincisi, web hizmeti bilgileri sağlamak için gerekenler:

- `ServiceDownMethod`Aşağı adını web yöntemi veya sayfa yöntemi
- `ServiceDownPath`Aşağı hizmet yöntemi web hizmetiyle yolu; bir sayfa yöntemini kullanıyorsanız atlayın
- `ServiceUpMethod`Yukarı adını web yöntemi veya sayfa yöntemi
- `ServiceUpPath`Yukarı hizmet yöntemi web hizmetiyle yolu; bir sayfa yöntemini kullanıyorsanız atlayın

Sayfa için tam biçimlendirme şöyledir:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

Sayfa çalıştırırsanız, üst düğmeyi tıklatın ve daha düşük düğmeyi tıkladığınızda yarıya nasıl metin kutusundaki değeri her zaman iki katına çıkar dikkat edin.


[![2'in olan sayılar görünür](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

Yalnızca 2'in sayılar görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

>[!div class="step-by-step"]
[Önceki](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
