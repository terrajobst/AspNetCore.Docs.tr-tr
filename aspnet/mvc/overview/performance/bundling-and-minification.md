---
uid: mvc/overview/performance/bundling-and-minification
title: Paketleme ve küçültme | Microsoft Docs
author: Rick-Anderson
description: Paketleme ve küçültme olan iki teknikleri isteği yükleme süresini artırmak için ASP.NET 4.5 içinde kullanabilirsiniz. Paketleme ve küçültme tarafından reducin yükleme süresini artırır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/23/2012
ms.topic: article
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 001ebf89cda66a50cddcd7e4944f27b9396d4450
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="bundling-and-minification"></a>Paketleme ve küçültme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Paketleme ve küçültme olan iki teknikleri isteği yükleme süresini artırmak için ASP.NET 4.5 içinde kullanabilirsiniz. Paketleme ve küçültme sunucuya isteklerinin sayısını azaltmak ve istenilen varlıkların (örneğin, CSS ve JavaScript.) boyutunu azaltarak yükleme süresini artırır


Geçerli ana tarayıcılarının çoğu sayısını sınırlandır [eşzamanlı bağlantı](http://www.browserscope.org/?category=network) altı her bir ana bilgisayar başına. Altı istekler işlenirken, bir ana bilgisayar üzerindeki varlıklar için ek istekler tarayıcı tarafından sıraya alınır anlamına gelir. Aşağıdaki resimde, IE F12 geliştirici araçları ağ sekmeleri örnek bir uygulama hakkında görünüm tarafından gerekli varlıklar için zamanlama gösterir.

![B/M](bundling-and-minification/_static/image1.png)

Gri çubukların isteği altı bağlantı sınırını bekleyen tarayıcı tarafından sıraya zamanı gösterir. İstek süresi ilk bayta sarı çubuğu olan başka bir deyişle, istek göndermek ve ilk yanıt sunucudan almak için geçen süre. Mavi çubukları yanıt verileri sunucudan almak için geçen süreyi gösterir. Ayrıntılı zamanlama bilgilerini almak için bir varlık çift tıklayabilirsiniz. Örneğin, aşağıdaki resimde yükleme için zamanlama ayrıntılarını gösterir */Scripts/MyScripts/JavaScript6.js* dosya.

![](bundling-and-minification/_static/image2.png)

Önceki görüntü gösterir **Başlat** hangi verir isteği nedeniyle tarayıcı sıraya zaman eşzamanlı bağlantı sayısını sınırla olay. Bu durumda, isteği tamamlamak başka bir istek için bekleniyor 46 milisaniye için sıraya alındı.

## <a name="bundling"></a>Paketleme

Paketleme, birleştirebilir veya tek bir dosyaya birden çok dosya paketini daha kolay hale getirir ASP.NET 4.5 içinde yeni bir özelliktir. CSS, JavaScript ve diğer paketleri oluşturabilirsiniz. Daha az sayıda dosya anlamına gelir daha az HTTP istekleri ve, ilk sayfa yükleme performansı artırabilir.

Aşağıdaki resimde, daha önce ancak bu kez paketleme ve küçültme etkin ile gösterilen hakkında görünüm aynı zamanlama görünümünü gösterir.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Küçültme

Küçültme farklı kod iyileştirmeler komut dosyaları veya gereksiz boşluk ve açıklamaları kaldırma ve bir karakter değişken adları kısaltmayı gibi css, çeşitli gerçekleştirir. Şu JavaScript işlevi göz önünde bulundurun.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Küçültme sonra işlevi aşağıdaki azalır:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Gereksiz boşluk ve açıklamaları kaldırma ek olarak, aşağıdaki parametreler ve değişken adları (kısaltılmış) aşağıdaki gibi yeniden adlandırıldı:

| **Özgün** | **Yeniden adlandırıldı** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | ı |

## <a name="impact-of-bundling-and-minification"></a>Etkisini paketleme ve küçültme

Aşağıdaki tablo arasındaki tüm varlıklarını ayrı ayrı listeleme paketleme ve küçültme (B/M) örnek programı kullanarak birkaç önemli farklılık gösterir.

|  | **B/M kullanma** | **B/M** | **değiştirme** |
| --- | --- | --- | --- |
| **Dosya istekleri** | 9 | 34 | 256% |
| **KB Sent** | 3.26 | 11.92 | 266% |
| **Alınan KB** | 388.51 | 530 | 36% |
| **Yükleme süresi** | 510 MS | 780 MS | 53% |

Gönderilen bayt tarayıcılar isteklerinde uygulamak HTTP üst bilgileri ile oldukça ayrıntılı olarak paketleme ile önemli ölçüde azalma vardı. Alınan bayt azaltma kadar büyük değil çünkü büyük dosyaları (*Scripts\jquery-ui-1.8.11.min.js* ve *Scripts\jquery-1.7.1.min.js*) zaten küçültülmüş. Not: Zamanlamaları kullanılan örnek program [Fiddler](http://www.fiddler2.com/fiddler2/) yavaş bir ağ benzetimini yapmak için aracı. (Fiddler gelen **kuralları** menüsünde, select **performans** sonra **benzetimini Modem hızlarını**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Hata ayıklama toplanmış ve JavaScript küçültülmüş

Bir geliştirme ortamında, JavaScript hata ayıklama kolaydır (burada [derleme öğesi](https://msdn.microsoft.com/library/s10awwz0.aspx) içinde *Web.config* dosya ayarlanmış `debug="true"` ) JavaScript dosyaları değil gruplandığından veya küçültülmüş. JavaScript dosyalarınızın nerede toplanmış ve küçültülmüş yayın derlemesinde hata ayıklaması yapabilirsiniz. IE F12 Geliştirici Araçları'nı kullanarak aşağıdaki yaklaşımı kullanarak küçültülmüş bir pakete eklenen bir JavaScript işlevi hata ayıklama:

1. Seçin **betik** sekmesini ve ardından **hata ayıklamayı Başlat** düğmesi.
2. Varlıklar düğmesini kullanarak hata ayıklamak istediğiniz JavaScript işlevi içeren paket seçin.  
    ![](bundling-and-minification/_static/image4.png)
3. Küçültülmüş JavaScript seçerek biçimlendirmek **Yapılandırma düğmesi** ![](bundling-and-minification/_static/image5.png)ve ardından seçerek **biçimi JavaScript**.
4. İçinde **arama betik** t giriş kutusuna, hata ayıklama istediğiniz işlevin adını seçin. Aşağıdaki görüntüde **AddAltToImg** girilen **arama betik** t giriş kutusu.  
    ![](bundling-and-minification/_static/image6.png)

F12 geliştirici araçları ile hata ayıklama ile ilgili daha fazla bilgi için MSDN makalesine bakın [JavaScript hataları hata ayıklamak için F12 geliştirici araçlarını kullanma](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Denetleme paketleme ve küçültme

Paketleme ve küçültme etkin veya devre dışı hata ayıklama özniteliğinin değeri ayarlayarak [derleme öğesi](https://msdn.microsoft.com/library/s10awwz0.aspx) içinde *Web.config* dosya. Aşağıdaki XML `debug` bunu true olarak ayarlanırsa, paketleme ve küçültme devre dışıdır.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Paketleme ve küçültme etkinleştirmek için ayarlanmış `debug` "false" değerine. Geçersiz kılabilirsiniz *Web.config* ayarı `EnableOptimizations` özellikte `BundleTable` sınıfı. Aşağıdaki kod paketleme ve küçültme sağlar ve herhangi bir ayar geçersiz kılmaları *Web.config* dosya.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> Sürece `EnableOptimizations` olan `true` veya hata ayıklama özniteliğinde [derleme öğesi](https://msdn.microsoft.com/library/s10awwz0.aspx) içinde *Web.config* dosya ayarlanmış `false`, dosyaları paketlenebilir veya küçültülmüş. Ayrıca, dosyaların .min sürümünü kullanılmayacak, tam hata ayıklama sürümleri seçilir. `EnableOptimizations` hata ayıklama özniteliği geçersiz kılar [derleme öğesi](https://msdn.microsoft.com/library/s10awwz0.aspx) içinde *Web.config* dosyası


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Kullanarak paketleme ve küçültme ASP.NET Web formları ve Web sayfaları

- Web sayfaları için blog girişine bakın [ekleyerek Web iyileştirme Web sayfaları siteye](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- Web formları için blog girişine bakın [ekleme paketleme ve küçültme Web formları için](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Paketleme kullanarak ve ASP.NET MVC ile küçültme

Bu bölümde bir ASP.NET MVC paketleme incelemek için proje ve küçültme oluşturacağız. İlk olarak, adlı yeni bir ASP.NET MVC Internet projesi oluşturun **MvcBM** Varsayılanları hiçbirini değiştirmeden.

Açık *uygulama\_Start\BundleConfig.cs* dosya ve inceleyin `RegisterBundles` oluşturmak, kaydetme ve paketleri yapılandırmak için kullanılan yöntem. Aşağıdaki kod bir kısmı gösterir `RegisterBundles` yöntemi.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Önceki kod adlı yeni bir JavaScript paket oluşturur *~/bundles/jquery* tüm uygun içerir (Hata Ayıkla'dır veya küçültülmüş değil. *vsdoc*) dosyalar *betikleri* "~/Scripts/jquery-{version} .js" joker dizeyi eşleştir klasör. ASP.NET MVC 4 için bu dosya bir hata ayıklama yapılandırması ile gelir *jquery 1.7.1.js* pakete eklenecek. Bir yayın yapılandırmasında *jquery 1.7.1.min.js* eklenir. Paket framework gibi birkaç genel kurallar aşağıdaki gibidir:

- Yayın için ".min" dosyasını "FileX.min.js" ve "FileX.js" mevcut seçme.
- Hata ayıklama için olmayan ".min" sürümü seçin.
- Yoksayılıyor "-vsdoc" yalnızca IntelliSense tarafından kullanılan dosyaları (örneğin, jquery-1.7.1-vsdoc.js),.

`{version}` Yukarıda gösterilen joker karakter eşleştirme jQuery paket içinde jQuery uygun sürümü ile otomatik olarak oluşturmak için kullanılır, *betikleri* klasör. Bu örnekte, joker kullanarak aşağıdaki avantajları sağlar:

- Yukarıdaki paket kod veya sayfalarını görüntüleme jQuery başvurularında değiştirmeden jQuery bir sürüme güncelleştirmek için NuGet kullanmanıza olanak sağlar.
- Otomatik olarak seçer, hata ayıklama yapılandırmaları için tam sürümü ve ".min" sürüm yayın için oluşturur.

## <a name="using-a-cdn"></a>Bir CDN kullanma

 İzleme kodu yerel jQuery paket CDN jQuery paket ile değiştirir.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

Yukarıdaki kod sürümde modu ve jQuery hata ayıklama sürümü yerel olarak hata ayıklama modunda getirileceği sırada jQuery CDN istenecektir. CDN isteği başarısız olursa bir CDN kullanırken, bir geri dönüş mekanizması olmalıdır. Aşağıdaki biçimlendirmede jQuery CDN hatasında istemek için eklenen düzeni dosyası gösterir betiği sonundan parçalara.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Bir paket oluşturma

[Paket](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) sınıfı `Include` yöntemi her dize kaynağa sanal bir yol olduğu bir dizisini alır. RegisterBundles yönteminde alınan aşağıdaki kod *uygulama\_Start\BundleConfig.cs* dosyasını gösterir birden çok dosyaları bir pakete ait eklenir:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

[Paket](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) sınıfı `IncludeDirectory` yöntemi, bir arama deseniyle eşleşen tüm dosyaları bir dizin (ve isteğe bağlı olarak tüm alt dizinlerde) eklemek için sağlanır. [Paket](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) sınıfı `IncludeDirectory` API aşağıda gösterilmektedir:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Paket oluşturma yöntemini kullanarak görünümlerde başvurulan ( `Styles.Render` CSS için ve `Scripts.Render` JavaScript için). Aşağıdaki biçimlendirmeden *görünümler/paylaşılan\\_Layout.cshtml* dosyasını gösterir CSS ve JavaScript paketleri varsayılan ASP.NET Internet proje görünümleri ne başvuru.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

İşleme yöntemleri bir kod satırı içinde birden çok paket ekleyebilmek dizeler, bir dizi sağladığına dikkat edin. Genellikle varlık başvurmak için gerekli HTML oluşturan oluşturma yöntemleri kullanmak istersiniz. Kullanabileceğiniz `Url` URL'yi varlık için varlık başvurusu için gereken biçimlendirme oluşturmak için yöntem. Yeni HTML5 kullanmak istediğinizi varsayalım [zaman uyumsuz](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) özniteliği. Aşağıdaki kod modernizr kullanarak başvuru gösterilmektedir `Url` yöntemi.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Kullanarak "\*" dosyaları seçmek için joker karakter

Belirtilen sanal yol `Include` yöntemi ve arama deseni içinde `IncludeDirectory` yöntemi bir kabul edebilir "\*" joker karakter öneki veya son yol kesimi için soneki olarak. Büyük küçük harfe duyarlı arama dizesidir. `IncludeDirectory` Yöntemi alt dizinleri arama seçeneğiniz vardır.

Bir proje ile aşağıdaki JavaScript dosyaları göz önünde bulundurun:

- *Scripts\Common\AddAltToImg.js*
- *Scripts\Common\ToggleDiv.js*
- *Scripts\Common\ToggleImg.js*
- *Scripts\Common\Sub1\ToggleLinks.js*

![dir imag](bundling-and-minification/_static/image7.png)

Aşağıdaki tabloda gösterildiği gibi joker karakter kullanarak bir pakete eklenen dosyalar gösterilmektedir:

| **Arama** | **Eklenen dosyalar veya özel durum oluştu** |
| --- | --- |
| Include("~/Scripts/Common/\*.js") | *AddAltToImg.js, ToggleDiv.js, ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | Geçersiz desen özel durum. Joker karakter yalnızca önek veya sonek izin verilir. |
| İçerir ("~/Scripts/Common/\*og.\*") | Geçersiz desen özel durum. Yalnızca bir joker karakter izin verilir. |
| "İçerir (" ~/Scripts/Common/T\*") | *ToggleDiv.js, ToggleImg.js* |
| "İçerir (" ~/Scripts/Common/\*") | Geçersiz desen özel durum. Saf joker kesimi geçerli değil. |
| IncludeDirectory("~/Scripts/Common", "T\*") | *ToggleDiv.js, ToggleImg.js* |
| IncludeDirectory ("~/Scripts/Common", "T\*", doğru) | *ToggleDiv.js, ToggleImg.js, ToggleLinks.js* |

Açıkça bir pakete ait her bir dosya ekleme, genellikle tercih edilen dosyaları aşağıdaki nedenlerle joker yüklemeyi üzerinden:

- Komut dosyaları, genellikle istemezsiniz olan alfabetik sırada yükleme için joker karakter Varsayılanları tarafından ekleniyor. CSS ve JavaScript dosyaları sık (alfabetik olmayan) belirli bir sırada eklenmesi gerekir. Özel bir ekleyerek bu riski en aza indirebileceğiniz [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) uygulaması, ancak açıkça her dosyanın daha az hataya ekleme. Örneğin, değiştirmenizi gerektirebilir yeni varlıklar bir klasöre gelecekte ekleme olasılığınız, [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) uygulaması.
- Bu paket başvuran tüm görünümlerde yüklenirken joker karakter kullanarak bir dizine eklenen belirli dosyaları görüntüle eklenebilir. Betiği görüntüle belirli bir pakete ait eklediyseniz, paket başvuran diğer görünümleri bir JavaScript hatası alabilirsiniz.
- Diğer dosyaları alma CSS dosyaları iki kez yüklenen içeri aktarılan dosyaları neden. Örneğin, aşağıdaki kod iki kez yüklenen jQuery UI tema CSS dosyaları çoğunu bir paket oluşturur. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  Joker karakteriyle Seçici "\*.css" klasöründeki her CSS dosyasında getirir dahil olmak üzere *Content\themes\base\jquery.ui.all.css* dosya. *Jquery.ui.all.css* dosyasını diğer CSS dosyaları aktarır.

## <a name="bundle-caching"></a>Paketini önbelleğe alma

Paketleri, paketin ne zaman oluşturulur, bir yıl Expires HTTP üstbilgisini ayarlayın. IE paket için koşullu bir istek yapmaz Fiddler gösterir daha önce görüntülenen bir sayfaya giderseniz, diğer bir deyişle, IE paketleri için bir HTTP GET isteklerine ve vardır sunucusundan gelen HTTP 304 yanıt yok. Her paket için koşullu bir istek (her paket için bir HTTP 304 yanıt sonuçta) F5 tuşuna ile yapmak için IE zorlayabilirsiniz. Kullanarak tam yenileme uygulayabilirsiniz ^ F5 (her paket için bir HTTP 200 yanıtı sonuçlanır.)

Aşağıdaki resimde gösterildiği **önbelleğe alma** Fiddler yanıt bölmesinin sekmesinde:

![fiddler önbelleğe alma görüntüsü](bundling-and-minification/_static/image8.png)

İstek   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 paket için olan **AllMyScripts** ve bir sorgu dizesi çiftini içeren **v r0sLDicvP58AIXN =\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. Sorgu dizesi **v** diğer bir deyişle önbelleğe alma işlemi için kullanılan benzersiz bir tanımlayıcı belirteci bir değere sahip. Paket değiştirmez sürece, ASP.NET uygulaması isteyeceğini **AllMyScripts** Bu belirteci kullanarak paket. Paket herhangi bir dosyada değişirse, ASP.NET iyileştirme çerçevesi tarayıcı isteklerini paket için en son paket alırsınız güvence altına almak, yeni bir belirteç oluşturur.

IE yanlış IE9 F12 geliştirici araçlarını çalıştırma ve daha önce yüklenen bir sayfasına gidin, her paket ve HTTP 304 döndürme sunucu yapılan koşullu GET istekleri gösterir. IE9 koşullu isteği blog girişi yaptıysanız belirleme sorunları neden olduğunu okuyabilirsiniz [kullanarak CDN'ler ve Web sitesinin performansını artırmak için Expires](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>LESS, CoffeeScript, SCSS, Sass Bundling.

Paketleme ve küçültme framework Ara diller gibi işlemek için bir mekanizma sağlar [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [daha az](http://www.dotlesscss.org/) veya [Coffeescript ](http://coffeescript.org/)ve sonuçta elde edilen paket için dönüşümler küçültme gibi uygulayın. Örneğin, eklemek için [.less](http://www.dotlesscss.org/) , MVC 4 proje dosyalarına:

1. Daha az içeriğiniz için bir klasör oluşturun. Aşağıdaki örnek kullanır *Content\MyLess* klasör.
2. Ekleme [.less](http://www.dotlesscss.org/) NuGet paketi **noktasız** projenize.  
    ![NuGet noktasız yükleme](bundling-and-minification/_static/image9.png)
3. Arabirimini uygulayan bir sınıf ekleyin [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) arabirimi. .Less dönüştürme için aşağıdaki kodu projenize ekleyin.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Paketi birlikte daha az dosyalarının oluşturma `LessTransform` ve [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) dönüştürün. Aşağıdaki kodu ekleyin `RegisterBundles` yönteminde *uygulama\_Start\BundleConfig.cs* dosya.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Daha az paket başvuran tüm görünümler için aşağıdaki kodu ekleyin.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Paket konuları

Paketleri oluştururken izlemeniz gereken bir iyi paket adı öneki olarak "paketi" eklenecek kuralıdır. Bu olası engelleyecek [yönlendirme çakışma](https://forums.asp.net/post/5012037.aspx).

Bir paketteki tek bir dosyayı güncelleştirmek sonra yeni bir belirteç paket sorgu dizesi parametresi için oluşturulur ve bir istemci paketini içeren bir sayfa istediğinde tam paket indirilmesi gerekir. Burada her varlık ayrı ayrı listelenir geleneksel biçimlendirmede yalnızca değiştirilen dosya indirilmesi. Sık sık değişen varlıklar, paketleme için iyi bir aday olmayabilir.

Paketleme ve küçültme öncelikle ilk sayfa isteği yükleme süresini artırır. Bir Web sayfası istenen sonra tarayıcı (JavaScript, CSS ve görüntüler) varlıklar önbelleğe paketleme ve küçültme olmaz sağlamak herhangi performansı artırma aynı sayfa isterken ya da aynı sayfalarında site aynı varlıklar isteme. Verme ayarlarsanız, varlıklarınızı doğru başlığındaki süresi dolar ve paketleme ve küçültme kullanmayın, tarayıcılar yenilik buluşsal yöntemler varlıklar eski birkaç gün sonra işaretler ve tarayıcı her varlık için bir doğrulama isteği gerektirir. Bu durumda, paketleme ve küçültme sonra ilk sayfa isteği performans artışı sağlar. Ayrıntılar için bkz: blog [kullanarak CDN'ler ve Web sitesinin performansını artırmak için Expires](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

Her bir ana bilgisayar başına altı eşzamanlı bağlantı tarayıcı sınırlandırılmasıdır kullanılarak azaltılabilir bir [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). CDN barındırma sitenizi farklı bir ana bilgisayar adı olduğundan, varlık istekleri CDN altı eşzamanlı bağlantı sayısı sınırı barındırma ortamınıza karşı sayar değil. Bir CDN ortak paketini önbelleğe alma ve avantajları önbelleğe alma kenar de sağlar.

Paketleri tarafından gereksinim duyulan sayfaları bölümlenmiş. Örneğin, varsayılan Internet uygulaması için ASP.NET MVC şablonu jQuery doğrulama paket jQuery ayrı oluşturur. Oluşturulan varsayılan görünümleri herhangi bir giriş varsa ve değerleri göndermeyin çünkü doğrulama paket dahil etmeyin.

`System.Web.Optimization` Ad alanı içinde System.Web.Optimization.DLL uygulanır. Bu WebGrease kitaplığı (WebGrease.dll) küçültme özellikleri için sırayla Antlr3.Runtime.dll kullanan yararlanır.

*Twitter hızlı gönderileri yapın ve bağlantıları paylaşmak için kullandığım. My Twitter tanıtıcısı*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Ek Kaynaklar

- Video:[paketleme ve en iyi duruma getirme](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) tarafından [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Web sayfaları siteye Web iyileştirme ekleme](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Ekleme paketleme ve küçültme Web formları için](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Performans etkileri paketleme ve küçültme Web gözatma](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) tarafından [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [CDN'ler kullanarak Web sitesi performansını artırmak için dolmadan](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) tarafından Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [RTT (iletim süreleriyle) simge durumuna küçült](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Katkıda Bulunanlar

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
