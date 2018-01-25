---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: "Mobil cihazlar için sayfaları (Razor) siteleri ASP.NET Web işleme | Microsoft Docs"
author: tfitzmac
description: "Bu makalede, mobil cihazlarda uygun şekilde kılacak bir ASP.NET Web sayfaları (Razor) sitesinde sayfaları oluşturmayı açıklar. Öğrenecekleriniz: size nasıl..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 899bbdef82d689be81cd77ea6805e0484fb614aa
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Mobil cihazlar için ASP.NET Web sayfaları (Razor) siteleri oluşturma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, mobil cihazlarda uygun şekilde kılacak bir ASP.NET Web sayfaları (Razor) sitesinde sayfaları oluşturmayı açıklar.
> 
> Öğrenecekleriniz:
> 
> - Bir adlandırma kuralı belirten bir sayfa için nasıl kullanılacağını mobil cihazlar için özel olarak tasarlanmıştır.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


ASP.NET Web sayfaları, cep telefonu numarası veya diğer cihazları işleme içerik için özel görüntüler oluşturmanıza olanak sağlar.

Böyle bir dosya adlandırma deseni kullanarak bir ASP.NET Web Pages sitesinde aygıta özgü sayfası oluşturmak için en basit yolu olan: *FileName. **Mobil**.cshtml*. Sayfanın iki farklı sürümlerini oluşturabilirsiniz (örneğin, bir adlı *MyFile.cshtml* ve adlı bir *MyFile.Mobile.cshtml*). Çalışma zamanında, bir mobil cihaz isteğinde bulunduğunda *MyFile.cshtml*, ASP.NET işler içerikten *MyFile.Mobile.cshtml*. Aksi takdirde, *MyFile.cshtml* işlenir.

Aşağıdaki örnek, mobil cihazlar için bir içerik sayfasını ekleyerek mobil işleme etkinleştirmek gösterilmiştir. *Page1.cshtml* içerik ek olarak bir gezinti Kenar Çubuğu'nu içerir. *Page1.Mobile.cshtml* aynı içerik içeriyor, ancak kenar atlar.

1. Bir ASP.NET Web Pages sitesinde adlı bir dosya oluşturun *Page1.cshtml* ve geçerli içerik aşağıdaki biçimlendirme ile değiştirin.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Adlı bir dosya oluşturun *Page1.Mobile.cshtml* ve varolan içeriği aşağıdaki biçimlendirme ile değiştirin. Sayfanın mobil sürümünü daha küçük bir ekran üzerinde daha iyi işleme için gezinti bölümüne atlar dikkat edin.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Bir masaüstü tarayıcısı çalıştırın ve Gözat *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Bir mobil tarayıcı (veya bir mobil aygıt benzeticisi) çalıştıran ve Gözat *Page1.cshtml*. (İçermiyor bildirimi *.mobile.* URL'SİNİN bir parçası.) İstek için olsa da *Page1.cshtml*, ASP.NET işler *Page1.Mobile.cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Mobil sayfalar sınamak için bir masaüstü bilgisayar üzerinde çalışan bir mobil aygıt benzeticisi kullanabilirsiniz. Bu araç, mobil cihazlarda görüneceği şekilde web sayfalarını test sağlar (diğer bir deyişle, genellikle bir daha küçük alan gösterir). Bir simulator bir örnektir [kullanıcı aracısı değiştirici eklenti](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) Mozilla Firefox olanak sağlayan, Firefox Masaüstü sürümünden çeşitli mobil tarayıcılar öykünmek.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


[Windows Phone öykünücüsü](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
