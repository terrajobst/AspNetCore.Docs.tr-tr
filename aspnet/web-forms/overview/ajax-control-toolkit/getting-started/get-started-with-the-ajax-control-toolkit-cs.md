---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: AJAX Denetim Araç Seti ile (C#) kullanmaya başlama | Microsoft Docs
author: microsoft
description: AJAX Denetim Araç Seti ile çalışmaya başlamak için bilmeniz gereken her şeyi öğrenin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: e6a7a8d45f32a33eaacf3c42b52a02d2ada1aab6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a>AJAX Denetim Araç Seti ile (C#) kullanmaya başlama
====================
tarafından [Microsoft](https://github.com/microsoft)

> AJAX Denetim Araç Seti ile çalışmaya başlamak için bilmeniz gereken her şeyi öğrenin.


AJAX Denetim Araç Seti, ASP.NET uygulamalarında kullanabilirsiniz 30'dan fazla ücretsiz denetimleri içerir. Bu öğreticide, AJAX Denetim Araç Seti indirin ve araç seti denetimleri, Visual Studio/Visual Web Developer Express araç kutusuna ekleme öğrenin.

## <a name="downloading-the-ajax-control-toolkit"></a>AJAX Denetim Araç Seti indirme

[AJAX Denetim Araç Seti](http://devexpress.com/act) açık kaynaklı proje ASP.NET topluluk ve ASP.NET takım üyeleri tarafından geliştirilmiştir. 


[![AJAX Denetim Araç Seti indirme](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Şekil 01**: AJAX Denetim Araç Seti indirme ([tam boyutlu görüntüyü görüntülemek için tıklatın](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))


Dosyayı indirdikten sonra dosyayı engellemesini kaldırmak gerekir. Dosyaya sağ tıklayın, Özellikler'i seçin ve tıklatın **Engellemeyi Kaldır** (bkz: Şekil 2) düğmesine tıklayın.


[![AJAX Denetim Araç Seti ZIP dosyası engellemelerini kaldırma](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Şekil 02**: AJAX Denetim Araç Seti ZIP dosyası engellemelerini kaldırma ([tam boyutlu görüntüyü görüntülemek için tıklatın](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))


Dosya engelini kaldırdıktan sonra dosyanın sıkıştırmasını: dosyaya sağ tıklayın ve seçin **tümünü Ayıkla** menü seçeneği. Şimdi, biz Araç Seti için Visual Studio/Visual Web Developer araç eklemek hazır olursunuz.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>AJAX Denetim Araç Seti araç kutusuna ekleme

AJAX Denetim Araç Seti kullanmak için en kolay yolu, Visual Studio/Visual Web Developer araç kutusu araç seti eklemektir (bkz: Şekil 3). Bunu kullanmak istediğinizde bu şekilde, yalnızca bir araç seti denetimi bir sayfaya sürükleyin.


[![AJAX Denetim Araç Seti araç kutusunda görüntülenir](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Şekil 03**: AJAX Denetim Araç Seti araç kutusunda görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))


İlk olarak, araç için bir AJAX Denetim Araç Seti sekme eklemeniz gerekir. Aşağıdaki adımları izleyin.

1. Dosya, yeni Web sitesi menü seçeneğini seçerek yeni bir ASP.NET Web sitesi oluşturun. Düzenleyicide dosyayı açmak için Default.aspx Solution Explorer penceresinde çift tıklayın.
2. Genel sekmesi altındaki araç kutusunu sağ tıklatın ve menü seçeneğini **sekmesi Ekle** (Şekil 4'e bakın).
3. AJAX Denetim Araç Seti adlı yeni bir sekme girin.


[![Yeni bir sekme ekleme](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Şekil 04**: yeni bir sekme ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))


Ardından, yeni bir sekmeye AJAX Denetim Araç Seti denetimleri eklemeniz gerekir. Aşağıdaki adımları uygulayın:

- AJAX Denetim Araç Seti sekmesini sağ tıklatın ve menü seçeneğini **seçin (bkz. Şekil 5) öğeleri**.
- Burada AJAX Denetim Araç Seti unzipped ve AjaxControlToolkit.dll derlemesi seçin konuma göz atın.


[![Araç kutusuna eklemek için öğeleri seçin](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**Şekil 05**: araç kutusu eklemek için öğeleri seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))


Bu adımları tamamladıktan sonra tüm araç seti denetimler araç kutusunda görüntülenir.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Araç setini yeni bir sürüme yükseltme

Araç Seti eski bir sürümünü kullanan ve taşımak artık ihtiyacınız varsa daha sonraki bir sürümü burada önerilen adımları şunlardır:

- İkili dosyalar - AjaxControlToolkit.dll derleme eski sürümü, Web sitesi Bin klasöründen silin.
- Araç kutusu öğelerini - AJAX Denetim Araç Seti sekmesini silin ve sekme AjaxControlToolkit.dll derleme yeni sürümü ile yeniden oluşturmak için yukarıdaki adımları izleyin.

> [!div class="step-by-step"]
> [Next](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
