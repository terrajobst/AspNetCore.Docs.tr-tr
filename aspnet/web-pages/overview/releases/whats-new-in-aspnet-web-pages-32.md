---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: ASP.NET Web yenilikler sayfaları 3.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 06/30/2014
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: a55c01c1430b983fbe34654cc2c00de05e941d71
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802257"
---
<a name="whats-new-in-aspnet-web-pages-32"></a>ASP.NET Web sayfaları 3.2 yenilikler
====================
tarafından [Microsoft](https://github.com/microsoft)

Bu konuda, ASP.NET Web sayfaları 3.2, Web sayfaları 3.2.2 yenilikler açıklanır ve [Web Pages 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>ASP.NET Web sayfaları 3.2

Bu yayın, bir hata düzeltmeleri ve yeni bir özellik sunmaktadır.

## <a name="download"></a>İndir

Çalışma zamanı özellikleri, NuGet galerisindeki NuGet paketleri olarak kullanıma sunulur. Tüm çalışma zamanı paketlerini izleyin [Semantic Versioning](http://semver.org/) belirtimi. ASP.NET Web sayfaları 3.2 Paket sürümü var: &ldquo;3.2.0&rdquo;. Yükleme veya güncelleştirme yoluyla bu paketleri [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). Sürüm, karşılık gelen yerelleştirilmiş NuGet paketleri de içerir.

NuGet Paket Yöneticisi konsolu kullanarak için kullanıma sunulan NuGet paketlerini güncelleştirin ya da yükleyin:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Yeni özellik ve hata düzeltmesi

Biz bir hata düzeltildi ve bir alt özellik geliştirme bu sürümde yapılan. Aynı sorgu bulabilirsiniz [burada](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>3.2.2 ASP.NET Web sayfaları

Bu sürüm pay değişiklik yukarı [ASP.NET Web Pages 3.2.1 Beta sürümü](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) büyük razor sayfaları işleme içinde önemli bir performans geliştirmesi sağlar. Bkz:[Codeplex sorunu 585](https://aspnetwebstack.codeplex.com/workitem/585). Bu sürüm, MVC 5.2.2 hizalar paketler artık bu sürümüne bağlı.

Büyük sayfaları işleme üzerinde MSN ekiple çalıştık. Sayfalar üzerinde 80 kilobayt veri işleme, biz büyük nesne yığını üzerindeki nesneler ile sonlanır. Bu etkiyi çözümsüz düzenleri kullanıldığında çarpılmasına.

Sonucu sunucuda ek CPU kullanımı, bellek ve daha uzun daha uzun bekletme süresi duraklatır sırasında [Gen 2 Temizleme](https://msdn.microsoft.com/en-us/library/ms973837.aspx) çöp toplayıcının içinde.

Çözümleme sonuçlarını gösteren bir tablo aşağıdadır bir [perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) bir çalıştırma. Büyük sayfaları oluşturulurken CPU hakkında 68 oranında sabit tutulur. Tablo 2. nesil koleksiyonlar sayısı neredeyse tamamen ortadan ve sonucu daha yüksek istek hızı ve nedeniyle çöp toplama duraklamaları önemli ölçüde bir düşüş olduğunu gösterir.

| **Alan** | **(3.2 önce)** | **(3.2.1 sonra)** | **Delta %** |
| --- | --- | --- | --- |
| Toplam istek (sayı) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| İzleme süresi (saniye) | 196.20 | 198.60 | 1.20% |
| İstek/saniye | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| CPU yükü | 68.80% | 68.50% |  -0.40% |
| GC CPU örnekleri | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Toplam Miktar (sayı) | 55,357,146 | 57,222,949 | 3.40% |
| Toplam GC Duraklat (örnekler) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Gen0 GC (sayı) | 403 | 1,216 | 201.70% |
| Gen1 GC (sayı) | 290 | 367 | 26.60% |
| Gen2 GC (sayı) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| CPU / isteği (samples/req) | 19.73 | 16.47 | -16.50% |

| Renk kodlaması: | <font style="background-color: #00ff00">Çekirdek geliştirme</font> | <font style="background-color: #4bacc6">Pozitif bir performans etkisi</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web Pages 3.2.3 beta1

Bu sürüm yalnızca hata düzeltmeleri içerir. Kullanabileceğiniz [bu sorgu](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) bu sürümde giderilen sorunlar listesinde görmek için.
