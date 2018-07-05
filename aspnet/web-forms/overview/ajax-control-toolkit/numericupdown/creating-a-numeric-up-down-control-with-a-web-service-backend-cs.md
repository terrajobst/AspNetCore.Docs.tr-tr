---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Web hizmeti arka ucuna sahip (C#) sayısal yukarı/aşağı denetimi oluşturma | Microsoft Docs
author: wenz
description: Bir onay kutusuna bir değer kullanıcının yapmasına izin vermek yerine bir sayısal yukarı/aşağı denetimi (Windows ve diğer işletim sistemlerinde var olan) daha fazla c kanıtlamak...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: f9f9bd6b565ae4309694df64d24aac2a03238930
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389317"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a>Web hizmeti arka ucuna sahip (C#) sayısal yukarı/aşağı denetimi oluşturma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)

> Bir onay kutusuna bir değer kullanıcının yapmasına izin vermek yerine bir sayısal yukarı/aşağı denetimi (Windows ve diğer işletim sistemlerinde var olan) gibi daha fazla kanıtlamak rahat. Varsayılan olarak, NumericUpDown denetimi her zaman artırır veya bir değer 1 ile azaltır, ancak daha fazla esneklik bir web hizmeti kanıtlar.


## <a name="overview"></a>Genel Bakış

Bir onay kutusuna bir değer kullanıcının yapmasına izin vermek yerine bir sayısal yukarı/aşağı denetimi (Windows ve diğer işletim sistemlerinde var olan) gibi daha fazla kanıtlamak rahat. Varsayılan olarak, `NumericUpDown` denetimi her zaman artırır veya bir değer 1 ile azaltır, ancak daha fazla esneklik bir web hizmeti kanıtlar.

## <a name="steps"></a>Adımlar

ASP.NET AJAX Denetim Araç Seti içeren `NumericUpDown` otomatik olarak iki düğme metin kutusuna ekleyen genişletici: kendi değeri, bunu azaltmak için bir tane artırmak için bir tane. Ancak denetim bir web hizmeti çağrısı (veya sayfa yöntem çağrısının) destekler. Zaman ölçeğini artırıp düğmesine tıklandığında, JavaScript kodu web sunucusuna bağlanır ve bir yöntemi yürütür. Aşağıdaki bir yöntem imzası verilmiştir:

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

`current` Bağımsız değişkendir metin kutusundaki; geçerli değer `tag` özniteliktir özelliği olarak ayarlanabilir ek bağlam verileri `NumericUpDown` genişletici (ancak gerekli değildir).

Bu örnek, sayısal yukarı/aşağı denetimi yalnızca iki powers olan değerlere izin: 1, 2, 4, 8, 16, 32, 64 ve benzeri. Bu nedenle, değeri artırmak kullanıcı istediğinde yürütülen yöntemi çift eski değeri gerekir; başka bir yöntem değeri tarafından iki bölme gerekir. Bu nedenle tam bir web hizmeti şu şekildedir:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

Son olarak, yeni bir ASP.NET sayfası oluşturun. Her zamanki şekilde ihtiyacınız bir `ScriptManager` denetimi, bir `TextBox` denetimi ve bir `NumericUpDownExtender` denetimi. İkincisi için web hizmeti bilgileri sağlamanız gerekir:

- `ServiceDownMethod` Aşağı adını web yöntemini veya sayfa yöntemi
- `ServiceDownPath` yolu aşağı hizmet yöntemi ile web hizmetine; bir sayfa yöntemini kullanıyorsanız atla
- `ServiceUpMethod` Yukarı adını web yöntemini veya sayfa yöntemi
- `ServiceUpPath` yolu yukarı hizmet yöntemi ile web hizmetine; bir sayfa yöntemini kullanıyorsanız atla

Sayfa için tam biçimlendirmesi şöyledir:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

Sayfa çalıştırırsanız, üst düğmesine tıklayın ve daha düşük düğmesine tıkladığınızda yarıya nasıl metin kutusundaki değeri her zaman çiftler dikkat edin.


[![2'in kuvveti olan sayılar görünür](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)

2'in kuvveti olan sayılar görünür ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
