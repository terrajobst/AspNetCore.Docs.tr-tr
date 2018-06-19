---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Bir yardımcı yükleme bir ASP.NET Web sayfaları (Razor) Site | Microsoft Docs
author: tfitzmac
description: Bu makalede, bir yardımcı bir ASP.NET Web sayfaları (Razor) Web sitesi yüklemeyi açıklar. Bir yardımcı kod ve başına biçimlendirme içeren yeniden kullanılabilir bir bileşenidir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 766fbb87ae8bcb8917eb8fa7f552c00792242cf6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896789"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitesinde bir yardımcı yükleme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir yardımcı bir ASP.NET Web sayfaları (Razor) Web sitesi yüklemeyi açıklar. A *yardımcı* kod ve sıkıcı veya karmaşık olabilir bir görevi gerçekleştirmek için biçimlendirme içeren yeniden kullanılabilir bir bileşendir.
> 
> Öğrenecekleriniz:
> 
> - Yardımcıyı WebMatrix 3 kullanılarak oluşturulan bir Web sitesinin yükleme.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - WebMatrix 3


## <a name="overview-of-helpers"></a>Yardımcıları genel bakış

Kişiler genellikle web sayfalarında yapmak istediğiniz bazı görevleri çok fazla kod zorunlu veya ek bilgi gerektirir. Veri için bir grafik görüntüleme örnekler; bir Twitter "İzle" düğmesine bir sayfada koyma; gönderen e-posta, Web sitesinden; Kırpma veya görüntüleri yeniden boyutlandırma; PayPal siteniz için kullanma. Bu tür bir şeyler yapın kolaylaştırmak için ASP.NET Web sayfaları kullanmanıza olanak sağlayan *Yardımcıları*. Bileşenleri ve olanak sağlayan bir site için yüklediğiniz tipik yalnızca bir çizgi veya iki Razor kodunun kullanarak görevleri yardımcılardır.

ASP.NET Web sayfaları birkaç Yardımcıları yerleşik olarak sahiptir. Bununla birlikte, birçok Yardımcıları NuGet Paket Yöneticisi'ni kullanarak sağlanan paketlerinde (eklentiler) kullanılabilir. NuGet yüklemek için bir paket seçmenize olanak sağlar ve ardından bunu yükleme tüm ayrıntılarını mvc'deki.

## <a name="installing-a-helper-in-webmatrix-3"></a>WebMatrix 3 yardımcıyı yükleme

1. WebMatrix 3'te tıklatın **NuGet** düğmesi.

    ![WebMatrix, NuGet Galerisi'nin iletişim kutusu](installing-helpers/_static/image1.png)
2. Bu, NuGet Paket Yöneticisi'ni başlatır ve kullanılabilir paketler görüntüler. Arama kutusuna, yüklemek istediğiniz Yardımcısı için bir anahtar sözcüğü girin.

    ![WebMatrix, NuGet Galerisi'nin iletişim kutusu](installing-helpers/_static/image2.png)
3. Paketi seçin ve ardından **yükleme**. Tıklatın **Evet** paket yüklemek ve koşullarını kabul ettiğinizi belirtmek isteyip istemediğiniz sorulduğunda.

     Yardımcıyı yüklediğiniz ilk kez kullanıyorsanız, NuGet klasörleri Web siteniz için Yardımcısı yapar kodu oluşturur.
4. Yardımcıyı kaldırmak için tıklatın **galeri** düğmesini tıklatın, **yüklü** sekmesini tıklatın ve kaldırmak istediğiniz paketi seçin.

## <a name="installing-the-twitter-helper"></a>Twitter Yardımcısı yükleniyor

Twitter API'si en son sürümünü NuGet yükleme Twitter Yardımcısı ile uyumlu değil. Bunun yerine bkz [Twitter Yardımcısı WebMatrix ile](twitter-helper.md) projenizdeki Twitter Yardımcısı ayarlama hakkında bilgi için konu.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


[2 - Programlama temelleri ile tanışın ASP.NET Web sayfaları](../getting-started/introducing-razor-syntax-c.md)

[WebMatrix ile twitter Yardımcısı](twitter-helper.md)
