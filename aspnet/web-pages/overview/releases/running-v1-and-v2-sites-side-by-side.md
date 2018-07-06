---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: ASP.NET Web sayfaları (Razor) yan yana farklı sürümlerini çalıştıran | Microsoft Docs
author: tfitzmac
description: Bu makalede, Web siteleri farklı sürümler kullanmak için yapılandırıldığında, ASP.NET Web sayfaları (Razor) Web siteleri aynı bilgisayara veya sunucuya çalıştırılması açıklanmaktadır...
ms.author: aspnetcontent
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 86b1d5babac747eb4fa7ba8abde0e6155c8b17fb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810062"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>ASP.NET Web sayfaları (Razor) farklı sürümlerini yan yana çalıştırma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, Web siteleri, ASP.NET Web Pages farklı sürümlerini kullanmak üzere yapılandırıldığında ASP.NET Web sayfaları (Razor) Web siteleri aynı bilgisayara veya sunucuya nasıl çalıştırılacağı açıklanmaktadır.
> 
> Öğrenecekleriniz:
> 
> - ASP.NET Web Pages ile oluşturulmuş sitelerin olduğunda varsayılan davranışı ASP.NET'te nedir?
> - Yeni bir site, ASP.NET Web sayfaları'nın eski bir sürümü ile çalışacak şekilde yapılandırma
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
> Bu öğreticide, ASP.NET Web sayfaları 1.0 ve ASP.NET Web Pages 2 ile de çalışır.


ASP.NET Web Pages Web siteleri yan yana çalıştırılması yeteneğini destekler. Bu, eski ASP.NET Web Pages uygulamalarınızı çalıştırmak için yeni ASP.NET Web Pages uygulamaları oluşturup bunların tümünün aynı bilgisayarda çalıştırma devam sağlar.

WebMatrix ile Web sayfalarını yüklemek unutmayın gereken bazı noktalar şunlardır:

- Varsayılan olarak, bilgisayarınızda en son sürümü olarak mevcut Web Pages uygulamaları çalıştırın. (Derlemeler genel derleme önbelleğinde (GAC) yüklü ve otomatik olarak kullanılır.)
- Farklı bir sürümü, ASP.NET Web Pages kullanılarak bir site çalıştırmak istiyorsanız, bunu yapmak için siteyi yapılandırabilirsiniz. Sitenizi zaten yoksa bir *web.config* dosya sitesinin kök dizininde, yeni bir tane oluşturun ve aşağıdaki XML'i dosyayı, var olan içeriğin üzerine kopyalayın. Site zaten varsa bir *web.config* ekleyin bir `<appSettings>` öğesi için aşağıdakine benzer `<configuration>` bölümü.

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  '-'Sürümünde belirtmezseniz *web.config* dosyası, bir siteyi en son sürümü olarak dağıtılır. (Derlemeler kopyalanır *bin* klasöründe sitesi dağıtıldı.)
- Web Matrix site şablonları kullanarak oluşturduğunuz yeni uygulamalar dahil Web sayfaları sürüm derlemeleri sitenin içinde *bin* klasör.

Genel olarak, her zaman uygun derlemelere sitenin yüklemek için NuGet kullanarak siteniz ile kullanmak için Web sayfaları hangi sürümünü denetleyebilir *bin* klasör. Paketler için ziyaret edin [NuGet.org](http://NuGet.org).

## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET Web sayfaları 2'de en iyi Özellikler](top-features-in-web-pages-2.md)
