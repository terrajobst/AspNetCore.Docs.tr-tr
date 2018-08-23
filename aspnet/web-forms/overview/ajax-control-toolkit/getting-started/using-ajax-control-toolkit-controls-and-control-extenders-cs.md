---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: AJAX Denetim Araç Seti denetimlerini ve denetim Genişleticilerini (C#) kullanma | Microsoft Docs
author: microsoft
description: AJAX Denetim Araç Seti denetimlerini ve genişleticilerini ASP.NET sayfalarınıza eklemeyi öğrenin.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 18ee6dd71fe0e84ec7628eba63aabeee0690d0b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756230"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a>AJAX Denetim Araç Seti denetimlerini ve denetim Genişleticilerini (C#) kullanma
====================
tarafından [Microsoft](https://github.com/microsoft)

> AJAX Denetim Araç Seti denetimlerini ve genişleticilerini ASP.NET sayfalarınıza eklemeyi öğrenin.


AJAX Denetim Araç Seti denetimlerini ve denetim genişleticilerini kümesi içerir. Bu kısa öğreticide bir ASP.NET sayfasına hem denetimlerini ve denetim genişleticilerini ekleme konusunda bilgi edinin.

> [!NOTE] 
> 
> AJAX Denetim Araç Seti yükleme ve AJAX Denetim Araç Seti Visual Studio/Visual Web Developer araç kutusuna ekleme yönergeleri görmek için öğreticiyi [AJAX Denetim Araç Seti ile çalışmaya başlama](get-started-with-the-ajax-control-toolkit-cs.md).


## <a name="using-ajax-control-toolkit-controls"></a>AJAX Denetim Araç Seti denetimlerini kullanma

AJAX Denetim Araç Seti denetim, yalnızca normal ASP.NET denetim gibi çalışır. Bir ASP.NET sayfasının araç kutusundan denetimi sürükleyin. Tasarım görünümü veya kaynak görünümü sayfası denetimi ekleyebilirsiniz.

AJAX Denetim Araç Seti denetimlerini kullanarak özel bir gereksinimi yoktur. Sayfanın bir ScriptManager denetimi içermesi gerekir. ScriptManager denetimi için tüm gerekli JavaScript AJAX Denetim Araç Seti denetimlerini tarafından gerekli birlikte sorumludur.

Örneğin, AJAX Denetim Araç Seti sekmesi düzenleyici denetimi adlı bir denetimi içerir. Bu denetimi bir zengin HTML Düzenleyicisi'ni görüntüler. Düzenleyici denetimi bir sayfasına eklemek için aşağıdaki adımları izleyin:

1. ShowEditor.aspx adlı yeni bir ASP.NET sayfası oluşturma
2. Araç kutusunda AJAX uzantılar sekmesinde from beneath ScriptManager denetimini seçin ve denetim sayfaya sürükleyin.
3. Araç kutusunda düzenleyici denetimi from beneath AJAX Denetim Araç Seti sekmesini seçin ve denetim sayfaya sürükleyin (bkz. Şekil 1). Tasarımcı, Şekil 2'gibi görünmelidir.
4. Menü seçeneğini belirleyerek web sitesi çalıştırmak **hata ayıklama, hata ayıklamayı Başlat** veya F5 tuşuna basarak.
5. Şekil 3'te sayfası görmeniz gerekir.


[![HTML düzenleyicisi denetimi seçme](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

**Şekil 01**: HTML düzenleyicisi denetimi seçme ([tam boyutlu görüntüyü görmek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))


[![ScriptManager ve düzen denetimi ile Visual Studio Tasarımcısı](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

**Şekil 02**: ScriptManager ve düzen denetimi ile Visual Studio Tasarımcısı ([tam boyutlu görüntüyü görmek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))


[![DisplayEditor.aspx sayfası](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

**Şekil 03**: DisplayEditor.aspx sayfa ([tam boyutlu görüntüyü görmek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>AJAX Denetim Araç Seti denetim Genişleticilerini kullanma

AJAX Denetim Araç Seti denetim genişleticilerini de içerir. Adından da anlaşılacağı gibi bir denetim genişletici varolan bir denetimi işlevselliğini genişletir. Örneğin, standart ASP.NET düğme denetimini ConfirmButton denetim genişletici genişletir. Düğmeyi tıklattığınızda düğme onay iletişim kutusu görüntülenir. böylece genişletici düğme denetimi s davranışını değiştirir.

Bir AJAX Denetim Araç Seti denetimi gibi bir denetim genişletici bir ScriptManager denetimi gereklidir. Denetim genişleticilerini sayfasında kullanmaya başlamadan önce bir ScriptManager denetimi bir sayfasına eklemeniz gerekir.

ConfirmButton denetim genişletici kullanmak için aşağıdaki adımları izleyin:

1. ShowConfirmButton.aspx adlı yeni bir ASP.NET sayfası oluşturma
2. Bir ScriptManager denetimi sayfasına AJAX uzantılar sekmesinde from beneath sayfaya, Denetim sürükleyerek ekleyin.
3. Standart bir düğme denetimi, Standart sekmesini from beneath düğme araç kutusunda Tasarımcı yüzeyine sürükleyerek sayfasına ekleyin.
4. Tıklayın **ekleme genişletici** görev seçeneği (bkz: Şekil 4).
5. Genişletici seçin iletişim kutusunda ConfirmButtonExtender seçin (bkz: Şekil 5) ve Tamam düğmesine tıklayın.
6. Tasarımcıda düğme denetimini seçin ve Genişleticileri Button1 genişletin\_ConfirmButtonExtender düğümü Özellikler penceresinde (bkz. Şekil 6). Değer atamak *gerçekten?* ConfirmText özelliğine.
7. Menü seçeneği seçerek çalıştırırsanız **hata ayıklama, hata ayıklamayı Başlat** veya F5 tuşuna basın.


[![Genişletici ekleme görev seçeneği](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

**Şekil 04**: ekleme genişletici görev seçeneği ([tam boyutlu görüntüyü görmek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))


[![ConfirmButton denetim genişletici seçme](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

**Şekil 05**: seçme ConfirmButton denetim genişletici ([tam boyutlu görüntüyü görmek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))


[![ConfirmButton özelliğini ayarlama](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

**Şekil 06**: ConfirmButton özelliği ayarı ([tam boyutlu görüntüyü görmek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))


Sayfa açıldığında bir düğme görürsünüz. Düğmeye tıkladığınızda, Şekil 7'de onay iletişim kutusunda alın.


[![Onay iletişim kutusunda görüntüleme](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

**Şekil 07**: onay iletişim kutusunda görüntüleme ([tam boyutlu görüntüyü görmek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))


Normalde bir denetim genişletici bir sayfaya sürükleyin değil, dikkat edin. Bunun yerine, kullandığınız **ekleme genişletici** görev bir sayfasına eklemiş bir denetim için bir uzatıcı ekleme seçeneği. Ayrıca, Denetim Genişletilmekte olan denetim için özellik sayfası açarak genişletici özelliklerini ayarlamak dikkat edin.

Tek bir ASP.NET denetimi tarafından birden çok denetim genişleticilerini genişletilebilir. Genişletilmekte olan denetim için özellik sayfası tüm denetimle ilişkili denetim genişleticilerini listeler.

> [!div class="step-by-step"]
> [Önceki](get-started-with-the-ajax-control-toolkit-cs.md)
> [İleri](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
