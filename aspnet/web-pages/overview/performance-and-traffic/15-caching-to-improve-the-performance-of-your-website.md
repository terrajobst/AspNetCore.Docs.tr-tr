---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: Daha iyi performans için sayfaları (Razor) Site bir ASP.NET Web verileri önbelleğe alma | Microsoft Docs
author: tfitzmac
description: Sitenizi onu depolamak - diğer bir deyişle, sağlayarak önbellek - almak veya işlem için uzun bir süre normalde götürecek veri sonuçlarını hızlandırabilir bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 742409219bd3b05f8ddf2c0d5034919fc9bf1d26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28039202"
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Bir ASP.NET Web sayfaları (Razor) sitesinde daha iyi performans için verileri önbelleğe alma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesinde daha hızlı performans için yardımcıyı bilgiyi önbelleğe kullanımı açıklanmaktadır. Sitenizi deposu &#8212;sağlayarak hızlandırabilirsiniz; diğer bir deyişle, önbellek &#8212; Normalde almak veya işlem için uzun bir süre götürecek ve genellikle değiştirmez veri sonuçları.
> 
> **Öğrenecekleriniz:** 
> 
> - Web sitenizin yanıt hızını artırmak için önbelleğe almayı kullanmak üzere nasıl.
> 
> Bu makalede sunulan ASP.NET özellikleri şunlardır:
> 
> - `WebCache` Yardımcısı.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


Birisi bir sayfayı sitenizden her istediğinde, isteği yerine getirmek için bazı iş yapmak web sunucusu vardır. Bir veritabanından veri alma gibi (yüksek) uzun zaman, görevleri gerçekleştirmek bazı sayfalarınızın sunucu olabilir. Sitenizi çok trafiği oluşursa bu görevleri uzun mutlak bağlamında ele yok olsa bile, karmaşık veya yavaş görevi gerçekleştirmek web sunucusu neden istekleri ayrı ayrı tam bir dizi çok fazla iş ekleyebilirsiniz. Bu, sonuçta site performansını etkileyebilir.

Bu gibi durumlarda Web sitenizin performansını artırmak için bir veriyi önbelleğe almak için yoludur. Sitenizi aynı bilgiler için tekrarlanan isteklerini alır ve bilgileri her kişi için değişiklik gerekmez ve zaman değilse hassas yeniden getirme veya, yeniden hesaplanması yerine, veri kez getirebilir ve ardından Sonuçların depolanacağı. Bunun için bir istek geldiğinde sonraki açışınızda bilgi, bunu yalnızca önbellek dışına almanız.

Genel olarak, sık değişmeyen bilgilerini önbelleğe. Önbellekte bilgi geçirdiğinizde, web sunucusu üzerindeki bellekte depolanır. Ne kadar süreyle onu, gün saniyeye gelen önbelleğe alınması gereken belirtebilirsiniz. Önbelleğe alma süresi sona erdiğinde, bilgileri otomatik olarak önbellekten kaldırılır.

> [!NOTE]
> Önbelleğindeki girişleri süresi nedenlerle dışında kaldırılabilir. Örneğin, web sunucusu geçici olarak belleği düşük çalışabilir ve belleği geri bir önbellek dışına girişleri atma tarafından yoludur. Bilgi önbelleğe yerleştirdiğiniz olsa bile, göreceğiniz gibi ihtiyacınız olduğunda hala var olduğundan emin olun sahiptir.


Web sitenizi geçerli sıcaklık ve hava tahmini görüntüleyen bir sayfa sahip düşünün. Bu tür bilgiler almak için bir dış hizmetine bir istek gönderebilir. Bu bilgi büyük (iki saatlik süre içinde örneğin) değişmez beri ve dış çağrıları süresi ve bant genişliği gerektiren bu yana, önbelleğe alma işlemi için iyi bir aday sağlanır.

## <a name="adding-caching-to-a-page"></a>Bir sayfaya önbelleğe alma ekleme

ASP.NET içeren bir `WebCache` önbelleğe alma sitenize ekleyin ve veri önbelleğine ekleme daha kolay hale getirir Yardımcısı. Bu yordamda, geçerli saati önbelleğe alan bir sayfa oluşturacaksınız. Geçerli saati, sık sık değişen ve, ayrıca hesaplamak için karmaşık olmayan bir şey olduğundan bu gerçek dünya örneği değil. Ancak, önbelleğe alma eylemini göstermeye iyi yoldur.

1. Adlı yeni bir sayfa ekleyin *WebCache.cshtml* Web sitesine.
2. Aşağıdaki kod ve biçimlendirme sayfasına ekleyin:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Veri önbelleğe aldığınızda, bunu bir ad kullanarak önbelleğe bu yerleştirdiğiniz Web sitesi arasında benzersizdir. Bu durumda, adlandırılmış bir önbellek girişi kullanacağınız `CachedTime`. Bu `cacheItemKey` aşağıdaki kod örneğinde gösterildiği.

    Kod ilk okur `CachedTime` önbellek girişi. (Diğer bir deyişle, önbellek girişi null değilse) değeri döndürülürse, kod verileri önbelleğe almak için yalnızca zaman değişkenin değerini ayarlar.

    Ancak, önbellek girişi yoksa (yani, null ise), kodu saat değeri ayarlar, önbelleğe ekler ve bir süre sonu değeri bir dakika olarak ayarlar. Bir dakika sonra önbellek girişi göz ardı edilir. (Varsayılan süre sonu değeri öğenin önbellekte 20 dakikadır.) Komut `WebCache.Set(cacheItemKey, time, 1, false)` önbelleğe geçerli saat değeri ekleyin ve ermesinden 1 dakika olarak ayarlarsanız gösterilmektedir. Ayarı *slidingExpiration* parametresi `false` , istenen her zaman sona erme saati yenilenmiş değil anlamına gelir. Tam olarak 1 dakika önbelleğe ilk olarak eklendikten sonra dolar. Bu değer ayarlanırsa, `true` 1 dakika süre sonu zamanı değeri önbellekten istenen her zaman sıfırlanır.

    Bu kod, verileri önbelleğe aldığınızda, her zaman kullanmalısınız düzeni gösterilmektedir. Önbellek dışına almak önce her zaman ilk kontrol olup olmadığını `WebCache.Get` yöntemi null döndürdü. Önbellek girişinin süresi sona ermiş olabilir veya belirli bir girişin hiçbir zaman önbellekte olması garanti şekilde diğer herhangi bir nedenle için kaldırılmış unutmayın.
3. Çalıştırma *WebCache.cshtml* bir tarayıcıda. (Emin olun sayfa seçildiğinde, **dosyaları** çalıştırmadan önce onu çalışma.) Sayfa isteği ilk kez saat verilerini önbellekte değil ve saat değeri önbelleğine eklemek koduna sahip.

    ![Önbellek-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Yenileme *WebCache.cshtml* tarayıcıda. Bu süre, önbellekte zaman verilerdir. Zaman sayfasında görüntülenen son tarihten değişmediğinden dikkat edin.

    ![Önbellek 2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Önbellek boşaltılması için bir dakika bekleyin ve sayfayı yenileyin. Sayfa yeniden saat verilerini önbellekte bulunamadı ve güncelleştirilmiş zaman önbelleğe eklenir gösterir.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


- [Verileri Bir Grafikte Görüntüleme](https://go.microsoft.com/fwlink/?LinkId=202895)
- [WebCache API Başvurusu](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
