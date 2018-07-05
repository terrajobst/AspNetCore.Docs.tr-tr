---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Haritalar görüntüleyen bir ASP.NET Web sayfaları (Razor) sitesinde | Microsoft Docs
author: tfitzmac
description: Bu makalede, Bing, Google, Ma tarafından sağlanan hizmetleri eşleme'temelinde bir ASP.NET Web sayfaları (Razor) Web sitesi sayfalarında etkileşimli haritaları görüntülemesi açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 210cc37427024a4e8cae309634141900bb28b55e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368841"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitesinde haritaları görüntüleme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, Bing, Google, MapQuest ve Yahoo tarafından sağlanan hizmetleri eşleme'temelinde bir ASP.NET Web sayfaları (Razor) Web sitesi sayfalarında etkileşimli haritaları görüntülemesi açıklanmaktadır.
> 
> Öğrenecekleriniz:
> 
> - Nasıl bir adresini temel alan bir harita oluşturur.
> - Enlem ve boylam koordinatlarına göre bir harita oluşturmak nasıl.
> - Nasıl bir Bing Haritalar Geliştirici hesabı kaydedin ve Bing Haritalar ile kullanmak için bir anahtar alın.
> 
> Bu makalede sunulan ASP.NET özelliğidir:
> 
> - `Maps` Yardımcısı.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - WebMatrix 2
>   
> 
> Bu öğreticide, WebMatrix 3'ile de çalışır.


Web sayfaları'nda eşlemeleri bir sayfada kullanarak görüntüleyebileceğiniz `Maps` Yardımcısı. Bir adres veya boylam ve enlem koordinatları kümesini temelli haritalar oluşturabilirsiniz. `Maps` Sınıfı Bing, Google, MapQuest ve Yahoo gibi popüler harita altyapıları çağırmanızı sağlar.

Bir sayfaya eşleme ekleme adımlarını çağırırsınız, harita altyapıları bağımsız olarak aynıdır. Haritada görüntülemek için kullanılabilen yöntemler sağlayan bir JavaScript dosya başvurusu eklemeniz yeterlidir ve ardından yöntemlerini çağırmanızı `Maps` Yardımcısı.

Temel bir harita hizmeti seçtiğiniz `Maps` yardımcı yöntemini kullanırsınız. Aşağıdakilerden herhangi birini kullanabilirsiniz:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Gereksinim duyduğunuz parçaları yükleme

Haritalar görüntülemek için bu parçaları gerekir:

- `Maps` Yardımcısı. Bu yardımcı, ASP.NET Web Yardımcıları kitaplığı 2. sürümü değil. Kitaplık eklemediyseniz, bir NuGet paketi olarak sitenizdeki yükleyebilirsiniz. Ayrıntılar için bkz [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372). (Galeride arama `microsoft-web-helpers` paket.)
- JQuery kitaplığı. WebMatrix site şablonları bazıları zaten jQuery kitaplıkları dahil, *betik* klasörleri. Bu kitaplıklar yoksa en son jQuery Kitaplığı'ndan doğrudan indirebileceğiniz [jQuery.org](http://jQuery.org) site. Ya da bir şablon kullanarak yeni bir site oluşturabilirsiniz (örneğin, **başlangıç sitesi** şablonu) ve geçerli sitenize'da bu siteden jQuery dosyaları kopyalayın.

Son olarak, Bing Haritalar'ı kullanmak isterseniz, öncelikle (ücretsiz) hesabı oluşturun ve bir anahtar alın. Bir anahtarı almak için şu adımları izleyin:

1. Bir hesap oluşturmak [Bing Haritalar Geliştirici hesabı](https://www.microsoft.com/maps/developers/web.aspx). Bir Microsoft hesabı (Windows Live kimliği) de olması gerekir.

    Anahtarı kullanmak istediğinizi belirtebilirsiniz **değerlendirme ve Test**. WebMatrix ve IIS Express kullanarak kendi bilgisayarınızda eşleme işlevi sınıyorsanız, Git **Site** çalışma ve Not sitenizin URL'sini (örneğin, `http://localhost:50408`, bağlantı noktası numaranızı büyük olasılıkla farklı olsa da). Bu *localhost* kaydettiğinizde site adresi.
2. Bir hesap için kaydettikten sonra Bing Haritalar hesap merkezine gidin ve tıklayın **oluşturun veya görünümünü anahtarları**:

    ![eşleme-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Kayıt anahtarı Bing oluşturur.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Bir adres (Google kullanarak) göre harita oluşturma

Aşağıdaki örnek, bir adresini temel alan bir haritası işleyen bir sayfa oluşturma işlemi gösterilmektedir. Bu örnekte, Google haritalar kullanma işlemini gösterir.

1. Adlı bir dosya oluşturun *MapAddress.cshtml* sitenin kök. Bu sayfayı, kendisine geçirdiğiniz bir adresini temel alarak bir harita oluşturur.
2. Aşağıdaki kod, var olan içeriğin üzerine dosyasına kopyalayın.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Sayfanın aşağıdaki özelliklere dikkat edin:

    - `<script>` Öğesinde `<head>` öğesi. Örnekte, `<script>` öğesi başvuruları *jquery 1.6.4.min.js* jQuery kitaplığı sürüm 1.6.4 küçültülmüş (sıkıştırılmış) sürümü olan dosyası. Başvuru varsaydığını unutmayın *.js* dosyası *betikleri* sitenizin klasör. 

        > [!NOTE]
        > JQuery kitaplığı farklı bir sürümü kullanıyorsanız, yalnızca, bu sürüm için doğru işaret eden emin emin olun.
    - Çağrı `@Maps.GetGoogleHtml` sayfanın gövdesindeki. Bir adresi eşlemek için bir adres dize geçmesi gerekir. Benzer şekilde, diğer bir harita alt yapılarının yöntemler çalışır (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
3. Sayfayı çalıştırın ve bir adres girin. Sayfa, belirttiğiniz konuma gösterir Google haritalar üzerinde temel bir harita, görüntüler.

     ![eşleme-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Enlem ve boylam temelli harita oluşturma (Bing kullanarak) koordine eden

Bu örnek, bir harita koordinatlarına göre oluşturma işlemi gösterilmektedir. Bu örnek, Bing Haritalar'ı kullanmayı ve Bing anahtarınızı nasıl eklendiğini gösterir. (Bir harita altyapıları ayrıca Bing anahtarı kullanmadan koordinatları temel bir harita oluşturabilirsiniz.)

1. Adlı bir dosya oluşturun *MapCoordinates.cshtml* kök site ve aşağıdaki kodu ve biçimlendirmeyi mevcut içerikle değiştirin:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Değiştirin `your-key-here` daha önce oluşturulan Bing Haritalar anahtarını ile.
3. Çalıştırma *MapCoordinates.cshtml* sayfasında, enlem ve boylam koordinatları girin ve ardından **Map It!** düğmesi. (Tüm koordinatları bilmiyorsanız, aşağıdakileri deneyin. Microsoft Redmond kampüs konumunda budur.)

   - Enlem: 47.6781005859375
   - Boylam:-122.158317565918

     Belirttiğiniz koordinatlar kullanarak sayfa görüntülenir.

     ![eşleme-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


[Microsoft.Maps API Başvurusu](https://msdn.microsoft.com/library/gg427611.aspx)
