---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: ASP.NET Web sayfaları (Razor) yan yana farklı sürümlerini çalıştıran | Microsoft Docs
author: tfitzmac
description: Bu makalede, Web siteleri farklı sürümlerini kullanmak üzere yapılandırıldığında ASP.NET Web sayfaları (Razor) Web sitelerine aynı bilgisayara veya sunucuya nasıl çalıştırılacağı açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 1729f3201013926b221afc92d23416b0081d8efb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>ASP.NET Web sayfaları (Razor) farklı sürümlerini yan yana çalıştırma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, Web siteleri farklı sürümler, ASP.NET Web sayfaları kullanmak üzere yapılandırıldığında ASP.NET Web sayfaları (Razor) Web sitelerine aynı bilgisayara veya sunucuya nasıl çalıştırılacağı açıklanmaktadır.
> 
> Öğrenecekleriniz:
> 
> - ASP.NET Web Pages ile oluşturulmuş sitelerin olduğunda ne varsayılan ASP.NET davranıştır.
> - Yeni bir site, ASP.NET Web sayfaları daha eski bir sürümü ile çalışacak şekilde yapılandırmak nasıl.
>   
> 
> Bu makalede sunulan ASP.NET özelliğidir:
> 
> - `webPages:Version` Yapılandırma ayarı.
>   
> 
> ## <a name="software-versions"></a>Yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ve ASP.NET Web sayfaları 1.0 ile de çalışır.


ASP.NET Web sayfaları Web siteleri yan yana çalıştırma yeteneğini destekler. Bu, eski ASP.NET Web sayfaları uygulamalarınızı çalıştırmak, yeni ASP.NET Web Pages uygulamaları geliştirmek ve bunların tümünün aynı bilgisayarda çalışan devam sağlar.

WebMatrix ile Web sayfaları yüklediğinizde unutmamanız gereken bazı noktalar şunlardır:

- Varsayılan olarak, bilgisayarınızda en son sürümü olarak mevcut Web Pages uygulamaları çalıştırın. (Derlemeleri genel derleme önbelleğinde (GAC) yüklü olduğundan ve otomatik olarak kullanılır.)
- ASP.NET Web sayfaları, farklı bir sürümünü kullanarak bir site çalıştırmak istiyorsanız, bunu yapmak için site yapılandırabilirsiniz. Sitenizi zaten yoksa bir *web.config* dosya sitesinin kök dizininde, yeni bir tane oluşturun ve aşağıdaki XML dosyasını buraya var olan içeriğin üzerine kopyalayın. Site zaten varsa bir *web.config* dosya, ekleme bir `<appSettings>` öğesi için aşağıdakine benzer `<configuration>` bölümü.

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  '-'Sürümünde belirtmezseniz *web.config* dosya, bir site en son sürümü olarak dağıtılır. (Derlemeler kopyalanır *bin* sitesi dağıtıldı klasöründe.)
- Web Matrix site şablonları kullanarak oluşturduğunuz yeni uygulamalar dahil Web sayfaları sürüm derlemeleri sitenin içinde *bin* klasör.

Genel olarak, her zaman uygun derlemelerini sitenin yüklemek için NuGet kullanarak sitenizle kullanmak için Web sayfalarının hangi sürümü kontrol edebilirsiniz *bin* klasör. Paketler için ziyaret edin [NuGet.org](http://NuGet.org).

## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET Web Pages 2 üst özellikleri](top-features-in-web-pages-2.md)
