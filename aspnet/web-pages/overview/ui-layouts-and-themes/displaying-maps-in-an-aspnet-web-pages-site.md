---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: MAPS görüntüleyen bir ASP.NET Web sayfaları (Razor) Site | Microsoft Docs
author: tfitzmac
description: Bu makalede, Bing, Google, Ma tarafından sağlanan hizmetlerin eşleme dayalı bir ASP.NET Web sayfaları (Razor) Web sayfalarında etkileşimli eşlemeleri görüntülemek açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 608dab8760bad7b877ab6fd4f89b21e980f5b1db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitesinde eşlemeleri görüntüleme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, Bing, Google, MapQuest ve Yahoo tarafından sağlanan hizmetlerin eşleme dayalı bir ASP.NET Web sayfaları (Razor) Web sayfalarında etkileşimli eşlemeleri görüntülemek açıklanmaktadır.
> 
> Öğrenecekleriniz:
> 
> - Bir adresini temel alan bir harita oluşturmak nasıl.
> - Enlem ve boylam koordinatları temelinde bir harita oluşturmak nasıl.
> - Nasıl bir Bing Haritalar Geliştirici hesabını kaydetmek ve Bing Haritalar ile kullanmak için bir anahtar alın.
> 
> Bu makalede sunulan ASP.NET özelliğidir:
> 
> - `Maps` Yardımcısı.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - WebMatrix 2
>   
> 
> Bu öğretici, WebMatrix 3 ile de çalışır.


Web sayfalarında, maps bir sayfada kullanarak görüntüleyebileceğiniz `Maps` Yardımcısı. Bir adres veya boylam ve enlem koordinatları kümesi üzerinde alan eşlemeleri oluşturabilirsiniz. `Maps` Sınıfı Bing, Google, MapQuest ve Yahoo gibi popüler harita motorları çağrı sağlar.

Bir sayfaya eşleme ekleme adımları hangi harita motorları bağımsız olarak çağrı aynıdır. Eşlemeyi görüntülemek için kullanılabilir yöntemleri sağlayan JavaScript dosyası başvuru eklemeniz yeterlidir, ardından yöntemlerini çağıran `Maps` Yardımcısı.

Temel bir harita hizmet seçin `Maps` yardımcı yöntemini kullanın. Aşağıdakilerden herhangi birini kullanabilirsiniz:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Gereksinim duyduğunuz parça yükleme

MAPS görüntülemek için bu parça gerekir:

- `Maps` Yardımcısı. Bu yardımcı sürüm ASP.NET Web Yardımcıları kitaplığı 2 dosyasıdır. Kitaplık zaten eklemediyseniz, bir NuGet paketi olarak sitenizdeki yükleyebilirsiniz. Ayrıntılar için bkz [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372). (Galerisi'nde arama `microsoft-web-helpers` paket.)
- JQuery kitaplığı. WebMatrix site şablonları çeşitli zaten jQuery kitaplıklarında dahil kendi *betik* klasörler. Bu kitaplıklar yoksa, en son jQuery kitaplıktan doğrudan indirebilirsiniz [jQuery.org](http://jQuery.org) site. Veya bir şablon kullanarak yeni bir site oluşturun (örneğin, **başlangıç sitesi** şablonu) ve jQuery dosyaları siteden geçerli sitenize kopyalayın.

Son olarak, Bing Haritalar kullanmak istiyorsanız, öncelikle (ücretsiz) bir hesap oluşturun ve bir anahtar alın. Bir anahtar almak için şu adımları izleyin:

1. Bir hesap oluşturmak [Bing Haritalar Geliştirici hesabını](https://www.microsoft.com/maps/developers/web.aspx). Bir Microsoft hesabı (Windows Live ID) de olması gerekir.

    Anahtar için kullanmak istediğiniz belirtebilirsiniz **değerlendirme ve Test**. WebMatrix ve IIS Express kullanarak kendi bilgisayarınızda eşleme işlevi test ediyorsanız, Git **Site** çalışma ve Not sitenizin URL'sini (örneğin, `http://localhost:50408`, bağlantı noktası numarası muhtemelen farklı olacaktır rağmen). Bu kullanabilirsiniz *localhost* kaydettiğinizde site adres.
2. İçin bir hesap kaydettikten sonra Bing Haritalar hesap Merkezi'ne gidin ve tıklatın **anahtarlar oluşturma veya Görünüm**:

    ![eşleme 2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Bing oluşturur anahtar kaydedin.

## <a name="creating-a-map-based-on-an-address-using-google"></a>(Google kullanarak) bir adresini temel alan bir eşleme oluşturma

Aşağıdaki örnekte bir adresini temel alan bir harita işleyen bir sayfa oluşturulacağını gösterir. Bu örnek, Google haritalar kullanmayı gösterir.

1. Adlı bir dosya oluşturun *MapAddress.cshtml* sitenin kök. Bu sayfa için geçirdiğiniz bir adresi göre bir harita oluşturur.
2. Aşağıdaki kod, var olan içeriğin üzerine dosyasına kopyalayın.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Sayfanın aşağıdaki özelliklere dikkat edin:

    - `<script>` Öğesinde `<head>` öğesi. Örnekte, `<script>` öğesi başvuruları *jquery 1.6.4.min.js* jQuery kitaplığı, sürüm 1.6.4 küçültülmüş (sıkıştırılmış) sürümü dosya. Başvuru varsaydığını unutmayın *.js* dosyası *betikleri* sitenizin klasörüne. 

        > [!NOTE]
        > JQuery kitaplığı farklı bir sürümünü kullanıyorsanız, yalnızca, bu sürüme doğru işaret ettiğinden olduğundan emin olun.
    - Çağrı `@Maps.GetGoogleHtml` sayfasının gövdesindeki. Bir adresi eşlemek için bir adres dizesinin geçmesi gerekir. Bir harita altyapıları için yöntemler benzer şekilde çalışır (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
3. Sayfayı çalıştırın ve bir adres girin. Sayfasında, belirttiğiniz konuma gösterir Google haritalar üzerinde temel bir harita görüntüler.

     ![eşleme-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Enlem ve boylam dayalı bir harita oluşturmak (Bing kullanarak) koordine eder

Bu örnek bir harita koordinatlarına göre oluşturulacağını gösterir. Bu örnek, Bing Haritalar kullanmayı ve Bing anahtarınızı içerecek şekilde nasıl gösterir. (Bir harita motorları ayrıca Bing anahtar kullanmadan koordinatları dayalı bir harita oluşturabilirsiniz.)

1. Adlı bir dosya oluşturun *MapCoordinates.cshtml* kök site ve var olan içerik aşağıdaki kodu ve İşaretleme ile değiştirin:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Değiştir `your-key-here` daha önce oluşturulan Bing Haritalar anahtara sahip.
3. Çalıştırma *MapCoordinates.cshtml* sayfasında, enlem ve boylam koordinatları girin ve ardından **Map It!** düğme. (Tüm koordinatları bilmiyorsanız, aşağıdaki deneyin. Microsoft Redmond kampüs konumunda budur.)

   - Latitude: 47.6781005859375
   - Boylam:-122.158317565918

     Belirttiğiniz koordinatları kullanarak sayfası görüntülenir.

     ![eşleme-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


[Microsoft.Maps API Başvurusu](https://msdn.microsoft.com/library/gg427611.aspx)
