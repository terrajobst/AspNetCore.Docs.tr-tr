---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: ASP.NET Web yenilikler sayfaları 3.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/30/2014
ms.topic: article
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: 80421018e0508d430b6142cd3cee1727d1d17b7e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="whats-new-in-aspnet-web-pages-32"></a>ASP.NET Web sayfaları 3.2 yenilikler nelerdir?
====================
tarafından [Microsoft](https://github.com/microsoft)

Bu konuda, ASP.NET Web sayfaları 3.2, Web sayfaları 3.2.2 yenilikler açıklanmaktadır ve [Web sayfalarını 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>ASP.NET Web sayfaları 3.2

Bu sürüm, bir hata düzeltmeleri ve yeni bir özellik sunmaktadır.

## <a name="download"></a>İndir

Çalışma zamanı özellikleri NuGet galerisinde NuGet paketlerini olarak yayımlanmıştır. Tüm çalışma zamanı paketleri izleyin [anlamsal sürüm oluşturma](http://semver.org/) belirtimi. ASP.NET Web sayfaları 3.2 paketi aşağıdaki sürümü vardır: &ldquo;3.2.0&rdquo;. Yükleme veya güncelleştirme bu paketleri yoluyla [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). Sürüm, karşılık gelen yerelleştirilmiş NuGet paketlerini de içerir.

Yükleme veya NuGet Paket Yöneticisi konsolu kullanılarak yayımlanan NuGet paketlerini güncelleştirin:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Yeni özellik ve hata düzeltme

Biz bir hatanın düzeltildiğini ve bu sürümdeki bir ikincil özelliği geliştirme yapılır. Sorgu için aynı bulabilirsiniz [burada](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>ASP.NET Web sayfaları 3.2.2

Bu sürüm dökümünü değişikliği yukarı [ASP.NET Web sayfaları 3.2.1 Beta sürümü](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) büyük razor sayfalarının işleme içinde bir önemli performans iyileştirme sağlar. Bkz:[Codeplex sorunu 585](https://aspnetwebstack.codeplex.com/workitem/585). Bu sürüm 5.2.2 MVC ile hizalar bu sürümünde artık bağlıdır paketler.

Biz, MSN ekibi ile büyük sayfalar işleme çalışmıştır. Üzerinde 80 kilobayt veri sayfaları işlemek, biz büyük nesne yığın nesnelerle sonlanır. Birden çok düzenleri katmanlarından kullanıldığında bu etkiyi çarpılan.

Sonucu sunucuda ek CPU kullanımı, bellek ve hatta uzun daha uzun bekletme duraklatır sırasında [Gen 2 Temizleme](https://msdn.microsoft.com/en-us/library/ms973837.aspx) atık toplayıcı içinde.

Çözümleme sonuçlarını gösteren bir tablo aşağıdadır bir [perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) için bir farklı çalıştır. Büyük sayfalar oluşturulması sırasında CPU hakkında 68 oranında sabit tutulur. Tablo 2. nesil koleksiyon sayısı neredeyse tamamen ortadan ve daha yüksek istek hızı ve atık toplama nedeniyle duraklatır, önemli ölçüde azalma sonucu olduğunu gösterir.

| **Alan** | **(3.2 önce)** | **(3.2.1 sonra)** | **Delta %** |
| --- | --- | --- | --- |
| Toplam istek (sayı) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| İzleme süre (saniye) | 196.20 | 198.60 | 1.20% |
| İstek/saniye | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| CPU yükü | 68.80% | 68.50% |  -0.40% |
| GC CPU örnekleri | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Toplam ayırmaları (sayı) | 55,357,146 | 57,222,949 | 3.40% |
| Toplam GC duraklatma (örnekler) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Gen0 GC (sayı) | 403 | 1,216 | 201.70% |
| Gen1 GC (sayı) | 290 | 367 | 26.60% |
| Gen2 GC (sayı) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| CPU / istek (örnekleri/req) | 19.73 | 16.47 | -16.50% |

| Renk kodlama: | <font style="background-color: #00ff00">Çekirdek geliştirme</font> | <font style="background-color: #4bacc6">Performans üzerindeki etkisini pozitif</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web sayfaları 3.2.3 beta1

Bu sürüm yalnızca hata düzeltmeleri içerir. Kullanabileceğiniz [bu sorguyu](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) bu sürümde giderilen sorunlar listesini görmek için.
