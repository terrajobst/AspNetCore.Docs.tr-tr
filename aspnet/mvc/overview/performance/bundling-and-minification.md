---
uid: mvc/overview/performance/bundling-and-minification
title: Paketleme ve küçültme | Microsoft Docs
author: Rick-Anderson
description: Paketleme ve küçültme olan iki teknik isteği yükleme süresini kısaltmak için ASP.NET 4.5 içinde kullanabilirsiniz. Paketleme ve küçültme reducin tarafından yükleme süresini artırır...
ms.author: riande
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 9b627a66007aec09a404147698e2bef06c7e7794
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577723"
---
<a name="bundling-and-minification"></a>Paketleme ve küçültme
====================
Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Paketleme ve küçültme olan iki teknik isteği yükleme süresini kısaltmak için ASP.NET 4.5 içinde kullanabilirsiniz. Paketleme ve küçültme sunucuya istek sayısını azaltarak ve istenilen varlıkların (örneğin, CSS ve JavaScript.) boyutunu azaltarak yükleme süresini artırır


Geçerli bilinen tarayıcılar çoğunu sayısını sınırlar [eşzamanlı bağlantı](http://www.browserscope.org/?category=network) altı için her bir ana bilgisayar başına. Altı istekler işlendiği sırada bir konakta varlıkları için ek istekler tarayıcı tarafından sıralanacak anlamına gelir. Aşağıdaki görüntüde, IE F12 geliştirici araçları ağı sekmeleri varlıklar hakkında bir örnek uygulamanın görünüm tarafından gerekli zamanlamasını gösterir.

![B/M](bundling-and-minification/_static/image1.png)

Gri çubuklar isteği altı bağlantı sınırı bekleyen tarayıcı tarafından sıraya zamanı gösterir. İstek süresi ilk bayta sarı çubuk olduğunu diğer bir deyişle, sunucudan ilk yanıtı alıp isteği göndermek için geçen süre. Mavi Çubuklar yanıt verileri sunucudan almak için geçen süreyi gösterir. Ayrıntılı zamanlama bilgileri almak için bir varlık dosyasına çift tıklayabilirsiniz. Örneğin, aşağıdaki görüntüde yüklenmesi için zamanlama ayrıntılarını gösterir */Scripts/MyScripts/JavaScript6.js* dosya.

![](bundling-and-minification/_static/image2.png)

Önceki gösterildiği **Başlat** olay, hangi isteği tarayıcının nedeniyle sıraya alınma zamanı sağlar, eşzamanlı bağlantı sayısını sınırlar. Bu durumda, istek tamamlamak başka bir isteği beklerken 46 milisaniye için kuyruğa alındı.

## <a name="bundling"></a>Paketleme

Paketleme, birleştirme veya birden çok dosyayı tek bir dosyada paket kolaylaştırır ASP.NET 4.5 içinde yeni bir özelliktir. CSS, JavaScript ve diğer paketleri oluşturabilirsiniz. Daha az dosya anlamına gelir daha az HTTP isteklerini ve, ilk sayfa yükleme performansı artırabilir.

Aşağıdaki görüntüde, daha önce ancak bu sefer paketleme ve küçültme etkin hakkında görünümünün aynı zamanlama görünümünü gösterir.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Küçültme

Küçültme betikleri veya gereksiz boşluk ve açıklamaları kaldırma ve bir karakter için değişken adlarını kısaltmak gibi css, farklı kod iyileştirmeleri çeşitli gerçekleştirir. Aşağıdaki JavaScript işlevi göz önünde bulundurun.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Küçültme sonra işlevi aşağıdaki azaltılır:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Açıklamalar ve gereksiz boşluk kaldırma ek olarak, aşağıdaki parametreler ve değişken adları (kısaltılmış) şu şekilde değiştirilmiştir:

| **Özgün** | **Yeniden adlandırıldı** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | ı |

## <a name="impact-of-bundling-and-minification"></a>Etkisini paketleme ve küçültme

Aşağıdaki tabloda, tek tek tüm varlıklarını listeleme ve paketleme ve küçültme (B/dk) örnek program kullanarak arasında bazı önemli farklılıklar gösterir.

|  | **B/dk kullanma** | **B/dk** | **Değişiklik** |
| --- | --- | --- | --- |
| **Dosya istekleri** | 9 | 34 | 256% |
| **Gönderilen KB** | 3.26 | 11.92 | 266% |
| **Alınan KB** | 388.51 | 530 | 36% |
| **Yükleme süresi** | 510 MS | 780 MS | 53% |

Gönderilen bayt tarayıcılar bunlar üzerinde istekleri geçerli HTTP üst bilgilerini oldukça ayrıntılı olarak paketleme ile önemli bir azalma var. Alınan bayt azaltma kadar büyük değil çünkü en büyük dosyaları (*betikleri\\jquery kullanıcı arabirimi 1.8.11.min.js* ve *betikleri\\jquery 1.7.1.min.js*) önceden küçültülmüş . Not: Zamanlamaları kullanılan örnek program [Fiddler](http://www.fiddler2.com/fiddler2/) yavaş bir ağa benzetmek için aracı. (Fiddler gelen **kuralları** menüsünde **performans** ardından **benzetimini Modem hızlarını**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Paketlenmiş ve küçültülmüş JavaScript hata ayıklama

Bir geliştirme ortamı, JavaScript'te hata ayıklamak kolaydır (burada [derleme öğesi](https://msdn.microsoft.com/library/s10awwz0.aspx) içinde *Web.config* dosya kümesine `debug="true"` ) JavaScript dosyaları değil gruplandığından veya küçültülmüş. JavaScript dosyalarınızın nerede toplanmış ve küçültülmüş yayın derlemesinde hata ayıklama yapabilirsiniz. IE F12 geliştirici araçlarını kullanarak, aşağıdaki yaklaşımı kullanarak bir küçültülmüş pakete eklenen bir JavaScript işlevi hata ayıklama:

1. Seçin **betik** sekmesini seçip **hata ayıklamayı Başlat** düğmesi.
2. Varlıklar düğmesini kullanarak hata ayıklamak istediğiniz JavaScript işlevi içeren bir paket seçin.  
    ![](bundling-and-minification/_static/image4.png)
3. Küçültülmüş JavaScript seçerek biçimlendirme **Yapılandırma düğmesi** ![](bundling-and-minification/_static/image5.png)seçip **biçim JavaScript**.
4. İçinde **arama betik** giriş kutusunda, hata ayıklamak istediğiniz işlevin adını seçin. Aşağıdaki görüntüde, **AddAltToImg** girilen **arama betik** giriş kutusu.  
    ![](bundling-and-minification/_static/image6.png)

F12 geliştirici araçları ile hata ayıklama ile ilgili daha fazla bilgi için bkz. MSDN makalesi [JavaScript hatalarını hata ayıklama için F12 geliştirici araçlarını kullanma](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Denetleme paketleme ve küçültme

Paketleme ve küçültme etkin veya devre dışı hata ayıklama özniteliğinin değerini ayarlayarak [derleme öğesi](https://msdn.microsoft.com/library/s10awwz0.aspx) içinde *Web.config* dosya. Aşağıdaki XML'de `debug` bunu true olarak ayarlanırsa, paketleme ve küçültme devre dışı bırakıldı.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Paketleme ve küçültme etkinleştirmek için ayarlayın `debug` "false" değeri. Geçersiz kılabilirsiniz *Web.config* ayarı `EnableOptimizations` özelliği `BundleTable` sınıfı. Aşağıdaki kod, paketleme ve küçültme sağlar ve içindeki herhangi bir ayarı geçersiz kılar *Web.config* dosya.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> Sürece `EnableOptimizations` olduğu `true` veya hata ayıklama özniteliğinde [derleme öğesi](https://msdn.microsoft.com/library/s10awwz0.aspx) içinde *Web.config* dosya kümesine `false`, dosyaları paketlenebilir veya küçültülmüş. Buna ek olarak, dosyaların .min sürümünü kullanılmayacak, tam hata ayıklama sürümleri seçilir. `EnableOptimizations` debug özniteliği geçersiz kılar [derleme öğesi](https://msdn.microsoft.com/library/s10awwz0.aspx) içinde *Web.config* dosyası


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Kullanarak paketleme ve küçültme ile ASP.NET Web Forms ve Web sayfaları

- Web sayfaları için blog girişine bakın [ekleyerek Web iyileştirme için bir Web sayfaları sitesinde](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- Web formları için blog girişine bakın [ekleme'ı paketleme ve küçültme Web Forms'a](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Kullanarak paketleme ve küçültme ile ASP.NET MVC

Bu bölümde bir ASP.NET MVC paketleme incelemek için proje ve küçültme oluşturacağız. İlk olarak adlandırılan yeni bir ASP.NET MVC internet projesi oluşturun **MvcBM** herhangi birini varsayılan değerleri değiştirmeden.

Açık *uygulama\\\_Başlat\\BundleConfig.cs* inceleyin ve dosya `RegisterBundles` oluşturmak, kaydedin ve paketleri yapılandırmak için kullanılan yöntem. Aşağıdaki kod bir bölümü gösterilmektedir `RegisterBundles` yöntemi.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Yukarıdaki kod adlı yeni bir JavaScript paket oluşturur *~/bundles/jquery* tüm uygun içeren (hata ayıklama veya küçültülmüş değil. *vsdoc*) dosyalarını *betikleri* "~/Scripts/jquery-{version} .js" joker karakter dizesi ile eşleşen bir klasör. ASP.NET MVC 4 için bu dosyanın bir hata ayıklama yapılandırması ile anlamına gelir *jquery 1.7.1.js* pakete eklenecek. Bir sürüm yapılandırmasında *jquery 1.7.1.min.js* eklenir. Paketleme framework gibi birkaç genel kurallar aşağıdaki gibidir:

- ".Min" dosya seçmek için sürümün *FileX.min.js* ve *FileX.js* mevcut.
- Hata ayıklama olmayan ".min" sürümü seçebilirsiniz.
- Yoksayma "-vsdoc" dosyaları (gibi *jquery 1.7.1 vsdoc.js*), IntelliSense tarafından kullanılır.

`{version}` Yukarıda gösterilen joker eşleşen jQuery paket jQuery'nün uygun sürümüne otomatik olarak oluşturmak için kullanılır, *betikleri* klasör. Bu örnekte, bir joker kullanarak aşağıdaki avantajları sağlar:

- Yukarıdaki paket kod veya Görünüm sayfalarınızda jQuery başvuruları değiştirmeden jQuery bir sürüme güncelleştirmek için NuGet kullanmanıza olanak tanır.
- Otomatik olarak seçer, hata ayıklama yapılandırmaları için tam sürümü ve yayını için ".min" sürümü oluşturur.

## <a name="using-a-cdn"></a>CDN kullanma

 İzleme kodu yerel jQuery paket CDN jQuery Paketle değiştirir.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

Yukarıdaki kod sürümde modu ve jQuery hata ayıklama sürümünü yerel olarak hata ayıklama modunda getirileceği sırada jQuery CDN'den istenecektir. CDN istek başarısız olursa bir CDN kullanarak, bir geri dönüş mekanizması olmalıdır. Aşağıdaki biçimlendirmede jQuery CDN hatasında istemek için eklenen Düzen dosyası gösterir betiği sonundan bölün.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Bir paket oluşturma

[Paket](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) sınıfı `Include` yöntemi her dize kaynak sanal yolu olduğu bir dizisini alır. Aşağıdaki kodu `RegisterBundles` yönteminde *uygulama\\\_Başlat\\BundleConfig.cs* dosyasını nasıl birden çok dosya bir pakete eklenen gösterir:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

[Paket](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) sınıfı `IncludeDirectory` yöntemi, bir arama deseniyle eşleşen tüm dosyaları bir dizin (ve isteğe bağlı olarak tüm alt dizinleri) eklemek için sağlanır. [Paket](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) sınıfı `IncludeDirectory` API aşağıda gösterilmektedir:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Paketler oluşturma yöntemini kullanarak görünümlerde başvurulan (`Styles.Render` için CSS ve `Scripts.Render` JavaScript için). Aşağıdaki biçimlendirmeden *görünümleri\\paylaşılan\\\_Layout.cshtml* dosya, varsayılan ASP.NET internet proje görünümleri CSS ve JavaScript paketleri nasıl başvuru gösterir.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

İşleme yöntemlerini bir kod satırında birden çok paket ekleyebilmek dizelerden oluşan bir dizi sağladığına dikkat edin. Genellikle, varlığa başvurmak için gereken HTML'yi oluşturur işleme yöntemlerini kullanmayı isteyeceksiniz. Kullanabileceğiniz `Url` varlığa başvurmak için gerekli biçimlendirme varlık için URL'yi oluşturmak için yöntemi. Yeni HTML5 kullanmayı istiyorlardı varsayalım [zaman uyumsuz](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) özniteliği. Aşağıdaki kodu kullanarak modernizr yapmayı gösterir `Url` yöntemi.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Kullanarak "\*" dosyalar seçmek için joker karakter

Belirtilen sanal yola `Include` yöntemi ve arama deseni olarak `IncludeDirectory` yöntemi bir kabul edebilir "\*" joker karakter olarak bir ön ek veya son yol kesimi için soneki. Arama dizesi büyük/küçük harf ve büyük harflere duyarlı değildir. `IncludeDirectory` Yöntemi alt dizinleri arama seçeneği vardır.

Bir proje şu JavaScript dosyaları göz önünde bulundurun:

- *Betikleri\\ortak\\AddAltToImg.js*
- *Betikleri\\ortak\\ToggleDiv.js*
- *Betikleri\\ortak\\ToggleImg.js*
- *Betikleri\\ortak\\Abon1\\ToggleLinks.js*

![imag dizini](bundling-and-minification/_static/image7.png)

Aşağıdaki tabloda gösterildiği gibi joker karakter kullanarak bir pakete eklenen dosyalar gösterilmektedir:

| **Çağrı** | **Eklenen dosyaları veya özel durum oluşturuldu** |
| --- | --- |
| İçerir ("~/Scripts/Common/\*.js") | *AddAltToImg.js*, *ToggleDiv.js*, *ToggleImg.js* |
| İçerir ("~/Scripts/Common/T\*.js") | Deseni geçersiz özel durum. Joker karakter yalnızca ön ek veya sonek izin verilir. |
| İçerir ("~/Scripts/Common/\*og.\*") | Deseni geçersiz özel durum. Yalnızca bir joker karaktere izin verilir. |
| İçerir ("~/Scripts/Common/T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| İçerir ("~/Scripts/Common/\*") | Deseni geçersiz özel durum. Saf joker karakter segmenti geçerli değil. |
| IncludeDirectory ("~/Scripts/Common", "T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| IncludeDirectory ("~/Scripts/Common", "T\*", true) | *ToggleDiv.js*, *ToggleImg.js*, *ToggleLinks.js* |

Açıkça bir pakete her dosya ekleme genellikle tercih edilir joker yükleme dosyalarının aşağıdaki nedenlerle üzerinde:

- Betikleri, genellikle değil istediğiniz alfabetik sırada yüklenmesi için joker karakter Varsayılanları tarafından ekleniyor. Sık CSS ve JavaScript dosyaları (alfabetik olmayan) belirli bir sırayla eklenmesi gerekir. Özel bir ekleyerek bu riski azaltmak [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) uygulaması, ancak her dosya, daha az hata yapmaya açık olduğu açıkça ekleme. Örneğin, değiştirmenizi gerektirebilir gelecekte bir klasöre yeni varlıkları ekleyebilir, [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) uygulaması.
- Bu paket başvuran tüm görünümlerde yüklenirken joker kullanarak bir dizine eklenen belirli dosyaları görüntüle eklenebilir. Betiği görüntüle belirli bir pakete eklenirse, paket başvuran diğer görünümlerde bir JavaScript hatası alabilirsiniz.
- İçeri aktarılan dosyaları iki kez yüklenen diğer dosyaları içeri aktarma CSS dosyaları neden. Örneğin, aşağıdaki kod iki kez yüklenen jQuery kullanıcı Arabirimi teması CSS dosyaları çoğunu bir paket oluşturur. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  Joker kart seçiciye "\*.css" klasöründeki her bir CSS dosyasına getirir dahil olmak üzere *içerik\\Temalar\\temel\\jquery.ui.all.css* dosya. *Jquery.ui.all.css* dosyası diğer CSS dosyaları alır.

## <a name="bundle-caching"></a>Paket önbelleğe alma

Paketler, paket ne zaman oluşturulur, bir yıl Expires HTTP üstbilgisini ayarlayın. IE, paket için bir koşullu isteği yapmaz Fiddler gösterir daha önce görüntülenen bir sayfaya geçerseniz diğer bir deyişle, hiçbir IE paketleri için gelen HTTP GET istekleri ve vardır sunucusundan gelen HTTP 304 yanıt yok. F5 tuşuna (her paket için bir HTTP 304 yanıt kaynaklanan) ile her paket için koşullu istekte bulunmak için IE zorlayabilirsiniz. Kullanarak tam yenilemeye zorlayabilirsiniz ^ F5 (her paket için bir HTTP 200 yanıtı olmasıyla sonuçlanır.)

Aşağıdaki görüntüde **önbelleğe alma** Fiddler yanıt bölmesinin sekmesinde:

![fiddler önbelleğe alma görüntüsü](bundling-and-minification/_static/image8.png)

İstek   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 paket için olan **AllMyScripts** ve bir sorgu dizesi çifti içeren **v r0sLDicvP58AIXN =\\\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. Sorgu dizesi **v** diğer bir deyişle önbelleğe almak için kullanılan benzersiz bir tanımlayıcı belirteç bir değere sahip. Paket değişmez sürece, ASP.NET uygulamasının isteyecek **AllMyScripts** Bu belirteci kullanarak paket. Paket herhangi bir dosyayı değiştirirse, ASP.NET iyileştirme çerçevesi tarayıcı istekleri paket için en son paket alırsınız güvence altına alır, yeni bir belirteç oluşturur.

IE9 F12 geliştirici araçlarını çalıştırma ve daha önce yüklenen bir sayfaya gidin, IE yanlış koşullu GET istekleri için her paket ve HTTP 304 döndüren sunucusu yapılan gösterir. IE9 koşullu bir istek blog girişi yapılmışsa belirleme sorunlara neden olduğunu okuyabilirsiniz [kullanarak CDN ve Web sitesi performansını geliştirmek için Expires](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>Daha az, CoffeeScript, SCSS, Sass paketleme.

Paketleme ve küçültme framework Ara diller gibi işlemek için bir mekanizma sağlar [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [daha az](http://www.dotlesscss.org/) veya [Coffeescript ](http://coffeescript.org/)ve elde edilen paket için dönüşümler küçültme gibi uygulayabilirsiniz. Örneğin, eklemek için [.less](http://www.dotlesscss.org/) MVC 4 proje dosyaları:

1. Daha az içeriğiniz için bir klasör oluşturun. Aşağıdaki örnekte *içerik\\MyLess* klasör.
2. Ekleme [.less](http://www.dotlesscss.org/) NuGet paketini **noktasız** projenize.  
    ![NuGet noktasız yükleme](bundling-and-minification/_static/image9.png)
3. Uygulayan bir sınıf ekleyin [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) arabirimi. .Less dönüştürme için projenize aşağıdaki kodu ekleyin.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Daha az dosya içeren bir paket oluşturma `LessTransform` ve [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) dönüştürün. Aşağıdaki kodu ekleyin `RegisterBundles` yönteminde *uygulama\\_başlatmayı\\BundleConfig.cs* dosya.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Daha az paket başvuran tüm görünümler için aşağıdaki kodu ekleyin.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Paket konuları

Paketleri oluştururken izlemek için iyi bir kuralı "paketi" Paket adı ön eki olarak eklemektir. Bu olası bir engeller [yönlendirme çakışma](https://forums.asp.net/post/5012037.aspx).

Bir paketteki tek dosya güncelleştirdikten sonra paket sorgu dizesi parametresi için yeni bir belirteç oluşturulur ve bir istemci paketi içeren bir sayfa istediğinde tam paket indirilmelidir. Burada tek tek her varlık listelenen geleneksel işaretlemede yalnızca değiştirilen dosya indirilecek. Sık sık değişen varlıklar, paketleme için iyi adaylar olabilir.

Paketleme ve küçültme öncelikle ilk sayfa isteği yükleme süresi geliştirmek. Tarayıcı varlıkları (JavaScript, CSS ve görüntü) önbelleğe alır, bu bir Web sayfası istendi sonra paketleme ve küçültme olmaz sağlayan herhangi bir performans artışının aynı sayfa isteğinde bulunurken ya da site sayfaları aynı aynı varlıkları isteniyor. Ayarlamazsanız varlıklarınızı başlığındaki doğru süresi dolar ve paketleme ve küçültme kullanmayın, tarayıcılar güncellik buluşsal yöntemler varlıkları eski birkaç gün sonra işaretler ve tarayıcı her varlık için bir doğrulama isteği gerektirir. Bu durumda, paketleme ve küçültme sonra ilk sayfa isteği bir performans artışı sağlar. Ayrıntılar için blog bkz [kullanarak CDN ve Web sitesi performansını geliştirmek için süre sonu](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

Her bir ana bilgisayar başına altı eşzamanlı bağlantı tarayıcı SORUMLULUĞUN kullanılarak azaltılabilir bir [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). CDN, barındırma sitesindeki puanlardan farklı bir ana bilgisayar adı sahip olacağından barındırma ortamınıza altı eşzamanlı bağlantı sayısı sınırı karşı varlık istekler CDN edilmez. CDN, ortak paketini önbelleğe alma ve avantajları önbelleğe alma uç de sağlayabilirsiniz.

Paketleri ihtiyaç sayfaları göre bölümlendirilmelidir. Örneğin, ' % s'varsayılan Internet uygulaması için ASP.NET MVC şablonu jQuery doğrulama paket jQuery ayrı oluşturur. Oluşturulan varsayılan görünümlere sahip herhangi bir giriş ve değerleri göndermeyin olduğundan doğrulama paket dahil değildir.

`System.Web.Optimization` Ad alanı içinde uygulanan *System.Web.Optimization.dll*. WebGrease kitaplığı yararlanır (*WebGrease.dll*) küçültme özellikleri için hangi sırayla kullandığı *Antlr3.Runtime.dll*.

*Twitter gönderileri hızlı yapıp bağlantıları paylaşın kullanıyorum. My Twitter tanıtıcısı*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Ek kaynaklar

- Video:[paketleme ve en iyi duruma getirme](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) tarafından [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Bir Web sayfaları sitesinde Web iyileştirme ekleme](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Ekleme paketleme ve küçültme Web Forms'a](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Performans etkilerini paketleme ve küçültme Web gözatma](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) tarafından [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [CDN'ler kullanarak Web sitesi performansını geliştirmek için dolmadan](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) tarafından Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [RTT (gidiş geliş zaman) simge durumuna küçült](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Katkıda Bulunanlar

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
