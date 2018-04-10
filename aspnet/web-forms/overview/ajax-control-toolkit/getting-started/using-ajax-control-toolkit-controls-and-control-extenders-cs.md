---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: AJAX Denetim Araç Seti denetimleri ve denetim Extender'larının (C#) kullanarak | Microsoft Docs
author: microsoft
description: AJAX Denetim Araç Seti denetimleri ve Extender'larının, ASP.NET sayfaları eklemeyi öğrenin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 3d7cea2452db01ca116849ffb17631db3b935668
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a>AJAX Denetim Araç Seti denetimleri ve denetim Extender'larının (C#) kullanarak
====================
tarafından [Microsoft](https://github.com/microsoft)

> AJAX Denetim Araç Seti denetimleri ve Extender'larının, ASP.NET sayfaları eklemeyi öğrenin.


AJAX Denetim Araç Seti denetimler ve denetim Extender'larının kümesi içerir. Kısa Bu öğreticide, bir ASP.NET sayfasına denetimler ve denetim Extender'larının ekleme öğrenin.

> [!NOTE] 
> 
> AJAX Denetim Araç Seti yükleme ve AJAX Denetim Araç Seti Visual Studio/Visual Web Developer araç kutusuna ekleme hakkında yönergeler için bkz öğretici [AJAX Denetim Araç Seti ile çalışmaya başlama](get-started-with-the-ajax-control-toolkit-cs.md).


## <a name="using-ajax-control-toolkit-controls"></a>AJAX Denetim Araç Seti denetimlerini kullanma

AJAX Denetim Araç Seti denetimi yalnızca normal ASP.NET denetim gibi çalışır. Bir ASP.NET sayfasının Kutusu'ndan denetimi sürükleyin. Tasarım görünümü veya kaynak görünümü sayfası denetimi ekleyebilirsiniz.

AJAX Denetim Araç Seti denetimlerden kullanırken özel gereksinim yoktur. Sayfada bir ScriptManager denetimi içermesi gerekir. ScriptManager denetimi tüm AJAX Denetim Araç Seti denetimleri tarafından gerekli gerekli JavaScript dahil etmek için sorumludur.

Örneğin, AJAX Denetim Araç Seti sekmesi düzenleyici denetimi adında bir denetimi içerir. Bu denetim zengin HTML düzenleyicisi gösterir. Bir düzenleyici denetimi eklemek için aşağıdaki adımları izleyin:

1. ShowEditor.aspx adlı yeni bir ASP.NET sayfası oluşturun
2. Araç kutusunda AJAX uzantılar sekmesi from beneath ScriptManager denetimi seçin ve denetimin sayfaya sürükleyin.
3. Araç kutusunda düzenleyici denetimi from beneath AJAX Denetim Araç Seti sekmesini seçin ve denetimin sayfaya sürükleyin (bkz: Şekil 1). Tasarımcı Şekil 2'gibi görünmelidir.
4. Menü seçeneğini belirleyerek web sitesini çalıştırmak **hata ayıklama, hata ayıklamayı Başlat** veya F5 tuşuna basmak.
5. Şekil 3'te sayfasını görmeniz gerekir.


[![HTML düzenleyicisi denetim seçme](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

**Şekil 01**: HTML düzenleyicisi denetim seçme ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))


[![ScriptManager ve düzenleme denetimi ile Visual Studio Tasarımcısı](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

**Şekil 02**: ScriptManager ve düzenleme denetimi ile Visual Studio Tasarımcısı ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))


[![DisplayEditor.aspx sayfası](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

**Şekil 03**: DisplayEditor.aspx sayfa ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>AJAX Denetim Araç Seti denetim Extender'larının kullanma

AJAX Denetim Araç Seti ayrıca Denetim Extender'larının içerir. Adından da anlaşılacağı gibi denetim genişletici varolan bir denetim işlevselliğini genişletir. Örneğin, standart ASP.NET düğme denetimi ConfirmButton denetim genişletici genişletir. Genişletici düğme denetim s davranışı değiştirir, böylece düğmesini tıklattığınızda bir onay iletişim kutusu görüntüler.

Bir AJAX Denetim Araç Seti denetimi gibi bir denetim genişletici bir ScriptManager denetimi gereklidir. Denetim Extender'larının sayfasında kullanmaya başlamadan önce bir sayfaya bir ScriptManager denetimi eklemeniz gerekir.

ConfirmButton denetim genişletici kullanmak için aşağıdaki adımları izleyin:

1. ShowConfirmButton.aspx adlı yeni bir ASP.NET sayfası oluşturun
2. Bir ScriptManager denetimi AJAX uzantılar sekmesi from beneath sayfaya denetimi sürükleyerek sayfasına ekleyin.
3. Standart bir düğme denetimi, araç çubuğundaki Tasarımcı yüzeyine Standart sekmesini from beneath düğmesi sürükleyerek sayfasına ekleyin.
4. Tıklatın **eklemek genişletici** görev seçeneği (Şekil 4'e bakın).
5. Genişletici Seç iletişim kutusunda ConfirmButtonExtender seçin (bkz. Şekil 5) ve Tamam düğmesine tıklayın.
6. Düğme denetimi Tasarımcısı'nda seçin ve Button1 Genişleticileri genişletin\_ConfirmButtonExtender düğüm Özellikleri penceresinde (bkz. Şekil 6). Değer atamak *gerçekten?* ConfirmText özelliğine.
7. Menü seçeneğini seçerek sayfayı çalıştırın **hata ayıklama, hata ayıklamayı Başlat** veya F5 tuşuna basın.


[![Genişletici eklemek görev seçeneği](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

**Şekil 04**: ekleme genişletici görev seçeneği ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))


[![ConfirmButton denetim genişletici seçme](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

**Şekil 05**: ConfirmButton denetim genişletici seçme ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))


[![ConfirmButton özelliği ayarlama](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

**Şekil 06**: ConfirmButton özelliği ayarı ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))


Sayfa açıldığında bir düğme görmeniz gerekir. Düğmeye tıkladığınızda, Şekil 7'de onay iletişim kutusu alın.


[![Onay iletişim kutusu görüntüleme](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

**Şekil 07**: onay iletişim kutusu görüntüleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))


Normalde bir denetim genişletici bir sayfaya sürüklediğiniz değil, dikkat edin. Bunun yerine, kullandığınız **eklemek genişletici** görev genişletici bir sayfasına eklemiş bir denetim eklemek için seçeneği. Ayrıca, Denetim genişletilen denetimi için özellik sayfası açarak genişletici özelliklerini ayarlamak dikkat edin.

Tek bir ASP.NET denetim birden çok denetim Extender'larının tarafından genişletilebilir. Özellik sayfasını genişletilen denetimi için tüm denetim Extender'larının denetimle ilişkili listeler.

> [!div class="step-by-step"]
> [Önceki](get-started-with-the-ajax-control-toolkit-cs.md)
> [sonraki](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
