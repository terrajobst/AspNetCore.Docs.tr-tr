---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: "Video görüntüleme bir ASP.NET Web sayfaları (Razor) Site | Microsoft Docs"
author: tfitzmac
description: "Bu bölümde, bir ASP.NET Web Pages'de Razor sözdizimi sayfasıyla video görüntülemek açıklanmaktadır."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 0e1849fb780908b55520d8108e2227d046759987
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Bir ASP.NET Web sayfaları (Razor) sitesinde video görüntüleme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir video (medya) oynatıcı bir ASP.NET Web sayfaları (Razor) Web sitesinde depolanan videoları izleyin, kullanıcıların izin için nasıl kullanılacağı açıklanmaktadır. Razor sözdizimi ile ASP.NET Web sayfaları Flash yürütmek olanak tanır (*.swf*), Media Player (*.wmv*) ve Silverlight (*.xap*) videolar.
> 
> Öğrenecekleriniz:
> 
> - Bir video oynatıcı seçmek nasıl.
> - Video bir web sayfasına ekleme.
> - Video oynatıcı öznitelikleri nasıl kurulur.
> 
> ASP.NET Razor bunlar sayfaları makalesinde sunulan özellikler:
> 
> - `Video` Yardımcısı.
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


## <a name="introduction"></a>Giriş

Sitenizdeki bir video görüntülemek isteyebilirsiniz. Bunu yapmak için bir video YouTube'da gibi zaten olan bir siteye bağlamak için yoludur. Kendi sayfaları doğrudan bu sitelerden bir videoyu katıştırmak istiyorsanız, genellikle HTML biçimlendirmesi sitesinden almak ve sayfanıza kopyalayın. Örneğin, aşağıdaki örnekte bir YouTube katıştırmak video gösterilmektedir:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Kendi Web sitesi (olmayan bir sitede genel video paylaşımı) açık bir video oynatmak istiyorsanız, doğrudan şöyle katıştırılmış biçimlendirme kullanarak kendisine bağlayamazsınız. Ancak, videolar sitenizden kullanarak yürütebilirsiniz `Video` media player doğrudan sayfasındaki işler Yardımcısı.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Bir Video Oynatıcı seçme

Çok sayıda video dosyaları için biçimleri vardır ve her biçimi, genellikle farklı bir yürütücü ve player yapılandırmak için farklı bir şekilde gerektirir. ASP.NET Razor sayfalarında, bir web sayfasını kullanarak bir video yürütebilirsiniz `Video` Yardımcısı. `Video` Yardımcı otomatik olarak oluşturması nedeniyle bir web sayfasında videolar katıştırma sürecini basitleştirir `object` ve `embed` normalde video eklemek için kullanılan HTML öğeleri.

`Video` Yardımcı aşağıdaki medya oynatıcıları destekler:

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>Flash Player

`Flash` Oynatıcı `Video` yardımcı Flash videoları oynat olanak tanır (*.swf* dosyaları) bir web sayfasında. En azından bir video dosyası yolunu girmeniz gerekir. Yolun ancak hiçbir şey belirtirseniz, player Flash geçerli sürümü tarafından belirlenen varsayılan değerleri kullanır. Genel varsayılan ayarlar şunlardır:

- Video varsayılan genişlik ve yükseklik kullanılarak görüntülenir ve olmadan bir arka plan rengi.
- Sayfa yüklendiğinde video otomatik olarak oynatılır.
- Sürekli olarak açıkça durdurulana kadar video döngüye girer.
- Video tümünü belirli bir boyuta sığması için video kırpma yerine video gösterecek şekilde ölçeklendirilir.
- Video bir pencerede oynatılır.

### <a name="the-mediaplayer-player"></a>MediaPlayer Player

`MediaPlayer` Oynatıcı `Video` yardımcı Windows Media videoları oynat olanak tanır (*.wmv* dosyaları), Windows Media Ses (*.wma* dosyaları) ve MP3 (*.mp3* bir web sayfasında dosyaları). Yürütmek için medya dosyasının yolunu içermelidir; diğer tüm parametreler isteğe bağlıdır. Yalnızca bir yol belirtirseniz, player MediaPlayer, geçerli sürümü tarafından ayarlamak varsayılan ayarları kullanır:

- Video varsayılan genişlik ve yükseklik kullanılarak görüntülenir.
- Sayfa yüklendiğinde video otomatik olarak oynatılır.
- Video kez çalınır (döngü değil).
- Player kullanıcı arabiriminde denetimleri kümesini görüntüler.
- Bir pencerede video oynadığı.

### <a name="the-silverlight-player"></a>Silverlight Player

`Silverlight` Oynatıcı `Video` yardımcı Windows Media çalmasına olanak tanır (*.wmv* dosyaları), Windows Media Ses (*.wma* dosyaları) ve MP3 (*.mp3* dosyaları). Path parametresi bir Silverlight tabanlı bir uygulama paketi işaret edecek şekilde ayarlamanız gerekir (*.xap* dosyası). Width ve height parametreleri de ayarlamanız gerekir. Diğer tüm parametreler isteğe bağlıdır. Video için Silverlight player kullandığınızda, yalnızca gerekli parametreleri ayarlarsanız Silverlight player arka plan rengi olmadan videoyu görüntüler.

> [!NOTE]
> Silverlight zaten tanımadığınız durumda: *.xap* dosyasıdır düzeni yönergeleri içeren bir sıkıştırılmış dosyayı bir *.xaml* dosya, yönetilen kodda derlemeler ve isteğe bağlı kaynaklar. Oluşturabileceğiniz bir *.xap* bir Silverlight uygulaması projesi olarak Visual Studio dosyasında.


`Silverlight` Video oynatıcı kullanan iki player için sağladığınız ayarları ve içinde sağlanan ayarları *.xap* dosya.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>MIME türleri
> 
> Bir tarayıcı bir dosyayı açtığında, tarayıcı dosya türü için işlenen belge belirtilen MIME türü eşleşmesini sağlar. MIME türü içerik türü veya medya dosyasının türüdür. `Video` Yardımcı aşağıdaki MIME türlerini kullanır:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Flash (.swf) video oynatma

Bu yordamda adlı bir Flash çalmasına gösterilmiştir *sample.swf*. Adlı bir klasör olduğuna yordam varsayar *medya* siteniz ve, *.swf* o klasördeki bir dosyadır.

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), onu zaten eklemediniz.
2. Web sitesi, bir sayfa ekleyin ve adını *FlashVideo.cshtml*.
3. Aşağıdaki biçimlendirmede sayfasına ekleyin: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Bir tarayıcıda. Sayfayı çalıştırın. (Emin olun sayfa seçildiğinde, **dosyaları** çalıştırmadan önce onu çalışma.) Sayfası görüntülenir ve video otomatik olarak yürütülür. 

    ![[Görüntü] ] (10-working-with-video/_static/image1.jpg "ch08_video 1.jpg")

Ayarlayabileceğiniz `quality` parametresi için bir Flash video `low`, `autolow`, `autohigh`, `medium`, `high`, ve `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Bir özel boyutu kullanarak yürütmek için Flash video değiştirebilirsiniz `scale` şu şekilde ayarlayabilirsiniz parametre:

- `showall`. Bu videonun tamamını görünür özgün en boy oranını koruyarak yapar. Bununla birlikte, her iki taraftaki Kenarlıklar şunun.
- `noorder`. Özgün en boy oranını korurken bu videoyu ölçeklendirir ancak kırpılmış.
- `exactfit`. Bu videonun tamamını görünür özgün en boy oranını koruyarak olmadan sağlar, ancak bozulma oluşabilir.

Belirtmediyseniz bir `scale` parametresi, tüm videoyu görünür olur ve özgün en boy oranını hiçbir kırpma olmadan korunur. Aşağıdaki örnekte nasıl kullanılacağını gösterir `scale` parametre:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Görüntü modu adlandırılmış ayarı Flash player destekleyen `windowMode`. Bu ayar `window`, `opaque`, ve `transparent`. Varsayılan olarak, `windowMode` ayarlanır `window`, web sayfasında ayrı bir pencerede video görüntüleyen. `opaque` Ayar her şeyi web sayfasında video arkasına gizler. `transparent` Ayarı, video, herhangi bir bölümünü saydam olduğunu varsayarak video Göster arka plan web sayfasının olanak sağlar.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>MediaPlayer çalma (*.wmv*) videolar

Aşağıdaki yordamda adlı bir pencere medya çalmasına gösterilmiştir *sample.wmv* alanında *medya* klasör.

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.
2. Adlı yeni bir sayfa oluşturma *MediaPlayerVideo.cshtml*.
3. Aşağıdaki biçimlendirmede sayfasına ekleyin: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Bir tarayıcıda. Sayfayı çalıştırın. Video yükler ve otomatik olarak yürütülür. 

    ![[Görüntü] ] (10-working-with-video/_static/image2.jpg "ch08_video 2.jpg")

Ayarlayabileceğiniz `playCount` otomatik olarak çalmasına kaç kez belirten bir tamsayı için:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

`uiMode` Parametresi hangi denetimlerin kullanıcı arabiriminde gösterilmesi belirtmenize olanak sağlar. Ayarlayabileceğiniz `uiMode` için `invisible`, `none`, `mini`, veya `full`. Belirtmediyseniz bir `uiMode` parametresi, video olacaktır durum penceresi görüntülenir, arama çubuğunda, düğmeler ve ses denetimlerini video penceresinin yanı sıra denetim. Bir ses dosyasını oynatmak için player kullanırsanız, bu denetimlerin da görüntülenir. İşte nasıl kullanılacağına ilişkin bir örnek `uiMode` parametre:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Video yürüttüğünde varsayılan olarak, ses açıktır. Ayarlayarak sesi `mute` parametresi true olarak:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Ayarlayarak MediaPlayer video ses düzeyini denetleyebilirsiniz `volume` parametresi için 0 ile 100 arasında bir değer. Varsayılan değer 50'dir. Örnek buradadır:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Silverlight video oynatma

Bu yordamda yer alan bir Silverlight çalmasına gösterilmiştir *.xap* bir klasörde adlı sayfa *medya*.

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.
2. Adlı yeni bir sayfa oluşturma *SilverlightVideo.cshtml*.
3. Aşağıdaki biçimlendirmede sayfasına ekleyin: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Bir tarayıcıda. Sayfayı çalıştırın. 

    ![[Görüntü] ] (10-working-with-video/_static/image3.jpg "ch08_video 3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


[Silverlight genel bakış](https://msdn.microsoft.com/en-us/library/bb404700(VS.95).aspx)

[Nesne ve EMBED etiket özniteliklerini flash](http://kb2.adobe.com/cps/127/tn_12701.html)

[Windows Media Player 11 SDK PARAM etiketleri](https://msdn.microsoft.com/en-us/library/aa392321(VS.85).aspx)
