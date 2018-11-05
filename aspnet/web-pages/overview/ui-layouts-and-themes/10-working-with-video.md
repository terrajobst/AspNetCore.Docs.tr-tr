---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Video görüntüleme bir ASP.NET Web sayfaları (Razor) sitesinde | Microsoft Docs
author: Rick-Anderson
description: Bu bölümde, bir ASP.NET Web Pages'de Razor söz dizimi sayfası ile videoyu görüntülemek açıklanmaktadır.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 8f4b7186ae5c7b7b384ebcb23f7c9ad65caeb0bd
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021046"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitesinde video görüntüleme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, kullanıcıların sitesinde depolanan videosu görüntüleyin izin vermek için bir ASP.NET Web sayfaları (Razor) Web sitesinde (ortam) video oynatıcı kullanmayı açıklar. Razor sözdizimi olan ASP.NET Web sayfaları Flash play olanak tanır (*.swf*), Media Player (*.wmv*) ve Silverlight (*.xap*) videolar.
> 
> Öğrenecekleriniz:
> 
> - Bir video oynatıcı nasıl seçeceğinizi öğrenin.
> - Video bir web sayfasına ekleme.
> - Nasıl video oynatıcı özniteliklerini ayarlayın.
> 
> Bunlar, ASP.NET Razor sayfaları makalesinde sunulan özellikleri:
> 
> - `Video` Yardımcısı.
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


## <a name="introduction"></a>Giriş

Sitenizde bir videoyu görüntülemek isteyebilirsiniz. Bunu yapmanın bir yolu gibi YouTube video zaten bir siteye bağlamaktır. Kendi sayfaları doğrudan bu sitelerden video eklemek istiyorsanız, genellikle bir HTML biçimlendirmesi sitesinden almak ve sayfanıza kopyalayın. Örneğin, aşağıdaki örnekte bir YouTube ekleme video gösterir:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Kendi Web (değil bir sitede genel bir video paylaşım) olan bir videoyu oynatmak istiyorsanız, doğrudan katıştırılmış biçimlendirme şunun gibi kullanarak kendisine bağlayamazsınız. Sitenizden kullanarak videoları ancak oynatabilen `Video` bir medya oynatıcı sayfasındaki doğrudan işleyen Yardımcısı.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Bir Video Oynatıcı seçme

Video dosyaları için biçimler çok sayıda vardır ve her biçim genellikle farklı bir yürütücü ve oyuncu yapılandırmak için farklı bir yol gereklidir. ASP.NET Razor sayfaları'nda bir görüntü kullanarak bir web sayfası yürütebilirsiniz `Video` Yardımcısı. `Video` Yardımcı otomatik olarak oluşturduğundan bir web sayfasında videoları ekleme işlemini basitleştiren `object` ve `embed` normalde video eklemek için kullanılan HTML öğeleri.

`Video` Yardımcısı aşağıdaki medya oynatıcıları destekler:

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>Flash Player

`Flash` Oyuncusu `Video` Yardımcısı, Flash videoları oynatın olanak tanır (*.swf* dosyaları) bir web sayfasında. En az bir görüntü dosyasının yolunu sağlamanız gerekir. Yolun ancak hiçbir şey belirtirseniz, oyuncunun Flash geçerli sürümü tarafından belirlenen varsayılan değerleri kullanır. Genel varsayılan ayarlar şunlardır:

- Varsayılan genişlik ve yükseklik kullanarak video görüntülenir ve arka plan rengi.
- Sayfa yüklendiğinde videonun otomatik olarak yürütülür.
- Video, sürekli olarak açıkça durdurulana kadar döngüde kalır.
- Video tümünü belirli bir boyuta sığması için videoyu kırpma yerine video gösterecek şekilde ölçeklendirilir.
- Bir pencerede bir video yürütür.

### <a name="the-mediaplayer-player"></a>MediaPlayer Player

`MediaPlayer` Oyuncusu `Video` Yardımcısı Windows Media videoları oynatın olanak tanır (*.wmv* dosyaları), Windows Media Ses (*.wma* dosyaları) ve MP3 (*.mp3* bir web sayfasında dosyalar). Oynatmak için medya dosyasının yolunu içermelidir; diğer tüm parametreler isteğe bağlıdır. Yalnızca bir yol belirtirseniz, oyuncunun MediaPlayer, geçerli sürümü tarafından ayarlamak varsayılan ayarları kullanır:

- Video, varsayılan genişlik ve yükseklik kullanılarak görüntülenir.
- Sayfa yüklendiğinde videonun otomatik olarak yürütülür.
- Video kez çalınır (döngü değil).
- Oyuncu denetimleri kümesini kullanıcı arabiriminin görüntüler.
- Bir pencerede bir video yürütür.

### <a name="the-silverlight-player"></a>Silverlight Player

`Silverlight` Oyuncusu `Video` Yardımcısı Windows Media Video oynatın olanak tanır (*.wmv* dosyaları), Windows Media Ses (*.wma* dosyaları) ve MP3 (*.mp3* dosyaları). Path parametresi bir Silverlight tabanlı bir uygulama paketi için işaret edecek şekilde ayarlamanız gerekir (*.xap* dosyası). Genişlik ve yükseklik parametreleri de ayarlamanız gerekir. Diğer tüm parametreler isteğe bağlıdır. Video için Silverlight player kullandığınızda, yalnızca gerekli parametreler ayarlarsanız Silverlight player arka plan rengi olmadan videoyu görüntüler.

> [!NOTE]
> Silverlight zaten bilinmiyor durumunda: *.xap* dosyasıdır Düzen yönergeleri içeren bir sıkıştırılmış dosyayı bir *.xaml* derlemeleri ve isteğe bağlı kaynakları yönetilen kodu dosyası. Oluşturabileceğiniz bir *.xap* dosyasını Visual Studio'da Silverlight uygulaması projesi olarak.


`Silverlight` Video oynatıcı kullanan iki player için sağladığınız ve sağlanan ayarlarını *.xap* dosya.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>MIME türleri
> 
> Tarayıcı, bir tarayıcı bir dosya yüklediğinde, dosya türü işlenen belge için belirtilmiştir MIME türü eşleşmesini sağlar. MIME içerik türü veya medya dosyasının türünü türüdür. `Video` Yardımcısı aşağıdaki MIME türlerini kullanır:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Flash (.swf) video oynatma

Bu yordamda adlı bir Flash videoyu oynatmak gösterilmiştir *sample.swf*. Adlı bir klasör kendinizi yordam varsayar *medya* siteniz ve, *.swf* bu klasörde dosyasıdır.

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), bunu zaten eklemediniz.
2. Web sitesi, bir sayfa ekleyin ve adlandırın *FlashVideo.cshtml*.
3. Sayfaya aşağıdaki işaretlemeyi ekleyin: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Sayfanın tarayıcıda çalıştırın. (Emin sayfanın içinde seçili **dosyaları** çalıştırmadan önce çalışma alanı.) Sayfa görüntülenir ve video otomatik olarak yürütülür. 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video 1.jpg")

Ayarlayabileceğiniz `quality` parametresi için Flash video `low`, `autolow`, `autohigh`, `medium`, `high`, ve `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Bir boyutta kullanarak oynatmak için Flash video değiştirebilirsiniz `scale` parametresini aşağıdaki şekilde ayarlayabilirsiniz:

- `showall`. Bu videonun tamamını görünür özgün en boy oranını koruyarak sağlar. Bununla birlikte, her iki taraftaki kenarlıklı çıkabilir.
- `noorder`. Özgün en boy oranını koruyarak bu videoyu ölçeklenen, ancak kırpılmış.
- `exactfit`. Bu videonun tamamını görünür özgün en boy oranını koruyarak gerek kalmadan kolaylaştırır, ancak bozulma ortaya çıkabilir.

Belirtmezseniz bir `scale` parametresi, tüm video görünür olur ve herhangi bir kırpma olmadan özgün en boy oranı korunur. Aşağıdaki örnek nasıl kullanılacağını gösterir `scale` parametresi:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Flash player adlı ayar video bir modu destekler `windowMode`. Bu ayar `window`, `opaque`, ve `transparent`. Varsayılan olarak, `windowMode` ayarlanır `window`, web sayfasında ayrı bir pencerede bir video görüntüleyen. `opaque` Ayar her şeyi web sayfasındaki videoyu arkasına gizler. `transparent` Ayarı, saydam herhangi bir videonun parçası olduğunu varsayarak video içinde Göster arka plan web sayfasının olanak sağlar.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>MediaPlayer Yürütülüyor (*.wmv*) videoları

Aşağıdaki yordamda adlı bir Windows Media videoyu oynatmak gösterilmiştir *sample.wmv* alanında *medya* klasör.

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.
2. Adlı yeni bir sayfa oluşturun *MediaPlayerVideo.cshtml*.
3. Sayfaya aşağıdaki işaretlemeyi ekleyin: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Sayfanın tarayıcıda çalıştırın. Video yükler ve otomatik olarak yürütülür. 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video 2.jpg")

Ayarlayabileceğiniz `playCount` için otomatik olarak videoyu oynatmak için kaç kez gösteren bir tam sayı:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

`uiMode` Parametresi kullanıcı arabiriminin hangi denetimlerin görünmesini belirtmenize olanak sağlar. Ayarlayabileceğiniz `uiMode` için `invisible`, `none`, `mini`, veya `full`. Belirtmezseniz bir `uiMode` parametresi, videoyu olacak durum penceresi görüntülenir, arama çubuğu, düğmeler ve ses denetimlerini video penceresinin yanı sıra denetim. Bir ses dosyasını oynatmak için player'ı kullanırsanız, bu denetimleri de görüntülenir. İşte nasıl kullanılacağına ilişkin bir örnek `uiMode` parametresi:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Video yürütüldüğünde, varsayılan ses açıktır. Ayarlayarak sesi `mute` parametresi true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Ayarlayarak MediaPlayer videonun ses düzeyini denetleyebilirsiniz `volume` parametresi için 0 ile 100 arasında bir değer. Varsayılan değer 50'dir. Örnek buradadır:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Silverlight video oynatma

Bu yordamda bir Silverlight'ta bulunan bir videoyu oynatmak gösterilmiştir *.xap* bir klasörde adlı sayfa *medya*.

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.
2. Adlı yeni bir sayfa oluşturun *SilverlightVideo.cshtml*.
3. Sayfaya aşağıdaki işaretlemeyi ekleyin: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Sayfanın tarayıcıda çalıştırın. 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video 3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


[Silverlight genel bakış](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Nesne ve ekleme etiketi öznitelikleri flash](http://kb2.adobe.com/cps/127/tn_12701.html)

[Windows Media Player 11 SDK PARAM etiketleri](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
