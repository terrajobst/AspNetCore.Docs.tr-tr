---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: Daha iyi performans için sayfaları (Razor) sitesinde bir ASP.NET Web verileri önbelleğe alma | Microsoft Docs
author: tfitzmac
description: Sitenizi depolamak - diğer bir deyişle, sağlayarak önbellek - normalde almak veya önemli ölçüde zaman alabileceğini veri sonuçlarını hızlandırabilirsiniz bir...
ms.author: riande
ms.date: 02/14/2014
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 53a3f38c23dc20479ddf231fe24a200d823d0345
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753636"
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Bir ASP.NET Web sayfaları (Razor) sitesinde daha iyi performans için verileri önbelleğe alma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesinde daha hızlı performans bilgileri yardımcı kullanmayı açıklar. Depolama sağlayarak sitenizi hızlandırabilirsiniz &#8212; diğer bir deyişle, önbellek &#8212; , normalde ele almak veya işlem için büyük miktarda zaman ve, değişmez genellikle veri sonuçlarını.
> 
> **Öğrenecekleriniz:** 
> 
> - Web sitenizin uygulamanın yanıt verme hızını artırmak için önbelleğe almayı kullanmak üzere nasıl.
> 
> Bu makalede sunulan ASP.NET özellikleri şunlardır:
> 
> - `WebCache` Yardımcısı.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


Birisi bir sayfa sitenizden istediği her durumda, isteği yerine getirmek için biraz çalışmanız gerekir. web sunucusu vardır. Sayfalarınızı bazıları için bir veritabanından veri almak gibi (daha) uzun sürüyor, görevleri gerçekleştirmek sunucu olabilir. Sitenizi çok trafik karşılaşırsa uzun mutlak bağlamında, bu görevleri yakalayana olsa bile, bir dizi karmaşık olan veya yavaş görevi gerçekleştirmek web sunucusu neden ayrı istekleri için çok fazla iş ekleyebilirsiniz. Sonuç olarak, site performansını da etkileyebilir.

Bu gibi durumlarda Web sitenizin performansını artırmak için bir verileri önbelleğe almak için yoludur. Zaman değil aynı bilgileri yönelik yinelenen isteklerden sitenizi alır ve bilgileri, her kişi için değiştirilmesi gerekmez. yaparsanız yeniden getirilirken veya, yeniden hesaplanması yerine hassas verileri bir kez getirin ve sonra sonuçları depolamak. İçin bir istek geldiğinde sonraki açışınızda bilgi, yalnızca bu önbellek dışına sahip olursunuz.

Genel olarak, sık sık değişmeyen bilgileri önbelleğe alın. Önbellekte bilgi yerleştirdiğinizde, web sunucusu üzerindeki bellekte depolanır. Ne kadar süreyle Bu, gün saniyelerden önbelleğe alınması gerektiğini belirtebilirsiniz. Önbelleğe alma süresi sona erdiğinde, bilgi önbellekten otomatik olarak kaldırılır.

> [!NOTE]
> Önbelleğindeki süresi nedenlerle dışında kaldırılabilir. Örneğin, web sunucusunun geçici olarak belleği düşük çalışabilir ve belleği geri kazanmak bir önbellek dışına girişleri özel durum atma yoludur. Bilgi önbelleğe yerleştirdiğiniz olsa bile, gördüğünüz gibi ihtiyacınız olduğunda hala var olduğundan emin olun gerekir.


Imagine Web sitenizi geçerli sıcaklık ve hava durumu tahminini görüntüleyen bir sayfa vardır. Bu tür bilgiler almak için bir dış hizmete istek gönderebilir. Bu bilgiler çok (iki saatlik süre içinde örneğin) değişmez olduğundan ve dış aramalar süreyi ve bant genişliği gerektirdiklerinden, önbelleğe alma işlemi için iyi bir aday olacaktır.

## <a name="adding-caching-to-a-page"></a>Bir sayfaya önbelleğe alma ekleme

ASP.NET içeren bir `WebCache` sitenize önbelleğe alma ekleme ve verileri önbelleğe eklemek kolaylaştıran Yardımcısı. Bu yordamda, geçerli zamanı önbelleğe alan bir sayfa oluşturacaksınız. Geçerli saati, sık sık değişen ve, ayrıca hesaplamak için karmaşık olmayan bir şey olduğundan bu gerçek bir örnek, değildir. Ancak, önbelleğe alma eylemini göstermek için iyi bir yoldur.

1. Adlı yeni bir sayfa ekleyin *WebCache.cshtml* Web sitesine.
2. Aşağıdaki kod ve İşaretleme sayfaya ekleyin:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Verileri önbelleğe alma, bunu bir adı kullanarak önbelleğe bu yerleştirdiğiniz Web sitesi arasında benzersizdir. Bu durumda, adlandırılmış bir önbellek girdisi kullanacağınız `CachedTime`. Bu `cacheItemKey` aşağıdaki kod örneğinde gösterilen.

    Kod ilk okur `CachedTime` önbellek girişi. (Diğer bir deyişle, önbellek girişi null değilse) bir değer döndürülürse, kod verileri önbelleğe almak için yalnızca zaman değişkenin değerini ayarlar.

    Ancak, önbellek girişi yoksa (yani, null ise), kod zaman değeri ayarlar, önbelleğe ekler ve bir dakika süre sonu değeri ayarlar. Bir dakika sonra önbellek girişi göz ardı edilir. (Varsayılan süre sonu değeri bir öğenin önbellekte 20 dakikadır.) Komut `WebCache.Set(cacheItemKey, time, 1, false)` ermesinden 1 dakika olarak ayarlayın ve geçerli bir zaman değerini önbelleğine eklemek gösterilmektedir. Ayarı *ilerlemiş* parametresi `false` süre sonu olmayan her zaman, istenen yenilendi anlamına gelir. Bu tam olarak 1 dakika önbelleğe ilk olarak eklendikten sonra sona erecek. Bu değeri ayarlamanız `true` önbelleğe alınan değeri istenen her zaman 1 dakika süre sıfırlanır.

    Bu kod, verileri önbelleğe alma, her zaman kullanmalısınız düzeni göstermektedir. Önbellek dışına geçmeden önce her zaman öncelikle denetleyin olmadığını `WebCache.Get` metodu null döndürdü. Önbellek girişinin süresi dolmuş olabilir veya belirli bir girişin hiçbir zaman önbellekte olması garanti için bazı diğer nedenlerle kaldırılmış olabilir unutmayın.
3. Çalıştırma *WebCache.cshtml* bir tarayıcıda. (Emin sayfanın içinde seçili **dosyaları** çalıştırmadan önce çalışma alanı.) İlk kez, sayfa istek saat verilerini önbellekte değil ve saat değeri önbelleğine eklemek kod vardır.

    ![1. önbellek](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Yenileme *WebCache.cshtml* tarayıcıda. Bu süre önbellekte zaman verilerdir. Zaman sayfası görüntülediğiniz son daraltılmasından değişmediğinden dikkat edin.

    ![Önbellek-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Önbellek boşaltılması için bir dakika bekleyin ve sonra sayfayı yenileyin. Sayfa yeniden önbellek süresi verisi bulunamadı ve güncelleştirme zamanı önbelleğe eklenir gösterir.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


- [Verileri Bir Grafikte Görüntüleme](https://go.microsoft.com/fwlink/?LinkId=202895)
- [WebCache API Başvurusu](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
