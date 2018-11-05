---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Mobil cihazlar için sayfaları (Razor) siteleri ASP.NET Web işleme | Microsoft Docs
author: Rick-Anderson
description: 'Bu makale, mobil cihazlarda uygun şekilde işlenir bir ASP.NET Web sayfaları (Razor) sitesinde sayfaları oluşturmayı açıklar. Öğrenecekleriniz: sizinle nasıl...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: dd06a54d396bd85eeef7c004ee115828d541a2c1
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020943"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Mobil cihazlar için ASP.NET Web sayfaları (Razor) siteleri oluşturma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makale, mobil cihazlarda uygun şekilde işlenir bir ASP.NET Web sayfaları (Razor) sitesinde sayfaları oluşturmayı açıklar.
> 
> Öğrenecekleriniz:
> 
> - Bir sayfa belirtmek için bir adlandırma kuralı kullanmak nasıl mobil cihazlar için özel olarak tasarlanmıştır.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


ASP.NET Web sayfaları, mobil veya diğer cihazlar işleme içerik için özel görüntüler oluşturma olanak sağlar.

Böyle bir dosya adlandırma deseni kullanarak bir ASP.NET Web sayfaları sitesinde cihaza özgü sayfa oluşturmanın en kolay yolu olan: <em>FileName.</em> <em>Mobil</em><em>.cshtml</em>. Sayfanın iki farklı sürümlerini oluşturabilirsiniz (örneğin, bir adlı <em>MyFile.cshtml</em> adlı bir <em>MyFile.Mobile.cshtml</em>). Çalışma zamanında, bir mobil cihaz istediğinde <em>MyFile.cshtml</em>, ASP.NET içeriği işler <em>MyFile.Mobile.cshtml</em>. Aksi takdirde, <em>MyFile.cshtml</em> işlenir.

Aşağıdaki örnek, mobil cihazlar için içerik sayfası ekleyerek mobil işleme olanağı gösterilmektedir. *Page1.cshtml* içerik gezinti kenar çubuğu içerir. *Page1.Mobile.cshtml* aynı içerik içeriyor, ancak kenar atlar.

1. Bir ASP.NET Web sayfaları sitesinde adlı bir dosya oluşturun *Page1.cshtml* ve geçerli içerik aşağıdaki biçimlendirme ile değiştirin.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Adlı bir dosya oluşturun *Page1.Mobile.cshtml* ve varolan içeriği aşağıdaki biçimlendirme ile değiştirin. Sayfayı mobil sürümü daha küçük bir ekranda daha iyi işleme için Gezinti bölümde atlar dikkat edin.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Bir masaüstü tarayıcısı çalıştırın ve göz atın *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Bir mobil tarayıcı (veya bir mobil cihaz öykünücüsünü) çalıştırın ve göz atın *Page1.cshtml*. (Dahil etmezseniz bildirimi *.mobile.* URL'nin bir parçası.) İstek için olsa da *Page1.cshtml*, ASP.NET işler *Page1.Mobile.cshtml*.

    ![mobilesites 2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Mobil sayfalar test etmek için bir masaüstü bilgisayarda çalışan bir mobil cihaz simülatörünü kullanabilirsiniz. Bu aracı, mobil cihazlarda görüntülendiği şekilde, web sayfaları test sağlar (diğer bir deyişle, genellikle bir daha küçük alan görüntüler). Simülatör biri [kullanıcı aracısı değiştirici eklenti](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) Mozilla Firefox olanak sağlayan çeşitli mobil tarayıcılar Firefox Masaüstü sürümünden öykünmek.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


[Windows Phone öykünücüsü](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
