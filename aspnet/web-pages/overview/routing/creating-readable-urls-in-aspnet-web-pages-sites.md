---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Okunabilir URL'ler oluşturma ASP.NET Web sayfaları (Razor) siteler | Microsoft Docs
author: tfitzmac
description: Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesi ve bu daha okunabilir ve SEO için daha iyi URL'leri kullanmanıza nasıl sağladığını yönlendirme açıklanır. Gerekir...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: b8405283dc5bf44a4cd8d1122d327346774d95e8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755156"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web sayfaları (Razor) sitelerinde okunabilir URL'ler oluşturma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesi ve bu daha okunabilir ve SEO için daha iyi URL'leri kullanmanıza nasıl sağladığını yönlendirme açıklanır.
> 
> Öğrenecekleriniz:
> 
> - Nasıl ASP.NET yönlendirmesi daha okunabilir ve aranabilir URL'lere kullanmanıza izin vermek için kullanır.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


## <a name="about-routing"></a>Yönlendirme hakkında

Sitenizde sayfaları için URL'leri site ne kadar iyi çalıştığı bir etkisi olabilir. Bir URL bu &quot;kolay&quot; sitesini kullanmak üzere kişiler için daha kolay yapabilirsiniz. Site için arama motoru iyileştirmesi (SEO) ile de yardımcı olabilir. ASP.NET Web sitesini kolay URL'leri otomatik olarak kullanma yeteneğini içerir.

ASP.NET yerine yalnızca bir dosya sunucusuna işaret eden kullanıcı eylemleri açıklayan anlamlı URL'ler oluşturmasına olanak tanır. Bu URL'ler için kurgusal bir blog göz önünde bulundurun:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Bu URL'leri aşağıdaki karşılaştırma:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

İlk çiftini, bir kullanıcı blog kullanarak görüntülendiğini bilmesi gerekir *blog.cshtml* sayfasında ve doğru bir kategori veya tarih aralığını alır bir sorgu dizesi oluşturmak sahip. İkinci bir örnekler kümesini kavrama ve oluşturmak çok daha kolaydır.

İlk örnek için URL'leri ayrıca doğrudan belirli bir dosyaya işaret (*blog.cshtml*). Herhangi bir nedenden dolayı sunucuda başka bir klasöre blog taşınan veya blog farklı bir sayfayı kullanmak için yazılan, bağlantıları yanlış olur. İkinci Küme URL'leri belirli bir sayfaya işaret etmiyor da blog uygulanması veya konum değişirse, URL'leri yine de geçerli olacaktır.

ASP.NET Web sayfaları, ASP.NET kullandığından bu Yukarıdaki örneklerde gibi daha kolay URL'leri oluşturabilirsiniz *yönlendirme*. Yönlendirme isteği gerçekleştirmek bir URL'den bir sayfayı (veya sayfaları) için mantıksal eşleme oluşturur. Eşleme mantıksal olduğu için (fiziksel, belirli bir dosyaya), yönlendirme URL'leri siteniz için nasıl tanımladığınızı büyük esneklik sağlar.

## <a name="how-routing-works"></a>Yönlendirmenin nasıl çalışır?

ASP.NET isteği işlerken yönlendirmek nasıl saptamak için URL'ye okur. ASP.NET, dosyaları diskte URL'sine soldan sağa doğru giden tek tek bölümleri eşleşmiyor dener. Bir eşleşme varsa, URL'yi kalan herhangi bir şey sayfasına geçirilir *yol bilgisi*.

Birisi bu URL'yi kullanarak bir isteği yapar düşünün:

`http://www.contoso.com/a/b/c`

Arama şöyle geçer:

1. Bir dosya yolu ve adı yok */a/b/c.cshtml*? Bu durumda, bu sayfayı çalıştırın ve hiçbir bilgi geçirin. Aksi takdirde...
2. Bir dosya yolu ve adı yok */a/b.cshtml*? Bu nedenle, bu sayfayı çalıştırın ve değeri geçirin `c` ona. Aksi takdirde...
3. Bir dosya yolu ve adı yok */a.cshtml*? Bu nedenle, bu sayfayı çalıştırın ve değeri geçirin `b/c` ona.

Hayır tam bulunan arama için eşleşiyorsa *.cshtml* belirtilen klasörlerine dosyaları ASP.NET devam bu dosyalar sırayla aranıyor:

1. */a/b/c/default.cshtml* (hiçbir yol bilgisi).
2. */a/b/c/index.cshtml* (hiçbir yol bilgisi).

> [!NOTE]
> Açık olması için belirli bir sayfa istekleri (diğer bir deyişle, içeren istekleri *.cshtml* dosya adı uzantısı) yalnızca beklediğiniz gibi çalışır. Bir isteği ister `http://www.contoso.com/a/b.cshtml` sayfasının çalışacağı *b.cshtml* düzgün.


Bir sayfa içinde sayfa aracılığıyla yolu bilgi edinebilirsiniz `UrlData` bir sözlük özelliği. Adlı bir dosyaya sahip olduğunuzu düşünün *ViewCustomers.cshtml* ve bu istek, sitenizi alır:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Yukarıdaki kuralları açıklandığı gibi istek sayfanıza geçer. İçinde sayfasında, aşağıdaki gibi bir kod almak ve yol bilgilerini görüntülemek için kullanabilirsiniz (Bu durumda, değer &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Yönlendirme tam dosya adları içermeyen çünkü olabilir belirsizlik aynı sayfaları varsa ad ancak farklı bir dosya adı uzantıları (örneğin, *MyPage.cshtml* ve *MyPage.html*) . Yönlendirme sorunlarını önlemek için sitenizde kendi uzantı yalnızca, adları farklı sayfaları yoksa emin olmak idealdir.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

[WebMatrix - URL'leri, UrlData ve SEO için yönlendirme](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Bu blog girişi Mike Brind tarafından ASP.NET Web Pages'de yönlendirmenin nasıl çalıştığıyla ilgili bazı ek ayrıntılar sağlar.
