---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Bir yardımcıyı yükleme bir ASP.NET Web sayfaları (Razor) sitesinde | Microsoft Docs
author: tfitzmac
description: Bu makalede, nasıl bir ASP.NET Web sayfaları (Razor) Web sitesinde bir yardımcı yükleneceği açıklanır. Bir yardımcı kod ve işaretlemede başına içeren yeniden kullanılabilir bir bileşen olan...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 8629d91e1e297244228898e28f70616c7ccf1acf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754731"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitesinde bir yardımcıyı yükleme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, nasıl bir ASP.NET Web sayfaları (Razor) Web sitesinde bir yardımcı yükleneceği açıklanır. A *Yardımcısı* kod ve yorucu bir süreç veya karmaşık olabilir bir görevi gerçekleştirmek için biçimlendirme içeren yeniden kullanılabilir bir bileşendir.
> 
> Öğrenecekleriniz:
> 
> - Nasıl yardımcı WebMatrix 3'ü kullanarak oluşturulan bir Web sitesine yüklenir.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - WebMatrix 3


## <a name="overview-of-helpers"></a>Yardımcıları genel bakış

Kişiler genellikle web sayfalarında yapmak istediğiniz bazı görevler bir sürü kod gerektiren veya ek bilgi gerektirir. Veriler için bir grafik görüntüleme verilebilir; bir Twitter "İzle" düğmesine bir sayfa koyarak; gönderen e-posta, Web sitesinden; Kırpma veya görüntüleri yeniden boyutlandırmayı; PayPal, siteniz için kullanma. Bu tür bir şeyler yapmasını kolaylaştırır, ASP.NET Web Pages kullanmanıza olanak sağlar. *Yardımcıları*. Bileşenleri ve olanak sağlayan bir site için yüklediğiniz tipik bir çizgi veya iki Razor kodunun kullanarak görevleri yardımcılardır.

ASP.NET Web Pages birkaç Yardımcıları yerleşik olarak sahiptir. Ancak, birçok Yardımcıları NuGet Paket Yöneticisi'ni kullanarak sağlanan paketleri (add-INS) kullanılabilir. NuGet paketini yüklemek için seçmenize olanak sağlar ve ardından bu yükleme tüm ayrıntılarını üstlenir.

## <a name="installing-a-helper-in-webmatrix-3"></a>WebMatrix 3 ' bir yardımcıyı yükleme

1. WebMatrix 3'te tıklatın **NuGet** düğmesi.

    ![WebMatrix, NuGet galerisindeki iletişim kutusu](installing-helpers/_static/image1.png)
2. Bu, NuGet Paket Yöneticisi'ni başlatır ve kullanılabilir paketler görüntüler. Arama kutusuna Yardımcısını yüklemek istediğiniz bir anahtar sözcüğü girin.

    ![WebMatrix, NuGet galerisindeki iletişim kutusu](installing-helpers/_static/image2.png)
3. Paketi seçin ve ardından **yükleme**. Tıklayın **Evet** paketi yükleyin ve koşullarını kabul ettiğinizi belirtmek isteyip istemediğiniz sorulduğunda.

     Bu yardımcı yüklediğiniz ilk kez kullanıyorsanız, NuGet klasörleri Web siteniz için yardımcı yapan kod oluşturur.
4. Bir yardımcı kaldırmak için tıklayın **galeri** düğmesini tıklatın, **yüklü** sekmesini ve kaldırmak istediğiniz paketi seçin.

## <a name="installing-the-twitter-helper"></a>Twitter Yardımcısı'nı yükleme

En son sürümünü Twitter API'si NuGet yükleme için Twitter Yardımcısı ile uyumlu değil. Bunun yerine bkz [WebMatrix ile Twitter Yardımcısı](twitter-helper.md) için Twitter Yardımcısı, projenizdeki ayarlama hakkında bilgi için konu.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


[Karşınızda ASP.NET Web sayfaları 2 - Programlama temelleri](../getting-started/introducing-razor-syntax-c.md)

[WebMatrix ile twitter Yardımcısı](twitter-helper.md)
