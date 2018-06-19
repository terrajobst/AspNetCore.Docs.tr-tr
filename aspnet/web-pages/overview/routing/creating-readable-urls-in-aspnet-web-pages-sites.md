---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Okunabilir URL'ler oluşturma ASP.NET Web sayfaları (Razor) siteleri | Microsoft Docs
author: tfitzmac
description: Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesi ve nasıl bu daha okunabilir ve SEO için daha iyi URL'leri kullanmanıza olanak sağlayan yönlendirme açıklanmaktadır. Ne artıracaksınız...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573231"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web sayfaları (Razor) sitelerinde okunabilir URL'ler oluşturma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesi ve nasıl bu daha okunabilir ve SEO için daha iyi URL'leri kullanmanıza olanak sağlayan yönlendirme açıklanmaktadır.
> 
> Öğrenecekleriniz:
> 
> - Nasıl ASP.NET yönlendirme daha okunabilir ve aranabilir URL'lere kullanmanıza izin vermek için kullanır.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


## <a name="about-routing"></a>Yönlendirme hakkında

Sitenizdeki sayfalar için URL'leri site ne kadar iyi çalıştığı bir etkisi olabilir. Bir URL bu &quot;kolay&quot; sitesini kullanmak üzere kişiler için daha kolay hale getirebilir. Site için arama motoru iyileştirme (SEO) de yardımcı olabilir. ASP.NET Web siteleri kolay URL'leri otomatik olarak kullanma yeteneğini içerir.

ASP.NET, yalnızca sunucu üzerindeki bir dosyaya işaret eden yerine kullanıcı eylemleri açıklayan anlamlı URL'leri oluşturmanızı sağlar. Bu URL'ler için kurgusal bir blog göz önünde bulundurun:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Bu URL'leri aşağıdaki olanlara karşılaştırın:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

İlk çiftinin bir kullanıcı blog kullanılarak görüntülenir bilmesi gerekir *blog.cshtml* sayfasında ve sağ kategori veya tarih aralığı alır bir sorgu dizesi oluşturmak sahip. Örnekler ikinci kümesi kavrama ve oluşturmak çok daha kolaydır.

İlk örnek için URL'leri de doğrudan belirli bir dosyaya işaret (*blog.cshtml*). Herhangi bir nedenden dolayı sunucu üzerinde başka bir klasöre blog taşınan veya blog başka bir sayfaya kullanacak şekilde yeniden yazılmıştır, bağlantıları yanlış olabilir. Belirli bir sayfaya URL'leri ikinci kümesi gösteremiyor da blog uygulama veya konumu değişirse, URL'leri hala geçerli olacaktır.

ASP.NET Web sayfaları, ASP.NET kullandığından Yukarıdaki örneklerde gelenler yaşamanızı URL'leri oluşturabilirsiniz *yönlendirme*. Yönlendirme mantıksal eşleme isteği gerçekleştirmek bir URL'den bir sayfa (veya sayfaları) oluşturur. Eşleme mantıksal olduğundan (fiziksel, belirli bir dosya için), yönlendirme URL'leri siteniz için nasıl tanımladığınız büyük esneklik sağlar.

## <a name="how-routing-works"></a>Yönlendirmenin nasıl çalışır

ASP.NET bir isteği işlerken yönlendirmek nasıl belirlemek için URL'yi okur. ASP.NET, diskte dosyalar soldan sağa giderek URL'sini tek tek parçaları eşleştirmeyi dener. Bir eşleşme varsa, URL'de kalan her şey için sayfa olarak geçirilen *yol bilgisi*.

Birisi bu URL'yi kullanarak bir isteği yapar düşünün:

`http://www.contoso.com/a/b/c`

Arama şöyle geçer:

1. Bir dosya yolu ve adı yok */a/b/c.cshtml*? Öyleyse, bu sayfayı çalıştırın ve hiçbir bilgi ona geçirin. Aksi takdirde...
2. Bir dosya yolu ve adı yok */a/b.cshtml*? Bu nedenle, bu sayfayı çalıştırın ve değerini geçirin `c` ona. Aksi takdirde...
3. Bir dosya yolu ve adı yok */a.cshtml*? Bu nedenle, bu sayfayı çalıştırın ve değerini geçirin `b/c` ona.

Hayır tam arama için eşleşiyorsa *.cshtml* belirtilen klasörlerine dosyalarında, ASP.NET devam bu dosyalar sırayla aranıyor:

1. */a/b/c/default.cshtml* (yol bilgisi yok).
2. */a/b/c/index.cshtml* (yol bilgisi yok).

> [!NOTE]
> Açık olması açısından belirli sayfaları için istekleri (diğer bir deyişle, içeren isteklerin *.cshtml* dosya adı uzantısı) yalnızca beklediğiniz gibi çalışır. Bir istek ister `http://www.contoso.com/a/b.cshtml` sayfasının çalışacağı *b.cshtml* yalnızca iyi.


İçinde bir sayfa, sayfanın yolu bilgisini elde edebilirsiniz `UrlData` bir sözlük özelliği. Adlı bir dosya olduğunu düşünün *ViewCustomers.cshtml* ve bu istek, sitenizi alır:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Yukarıdaki kurallara açıklandığı gibi istek sayfanıza gidin. Sayfanın içinde aşağıdaki gibi bir kod almak ve yol bilgileri görüntülemek için kullanabilirsiniz (Bu durumda, değer &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Yönlendirme tam dosya adlarının içermeyen çünkü olabilir belirsizlik aynı olan sayfaları varsa ad ancak farklı dosya adı uzantıları (örneğin, *MyPage.cshtml* ve *MyPage.html*) . Yönlendirme sorunlarını önlemek için bunların uzantısı yalnızca, adları farklı sitenizdeki sayfaları yok emin olmak en iyisidir.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

[WebMatrix - URL'leri, UrlData ve SEO için yönlendirme](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Bu blog girişi CAN Brind tarafından yönlendirmenin nasıl çalıştığını ASP.NET Web Pages'de bazı ek ayrıntılar sağlar.
