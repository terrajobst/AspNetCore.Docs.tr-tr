---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: Üst özellikleri ASP.NET Web sayfaları 2 | Microsoft Docs
author: microsoft
description: Bu konu en iyi yeni özellikler WebMatr ile birlikte sağlanan bir basit bir web programlama çerçevesi ASP.NET Web sayfaları 2 sürümündeki genel bir bakış sağlar...
ms.author: aspnetcontent
ms.date: 02/13/2012
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: 6e20dedd19ae458b9881973570f23b5d77dda654
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827407"
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>ASP.NET Web sayfaları 2'de en iyi Özellikler
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu makalede, ASP.NET Web sayfaları 2 RC ile birlikte sağlanan bir basit bir web programlama çerçevesi içinde en yeni özelliklere genel bakış sağlar [Microsoft WebMatrix RC 2](https://www.microsoft.com/web/).
> 
> **Neleri kapsar:** 
> 
> - [WebMatrix yükleme](#install)
> - [Yeni ve geliştirilmiş özellikler](#New_and_Enhanced_Features)
> 
>     - [RC sürümü için değişiklikler](#Changes_for_the_RC_Version)
>     - [Beta sürümü için değişiklikler](#Changes_for_the_Beta_Version)
>     - [Yeni ve güncelleştirilmiş Site şablonları kullanma](#templates)
>     - [Kullanıcı girişini doğrulama](#validation)
>     - [Oturum açma bilgileri Facebook ve OAuth ve Openıd kullanarak diğer sitelere etkinleştirme](#oauthsetup)
>     - [Haritalar Yardımcısını kullanarak eşlemeleri ekleme](#maphelper)
>     - [Web sayfaları uygulamalarını yan yana çalıştırma](#sidebyside)
>     - [Mobil cihazlar için sayfa oluşturma](#mobile)
> - [Ek kaynaklar](#resources)
> 
> > [!NOTE]
> > Bu konuda, WebMatrix ile ASP.NET Web Pages 2 kodunuzu çalışmak için kullandığınız varsayılır. Web sayfaları 1 ile Visual Studio kullanarak Web Pages 2 Web siteleri de oluşturabileceğiniz gibi ancak size sağlayan IntelliSense özellikleri ve hata ayıklama iyileştirdik. Visual Studio'da Web sayfaları ile çalışmak için önce Visual Studio 2010 SP1, Visual Web Developer Express 2010 SP1 veya Visual Studio 11 Beta yüklemelisiniz. Ardından, şablonları ve Visual Studio'da ASP.NET MVC 4 ve Web Pages 2 uygulamaları oluşturmaya yönelik araçlar içeren ASP.NET MVC 4 Beta yükleyin.
> 
> 
> *Son güncelleştirme: 18 Haziran 2012*


<a id="install"></a>
## <a name="installing-webmatrix"></a>WebMatrix yükleme

Web sayfalarını yüklemek için Microsoft Web Platformu yükleyicisi, yükleme ve web ile ilgili teknolojileri yapılandırma kolaylaştıran ücretsiz bir uygulama olduğu kullanabilirsiniz. Web sayfaları 2 Beta içerir WebMatrix 2 Beta yükler.

1. Web Platformu Yükleyicisi'nin en son sürümü yükleme sayfasına göz atın:

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > WebMatrix 1 zaten yüklüyse, bu yükleme için WebMatrix 2 Beta güncelleştirir. 1 veya 2 sürümünü aynı bilgisayara kullanılarak oluşturulan Web siteleri çalıştırabilirsiniz. Daha fazla bilgi için üzerinde bölümüne bakın. [çalışan Web Pages uygulamaları yan yana](#sidebyside).
2. Seçin **Şimdi Yükle**. 

    Internet Explorer kullanıyorsanız, sonraki adıma gidin. Mozilla Firefox veya Google Chrome gibi farklı bir tarayıcı kullanıyorsanız kaydetmeniz istenir *Webmatrix.exe* dosyayı bilgisayarınıza. Dosyayı kaydedin ve ardından yükleyiciyi başlatmak için tıklatın.
3. Yükleyiciyi çalıştırın ve seçin **yükleme** düğmesi. Bu, WebMatrix ve Web sayfaları yükler.

## <a id="New_and_Enhanced_Features"></a>  Yeni ve geliştirilmiş özellikler

### <a id="Changes_for_the_RC_Version"></a>  Değişiklikleri RC sürümünü (Haziran 2012)

Haziran 2012 RC sürüm Mart 2012'de yayımlanan Beta sürümü yenileme bazı değişiklikler vardır. Bu değişiklikler şunlardır:

- A `Validation.AddFormError` yöntemi eklendiği `Validation` Yardımcısı. El ile doğrulama gerçekleştirirseniz kullanışlıdır (örneğin, sorgu dizesinde geçirilen bir değeri doğrulamak) ve tarafından görüntülenen hata iletisi eklemek istediğiniz `Html.ValidationSummary` yöntemi. Daha fazla bilgi için bkz [doğrulama veri emin değil gelir doğrudan kullanıcıların](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) içinde [ASP.NET Web sayfaları (Razor) sitelerinde kullanıcı girişini doğrulama](https://go.microsoft.com/fwlink/?LinkId=253002).
- ASP.NET Web Pages 2 Çekirdek derlemeleri paketleme ve küçültme için işlevsellik kaldırıldı. Sonuç olarak `Assets` yardımcı bu belgenin sonraki bölümlerinde listelenen kullanılabilir değil. Bunun yerine, yüklemeniz gereken [ASP.NET iyileştirme](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) NuGet paketi. Daha fazla bilgi için [paketleme ve küçültme bir ASP.NET Web sayfaları (Razor) sitesinde varlıkları](https://go.microsoft.com/fwlink/?LinkId=255373).
- ASP.NET Web Pages 2 desteklemek için ek derlemeler sürümüne eklenmiştir. Bir sitenin daha fazla derlemeleri görebilirsiniz bu değişiklik yalnızca fark edilebilir etkisi olan *bin* bir site oluşturun ya da bir barındırma sağlayıcısına bir site dağıtma sonra klasör.

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Değişiklikleri Beta sürümünün (Şubat 2012)

Şubat 2012'de yayınlanan Beta sürümü, yalnızca birkaç değişiklik aralık 2011'de yayımlanan Beta sürümüne sahiptir. Bu değişiklikler şunlardır:

- Razor artık koşullu özniteliklerini de destekler. Sunucu kodu öğesi bir öznitelik için bir değer, ayarlanırsa, bir HTML çözümler `false` veya `null`, ASP.NET değil işleme öznitelik hiç. Örneğin, bir onay kutusu için aşağıdaki biçimlendirme olduğunu düşünün:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    Varsa değerini `checked1` çözümler `false` veya `null`, `checked` özniteliği değil işlenir. Bir değişiklik budur.
- `Validation.GetHtml` Yöntemi adlandırıldı `Validation.For`. Bu, bir değişiklik, `Validation.GetHtml` Beta sürümünde çalışmaz.
- Artık içerebilir `~` kullanmadan bir site kökünde başvurmak için biçimlendirmeyi işlecinde `Href` işlevi. (Diğer bir deyişle, Razor ayrıştırıcısı Şimdi Bul çözmek ve `~` işleci bir açık yöntem çağrısına gerektirmeden `Href`.) `Href` Yöntem hala çalışır, bir değişiklik olmadığından.

    Örneğin, daha önce bu gibi biçimlendirme tablonuz varsa:

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    Artık bu gibi biçimlendirme kullanabilirsiniz:

    `<a href="~/Default.cshtml">Home</a>`
- `Scripts` İle yardımcı varlıklar (kaynak) yönetimi için değiştirilmiştir `Assets` Yardımcısı, aşağıdaki gibi biraz farklı yöntemleri vardır:

  - İçin `Scripts.Add`, kullanın `Assets.AddScript`
  - İçin `Scripts.GetScriptTags`, kullanın `Assets.GetScripts`

    Bu, bir değişiklik, `Scripts` sınıf Beta sürümünde kullanılabilir değil. Varlık Yönetimi kullanan kod örnekleri bu belgede, bu değişiklik ile güncelleştirildi.

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>Yeni ve güncelleştirilmiş Site şablonları kullanma

**Başlangıç sitesi** şablonu güncelleştirilmiş Web Pages 2 üzerinde çalışır, böylece varsayılan olarak. Ayrıca, aşağıdaki yeni özellikleri içerir:

- Mobil aygıt dostu sayfa işleme. CSS stilleri kullanarak ve `@media` Seçici, **başlangıç sitesi** mobil cihaz ekranlar dahil olmak üzere küçük ekranlarda sayfaların gelişmiş işleme seçenekleri sağlar.
- Gelişmiş üyelik ve kimlik doğrulama seçenekleri. Kullanıcıların oturum Twitter, Facebook ve Windows Live gibi diğer sosyal ağ sitelerine hesaplarını kullanarak sitenizi uygulamasına izin verebilirsiniz. Daha fazla bilgi için [etkinleştirme oturumları Facebook ve OAuth ve Openıd kullanarak diğer sitelere](#oauthsetup) bölümü.
- HTML5 öğeleri.

Yeni **Kişisel Site** şablon kişisel bir blog, fotoğraf sayfasına ve Twitter sayfasını içeren bir Web sitesinin oluşturmanıza olanak tanır. Temel bir site özelleştirebilirsiniz **Kişisel Site** aşağıdakileri yaparak şablonu:

- Düzen dosyasını düzenleyerek site görünümünü değiştirme (*\_SiteLayout.cshtml*) ve stilleri dosyasını (*Site.css*).
- Sitenize işlevsellik ekleyen bir NuGet paketlerini yükleyin. Paketleri yükleme hakkında daha fazla bilgi için ASP.NET Web Yardımcıları kitaplığı dahil olmak üzere görecekleri öğretici [Yardımcıları yükleme](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).

Erişim için **Kişisel Site** şablonu seçin **şablonları** WebMatrix üzerinde **Hızlı Başlangıç** ekran.

[![topseven personalsite 1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

İçinde **şablonları** iletişim kutusunda **Kişisel Site** şablonu.

[![topseven personalsite 2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

Giriş sayfasında **Kişisel Site** sayfası ve fotoğraf sayfasına Twitter şablon blogunuz ayarlamak için bağlantıları izleyin olanak tanır.

[![topseven personalsite 3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>Kullanıcı girişini doğrulama

Web sayfaları 1'de gönderilen formlar üzerindeki kullanıcı girişini onaylamak için kullandığınız `System.Web.WebPages.Html.ModelState` sınıfı. (Bu kod örnekleri birkaç başlıklı Web sayfaları 1 öğreticide gösterilmiştir [verilerle çalışma](../data/5-working-with-data.md).) Web sayfaları 2'de bu yaklaşım kullanmaya devam edebilirsiniz. Ancak, Web Pages 2 Ayrıca kullanıcı girişini doğrulama için geliştirilmiş araçlar sunar:

- Yeni doğrulama sınıflar `System.Web.WebPages.ValidationHelper` ve `System.Web.WebPages.Validator`, izin veren birkaç satır kodla güçlü doğrulama görevlerine.
- İsteğe bağlı olarak, istemci tarafı doğrulama, doğrulama hataları için denetlenecek sunucuya gidiş dönüş gerektiren yerine kullanıcıya anında geri bildirim sağlar. (Güvenlik nedeniyle, denetimler istemci önceden yapılan bile doğrulama sunucu üzerinde gerçekleştirilir.)

Yeni doğrulama özellikleri kullanmak için aşağıdakileri yapın:

Sayfanın kodda yöntemleri kullanılarak doğrulanması için bir öğe kaydetme `Validation` Yardımcısı: `Validation.RequireField`, `Validation.RequireFields` (gerekli olması için birden çok öğe kaydetmek için), veya `Validation.Add`. `Add` Yöntemi doğrulama denetimleri, veri denetimi, farklı alanlarda dize uzunluğu denetimleri, girdileri karşılaştırma türü gibi diğer tür belirtmenize olanak tanır ve desenler (normal ifadeler kullanarak). Bazı örnekler şunlardır:

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

Bir alana özgü hata görüntülemek için çağrı `Html.ValidationMessage` biçimlendirmede Doğrulanmakta olan her öğe için:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

Özeti görüntülemek için (`<ul>` listesi) sayfasında, tüm hataların `Html.ValidationSummary` biçimlendirme içinde:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

Bu adımlar, sunucu tarafı doğrulama uygulamak için yeterli içindir. İstemci tarafı doğrulama eklemek istiyorsanız, aşağıdakileri ayrıca yapın.

Aşağıdaki komut dosyası başvuruları içinde ekleme `<head>` bölümünü bir web sayfası. Bir içerik teslim ağı (CDN) sunucusundaki uzak dosyaları ilk iki komut dosyası başvuruları gelin. Üçüncü başvuru yerel komut dosyasına işaret eder. CDN kullanılamıyorsa, üretim uygulamaları bir geri dönüş uygulamalıdır. Geri dönüş test edin.

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

Yerel bir kopyasını almak için en kolay yolu *jquery.validate.unobtrusive.min.js* kitaplığı (örneğin, başlangıç sitesi) sitesi şablonlarından birini dayalı yeni bir Web sayfaları sitesinde oluşturmaktır. Şablon tarafından oluşturulan sitesi içeren *jquery.validate.unobtrusive.js* içinden, kopyalayabilir, sitenizde betikleri klasörünün dosyasında.

Web sitenizi kullanıyorsa bir<em>\_SiteLayout</em> sayfa düzeni denetlemek için sayfa, böylece tüm içerik sayfalarına doğrulama kullanılabilir bu sayfadaki bu komut dosyası başvuruları ekleyebilirsiniz. Yalnızca belirli sayfalarda doğrulama gerçekleştirmek istiyorsanız, komut dosyaları yalnızca bu sayfalara kaydetmek için varlıklar Yöneticisi'ni kullanabilirsiniz. Bunu yapmak için `Assets.AddScript(path)` sayfasına, doğrulamak ve her komut dosyalarını başvurmak istiyorsanız. Ardından bir çağrı ekleyin `Assets.GetScripts` içinde  <em>\_SiteLayout</em> kayıtlı işlemek için sayfa `<script>` etiketler. Daha fazla bilgi için konudaki [varlıklar Yöneticisi ile kaydetme betikleri](#resmanagement).

Tek bir öğe için işaretlemede çağrı `Validation.For` yöntemi. Öznitelikler bu yöntem yayar, jQuery, istemci tarafı doğrulama sağlamak için takabilirsiniz. Örneğin:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

Aşağıdaki örnek, bir form üzerinde kullanıcı girişini doğrulayan bir sayfa görüntülenir. Ve bu doğrulama kodu test çalıştırmak için şunu yapın:

1. İçeren WebMatrix 2 sitesi şablonlarından birini kullanarak yeni bir web sitesi oluşturma bir *betikleri* klasörü gibi **başlangıç sitesi** şablonu.
2. Yeni site yeni bir oluşturma *.cshtml* sayfası ve sayfanın içeriğini aşağıdaki kodla değiştirin.
3. Sayfanın tarayıcıda çalıştırın. Doğrulama etkilerini görmek için geçerli ve geçersiz değerler girin. Örneğin, gerekli bir alanı boş bırakın veya bir harfini girin **KREDİLERİ** alan.


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

Bir kullanıcı geçerli giriş gönderdiğinde sayfa şöyledir:

[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

İsteğe bağlı olarak bir kullanıcı bir gerekli alan boş bırakılırsa gönderdiğinde, sayfanın şöyledir:

[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

İşte sayfası bir kullanıcı, bir tamsayı olarak dışında bir şey ile gönderdiğinde **KREDİLERİ** alan:

[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

Daha fazla bilgi için aşağıdaki blog gönderilerine bakın:

- [Güncelleştirilen Web sayfaları v2 doğrulama](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) kullanarak doğrulama ekleme Temelleri `Validation` yardımcı (yalnızca sunucu tarafı)
- [Güncelleştirilen Web sayfaları v2, 2. Bölüm doğrulama](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) istemci tarafı doğrulama ekleme.
- [Güncelleştirilen Web sayfaları v2, 3. Kısım doğrulama](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) biçimlendirme doğrulama hataları.

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>Varlık Yöneticisi'ni kullanarak komut dosyalarını kaydetme

Varlık Yöneticisi'ni sunucu kodunda kaydetmek ve istemci komut dosyalarını işlemek için kullanabileceğiniz yeni bir özelliktir. Kod (örneğin, Düzen sayfaları, içerik sayfalarını, Yardımcıları, vb.) tek bir sayfada çalışma zamanında birleştirilir birden fazla dosyalardan ile çalışırken bu yararlı bir özelliktir. Varlık Yöneticisi'ni betiklerinin doğru başvurulduğundan ve verimli bir şekilde işlenen sayfasında, hangi kod dosyaları bağımsız olarak bunlara gelen verilir veya kaç kez çağrılır, emin olmak için kaynak dosyaları düzenler. Varlık Yöneticisi ayrıca işler `<script>` sayfayı hızlı bir şekilde (komut işleme sırasında indirmeden) yükleyebilir ve betikleri işlemeden önce çağrılırsa oluşabilecek hataları önlemek için tam etiketler doğru yerdesiniz.

Örneğin, bir JavaScript dosyası çağıran özel bir yardımcı oluşturduğunuz düşünün ve bu yardımcı, üç farklı konumlarda içerik sayfası kodunuzda çağırın. Varlıklar manager kullanmazsanız kaydetmek için yardımcıyı içinde üç farklı betik çağıran `<script>` etiketleri aynı komut dosyasını tüm noktasına işlenmiş sayfanızda görünecektir. Ayrıca, bağlı olarak `<script>` etiketleri, oluşturulan sayfada yerleştirildiğinde, betik sayfa tam yüklenmeden önce belirli sayfa öğeleri erişmeye çalıştığında hata oluşabilir. Komut dosyası kaydetmek için varlıklar Yöneticisi'ni kullanırsanız, bu sorunlardan kaçının.

Bunu yaparak varlıklar Yöneticisi ile bir betik kaydedebilirsiniz:

- Betik başvurmak için gereken kod içinde çağrı `Assets.AddScript` yöntemi.
- İçinde bir  *\_SiteLayout* sayfasında, çağrı `Assets.GetScripts` işlenecek yöntemi `<script>` etiketler. 

    > [!NOTE]
    > Çağrı yerleştirmek `Assets.GetScripts` son öğesi olarak `<body>` öğesinin  *\_SiteLayout* sayfası. Bu sayfa yardımcı oldu daha hızlı bir şekilde yüklemek ve betik hataları önlemeye yardımcı olabilir.

Aşağıdaki örnek, varlık Yöneticisi'ni nasıl çalıştığını gösterir. Kod, aşağıdaki öğeleri içerir:

- Adlı özel bir yardımcı `MakeNote`. Bu yardımcı sarmalama tarafından bir kutu içinde bir dize oluşturur. bir `div` çevresinde öğesi, ekleyerek bir kenarlık ile biçimlendirilmiş &quot;Not:&quot; ona. Yardımcı ayrıca notu çalışma zamanı davranışını ekleyen bir JavaScript dosyasını çağırır. Komut dosyasını başvurusu yerine bir `<script>` etiket Yardımcısı çağırarak betik kaydeder `Assets.AddScript` .
- Bir JavaScript dosyası. Geçici olarak sırasında Not öğelerinin yazı tipi boyutunu artırır ve bu Yardımcısı tarafından çağrılan dosyasıdır bir `mouseover` olay.
- Başvuran bir içerik sayfasını<em>\_SiteLayout</em> sayfasında, bazı içerikleri gövdesine oluşturur ve ardından çağırır `MakeNote` Yardımcısı.
- A  *\_SiteLayout* sayfası. Bu sayfa, bir ortak üstbilgisi ve sayfa düzeni yapısı sağlar. Ayrıca bir çağrı içerdiğine `Assets.GetScripts`, varlıklar manager betik nasıl işlediğini olduğu bir sayfaya çağırır.

Örneği çalıştırmak için:

1. Boş bir Web Pages 2 Web sitesi oluşturun. WebMatrix kullanarak **boş Site** için bu şablonu.
2. Adlı bir klasör oluşturun *betikleri* sitesinde.
3. İçinde *betikleri* klasöründe adlı bir dosya oluşturun *Test.js*, kopyalama *Test.js* bağlanacağım örnekte içerik ve dosyayı kaydedin...
4. Adlı bir klasör oluşturun *uygulama\_kod* sitesinde.
5. İçinde *uygulama\_kod* klasöründe adlı bir dosya oluşturun *Helpers.cshtml*içine örnek kodu kopyalayın ve adlı bir klasöre kaydedin *uygulama\_kod*kök klasöründe.
6. Sitenin kök klasöründe adlı bir dosya oluşturun  *\_SiteLayout.cshtml,* örneği içine kopyalayın ve dosyayı kaydedin.
7. Sitesinin kök dizininde adlı bir dosya oluşturun *ContentPage.cshtml*, örnek kod ekleyin ve kaydedin.
8. Çalıştırma *ContentPage* bir tarayıcıda. Geçirilen için dize `MakeNote` Yardımcısı, kutulanmış Not olarak işlenir.
9. Fare işaretçisi bir notun üzerine geçirin. Betik, geçici olarak Not yazı tipi boyutu da artar.
10. İşlenen sayfanın kaynağı görüntüleyin. Çağrı yerleştirdiğiniz nedeniyle `Assets.GetScripts`, işlenen `<script>` çağıran etiketi *Test.js* sayfanın gövdesindeki son öğe.

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

Aşağıdaki ekran görüntüsü gösterildiği *ContentPage.cshtml* fare işaretçisini bir not üzerinde tuttuğunuzda, bir tarayıcıda:

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Oturum açma bilgileri Facebook ve OAuth ve Openıd kullanarak diğer sitelere etkinleştirme

Web Pages 2 üyelik ve kimlik doğrulama için Gelişmiş seçenekler sunar. Ana geliştirme yeni yoktur [OAuth](http://oauth.net/) ve [Openıd](http://openid.net/) sağlayıcıları. Bu sağlayıcıları kullanarak, sitenizi Facebook, Twitter, Windows Live, Google ve Yahoo var olan kimlik bilgilerini kullanarak oturum kullanıcılar oturum sağlayabilirsiniz. Örneğin, bir Facebook hesabı kullanarak oturum açın, kullanıcılar yalnızca bunları kullanıcı bilgilerini girdiğiniz yere Facebook oturum açma sayfasına yönlendiren bir Facebook simgesi seçebilirsiniz. Bunlar daha sonra Facebook oturum açma hesabıyla sitenizde ilişkilendirebilirsiniz. Bir ilgili Web sayfalarını üyelik özelliklerine kullanıcılar birden çok oturum açma (sosyal ağ sitelerine oturumlardan dahil) ilişkilendirebilirsiniz tek bir hesap kullanarak Web sitenizde geliştirmedir.

Bu görüntü oturum açma sayfasından gösterir **başlangıç sitesi** şablonu, burada bir kullanıcı seçebilirsiniz dış bir hesabıyla oturum açma etkinleştirmek için bir Facebook, Twitter veya Windows Live simgesi:

[![topSeven oauth 1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

Yalnızca birkaç satır kod kullanarak, OAuth ve Openıd üyelik etkinleştirebilirsiniz. Özellikler ve yöntemler OAuth ile çalışmak için kullanın ve Openıd sağlayıcılarının alanlarındadır `WebMatrix.Security.OAuthWebSecurity` sınıfı.

Ancak, diğer sitelerden oturum açmayı etkinleştirebilir için kod kullanmak yerine, yeni sağlayıcıları ile kullanmaya başlamak için önerilen yol yeni kullanmaktır **başlangıç sitesi** WebMatrix 2 Beta dahil olan şablon. **Başlangıç sitesi** şablonu tam üyelik altyapı içerir, kullanıcıların oturum yerel kimlik bilgilerini veya bu başka bir siteden kullanarak sitenizi uygulamasına izin vermeniz gerekiyor bir oturum açma sayfası, bir üyelik veritabanı ve tüm kodu ile tamamlandı .

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>Oturum açma bilgileri OAuth ve Openıd sağlayıcılarının kullanarak etkinleştirme

Bu bölümde, temel alan bir siteye (Facebook, Twitter, Windows Live, Google ve Yahoo) dış sitelerden oturum açmasına izin vermek nasıl bir örnek sağlar. **başlangıç sitesi** şablonu. Başlangıç sitesi oluşturduktan sonra bu (ayrıntıları izleme) yapın:

- Bir OAuth sağlayıcısı (Facebook, Twitter ve Windows Live) kullanan siteler için dış sitesinde bir uygulama oluşturun. Bu, bu siteleri için oturum açma özelliği çağırmak için gereken uygulama anahtarlarını sağlar. Bir Openıd sağlayıcısı (Google, Yahoo) kullanan siteler için uygulama oluşturma gerekmez. Tüm bu sitelerden oturum açmak için ve geliştirici uygulamaları oluşturmak için hesabınız gerekir. 

    > [!NOTE]
    > Oturum açma bilgileri test etmek için bir yerel Web sitesi URL'si kullanamazsınız. Bu nedenle Windows Canlı uygulamaları yalnızca bir çalışan Web sitesi için Canlı bir URL kabul edin.
- Web sitenizi birkaç dosyalarında, uygun kimlik doğrulama sağlayıcısını belirtmek için ve bir oturum açma kullanmak istediğiniz siteye göndermek için düzenleyin.

**Google ve Yahoo oturum açmayı etkinleştirmek için**:

1. Web siteniz Düzenle  *\_AppStart.cshtml* sayfasında ve çağrısından sonra Razor kod bloğunda aşağıdaki iki kod satırlarını ekleme `WebSecurity.InitializeDatabaseConnection` yöntemi. Bu kod Google ve Yahoo Openıd sağlayıcılarının sağlar. 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. İçinde *~/Account/Login.cshtml* sayfasında, aşağıdakilerden yorumları Kaldır `<fieldset>` işaretleme, sayfanın sonundaki bloğu. Kodun açıklamasını kaldırın için kaldırın `@*` koyun ve izleyin karakterler `<fieldset>` blok. Sonuçta elde edilen kod bloğu şöyle görünür:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. Ekleme bir `<input>` Google veya Yahoo sağlayıcının öğesi `<fieldset>` grubu *~/Account/Login.cshtml* sayfası. Güncelleştirilmiş `<fieldset>` grubu `<input>` öğeleri için Google ve Yahoo hem görünüyor aşağıdaki örnekteki gibi: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. İçinde *~/Account/AssociateServiceAccount.cshtml* sayfasında, eklemek `<input>` öğeleri için Google veya Yahoo için `<fieldset>` dosyanın sonlarında grubu. Aynı kopyalayabilirsiniz `<input>` eklendiğiniz öğeleri `<fieldset>` konusundaki *~/Account/Login.cshtml* sayfası. 

    *~/Account/AssociateServiceAccount.cshtml* üzerinde kullanıcıların ilişkilendirebilir birden çok oturum açma diğer sitelerdeki tek bir hesap kullanarak Web sitenizde bir sayfa oluşturmak istiyorsanız başlangıç sitesi şablonunda sayfasında kullanılabilir.

Artık Google ve Yahoo oturum açmayı test edebilirsiniz.

1. Çalıştırma *default.cshtml* sitenizin sayfasını ve **oturum** düğmesi.
2. Üzerinde *oturum açma* sayfasında **başka bir hizmete oturum açmak için kullandığınız** bölümünde, ya da seçin **Google** veya **Yahoo** Gönder düğmesi. Bu örnek, Google oturum açma bilgilerini kullanır. 

    Web sayfasının isteği Google oturum açma sayfasına yönlendirir.

    [![topSeven oauth 6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. Varolan bir Google hesabı kimlik bilgilerini girin.
4. Google hesabı bilgileri kullanmak Localhost izin vermek istediğiniz isterse **izin**.

    Kodu, kullanıcının kimliğini doğrulamak için Google belirteci kullanır ve bu sayfaya Web sitenizde döndürür. Bu sayfa, kullanıcıların kendi Google oturum açma bilgilerini kullanarak Web sitenizde var olan bir hesapla ilişkilendirmek sağlar veya dış oturum açma ile ilişkilendirmek için sitenizde yeni bir hesap kaydedebilir.

    [![topSeven oauth 5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. Seçin **ilişkilendirmek** düğmesi. Tarayıcıda uygulama giriş sayfasına döndürür.

    [![topSeven oauth 3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven oauth 3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Facebook oturum açma bilgileri sağlamak**:

1. Git [Facebook geliştiriciler site](https://developers.facebook.com/apps) (henüz oturum açmadıysanız oturum açın).
2. Seçin **yeni uygulama oluştur** düğmesini ve sonra adı ve yeni uygulama oluşturmak için istemleri izleyin.
3. Bölümünde **uygulamanızı Facebook ile nasıl tümleştirilecek seçin**, seçin **Web sitesi** bölümü.
4. Doldurun **Site URL'si** alanına sitenizin URL'siyle (örneğin, [ `http://www.example.com` ](http://www.example.com)). **Etki alanı** alan isteğe bağlıdır; bunu tüm etki alanı için kimlik doğrulaması sağlamak için kullanabilirsiniz (örneğin *example.com*). 

    > [!NOTE]
    > Yerel bilgisayarınızda bir URL ile bir site çalıştırıyorsanız, ister `http://localhost:12345` (sayıdır bir yerel bağlantı noktası numarası), bu değeri ekleyebileceğiniz **Site URL'si** sitenizi test etmek için alan. Ancak, yerel site değişikliği bağlantı noktası numarası için istediğiniz zaman, güncelleştirmeniz gerekecektir **Site URL'si** uygulamanızın alan.
5. Seçin **Değişiklikleri Kaydet** düğmesi.
6. Seçin **uygulamaları** sekmesine ve ardından uygulamanız için başlangıç sayfasını görüntüleyin.
7. Kopyalama **uygulama kimliği** ve **uygulama gizli anahtarı** uygulamanız için değerleri ve bunları bir geçici bir metin dosyasına yapıştırın. Bu değerler, Web sitesi kodunuzda Facebook sağlayıcıya geçer.
8. Facebook Geliştirici sitesi çıkın.

Kullanıcıların böylece artık iki siteniz Facebook hesaplarını kullanarak siteye kayıt yapabiliyor değişiklik.

1. Web siteniz Düzenle  *\_AppStart.cshtml* sayfasında ve Facebook OAuth sağlayıcısı için kodun açıklamasını kaldırın. Açıklamalı olmayan kod bloğunu aşağıdaki gibi görünür: 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. Kopyalama **uygulama kimliği** değeri olarak Facebook uygulaması değerinden `consumerKey` parametre (tırnak işaretleri içinde).
3. Kopyalama **uygulama gizli anahtarı** Facebook uygulaması değerinden `consumerSecret` parametre değeri.
4. Dosyayı kaydedin ve kapatın.
5. Düzen *~/Account/Login.cshtml* sayfasında ve açıklamayı Kaldır `<fieldset>` sayfanın sonundaki blok. Kodun açıklamasını kaldırın için kaldırın `@*` koyun ve izleyin karakterler `<fieldset>` blok. Açıklamaları olan kod bloğunu aşağıdaki gibi görünen kaldırıldı: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. Dosyayı kaydedin ve kapatın.

Artık Facebook oturum açma test edebilirsiniz.

1. Sitenin çalıştırması *default.cshtml* sayfasında ve **oturum açma** düğmesi.
2. Üzerinde *oturum açma* sayfasında **başka bir hizmete oturum açmak için kullandığınız** bölümünde, seçin **Facebook** simgesi. 

    Web sayfasının isteği Facebook oturum açma sayfasına yönlendirir.

    [![topSeven oauth 2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Bir Facebook hesabınızda oturum açmak. 

    Kod, kimlik doğrulaması için Facebook belirteci kullanır ve burada Facebook oturum açma bilgilerinizi sitenizin oturum açma ile ilişkilendirebilirsiniz bir sayfasına döndürür. Kullanıcı adı veya e-posta adresinizi içine doldurulur **e-posta** formdaki alan.

    [![topSeven oauth 5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. Seçin **ilişkilendirmek** düğmesi. 

    Tarayıcı giriş sayfasına döndürür ve günlüğe kaydedilir.

    [![topSeven oauth 3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Twitter oturum açma bilgileri sağlamak için:** 

1. Gözat [Twitter geliştiriciler site](https://dev.twitter.com/).
2. Seçin **uygulama oluşturma** bağlamak ve siteye oturum.
3. Üzerinde **uygulama oluşturma** oluşturmak, doldurmak **adı** ve **açıklama** alanları.
4. İçinde **Web sitesi** sitenizin URL'sini girin (örneğin, [ `http://www.example.com` ](http://www.example.com)). 

    > [!NOTE]
    > Siteniz yerel olarak test ediyorsanız (gibi bir URL kullanarak `http://localhost:12345`), Twitter URL'si kabul. Ancak, yerel bir geri döngü IP adresini kullanmanız mümkün olabilir (örneğin `http://127.0.0.1:12345`). Bu, uygulamanızı yerel olarak test etme işlemini basitleştirir. Ancak, yerel site bağlantı noktası numarası her değiştiğinde güncellemeniz gerekecektir **Web sitesi** uygulamanızın alan.
5. İçinde **geri çağırma URL'si** alan, kullanıcıların Twitter ile oturum açtıktan sonra dönmek için istediğiniz Web sayfası için bir URL girin. Örneğin, kullanıcıların (Bu, oturum açma durumu algılar) başlangıç sitesi giriş sayfasına göndermek için girdiğiniz aynı URL'yi girin. **Web sitesi** alan.
6. Koşulları kabul edin ve seçin **kendi Twitter uygulamanızı oluşturun** düğmesi.
7. Üzerinde **uygulamalarım** giriş sayfası, oluşturduğunuz uygulamayı seçin.
8. Üzerinde **ayrıntıları** için sekmesinde, kaydırın ve seçin **oluşturma My erişim belirteci** düğmesi.
9. Üzerinde **ayrıntıları** sekmesinde, kopya **tüketici anahtarı** ve **tüketici gizli** uygulamanız için değerleri ve bunları bir geçici bir metin dosyasına yapıştırın. Bu değerler, Web sitesi kodunuzda Twitter sağlayıcıya ileteceksiniz.
10. Twitter site çıkın.

Kullanıcıların Twitter hesaplarını kullanarak siteye bağlanmanız mümkün olacaktır, böylece artık iki Web sitenize değişiklik.

1. Web siteniz Düzenle  *\_AppStart.cshtml* sayfasında ve Twitter OAuth sağlayıcısı için kodun açıklamasını kaldırın. Açıklamalı olmayan kod bloğu şöyle görünür: 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. Kopyalama **tüketici anahtarı** değeri olarak bir Twitter uygulaması değerinden `consumerKey` parametre (tırnak işaretleri içinde).
3. Kopyalama **tüketici gizli** değeri olarak bir Twitter uygulaması değerinden `consumerSecret` parametresi.
4. Dosyayı kaydedin ve kapatın.
5. Düzen *~/Account/Login.cshtml* sayfasında ve açıklamayı Kaldır `<fieldset>` sayfanın sonundaki blok. Kodun açıklamasını kaldırın için kaldırın `@*` koyun ve izleyin karakterler `<fieldset>` blok. Açıklamaları olan kod bloğunu aşağıdaki gibi görünen kaldırıldı: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. Dosyayı kaydedin ve kapatın.

Artık Twitter oturum açma test edebilirsiniz.

1. Çalıştırma *default.cshtml* sitenizin sayfasını ve **oturum açma** düğmesi.
2. Üzerinde *oturum açma* sayfasında **başka bir hizmete oturum açmak için kullandığınız** bölümünde, seçin **Twitter** simgesi. 

    Web sayfasının isteği oluşturduğunuz uygulama için bir Twitter oturum açma sayfasına yönlendirir.

    [![topSeven oauth 4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Bir Twitter hesabınızda oturum açmak.
4. Kod kullanıcının kimliğini doğrulamak için Twitter belirtecini kullanır ve size bir sayfaya burada ilişkilendirebilirsiniz Web sitesi hesabınız ile oturum açma bilgilerinizi döndürür. Ad veya e-posta adresinizi içine doldurulur **e-posta** formdaki alan.

    [![topSeven oauth 5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. Seçin **ilişkilendirmek** düğmesi. 

    Tarayıcı giriş sayfasına döndürür ve günlüğe kaydedilir.

    [![topSeven oauth 3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>Haritalar Yardımcısını kullanarak eşlemeleri ekleme

Web Pages 2, Web sayfaları sitesinde için eklentiler paketi olan ASP.NET Web Yardımcıları kitaplığı eklemeleri içerir. Bunlardan biri tarafından sağlanan bir eşlemesi bileşeni olan `Microsoft.Web.Helpers.Maps` sınıfı. Kullanabileceğiniz `Maps` bir adres veya boylam ve enlem koordinatları kümesini dayalı haritaları oluşturmak için sınıf. `Maps` Sınıfının doğrudan Bing, Google, MapQuest ve Yahoo gibi popüler harita altyapıları çağırmanızı sağlar.

Yeni `Maps` sınıfı Web siteniz Web Yardımcıları kitaplığı 2 sürümünü yüklemelisiniz. Bunu yapmak için şu anda yayımlanmış sürümü yüklemek için yönergeleri gidin [ASP.NET Web Yardımcıları Kitaplığı](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) ve sürüm 2 yükleyin.

Bir sayfaya eşleme ekleme adımlarını çağırırsınız, harita altyapıları bağımsız olarak aynıdır. Eşleme sayfanıza bir JavaScript dosya başvurusu eklemeniz yeterlidir ve işleyen bir çağrı ekleyin `<script>` sayfanızda etiketler. Eşleme sayfanızda, ardından kullanmak istediğiniz harita altyapısı çağırın.

Aşağıdaki örnek, bir adresini temel alan bir haritası işleyen bir sayfa ve boylam ve enlem koordinatlarına göre bir haritası işleyen başka bir sayfa oluşturma işlemi gösterilmektedir. Adres eşleme örneği Google Haritalar ve Bing Haritalar koordinat eşleme örneği kullanır. Kod aşağıdaki öğelere dikkat edin:

- Çağrı `Assets.AddScript` iki eşlemeyi sayfalarının üstünde. Bu yöntem bir başvuru ekler *jquery 1.6.2.min.js* bulunan dosya **başlangıç sitesi** şablon tarafından gereken `Maps` sınıfı.
- Çağrı `Assets.GetScripts` Düzen dosyası yöntemi. Bu yöntem işler `<script>` iki eşleme sayfalarında etiketleyin.
- Çağrı `@Maps.GetGoogleHtml` ve `@Maps.GetBingHtml` eşleme sayfaları yöntemleri. Bir adresi eşlemek için bir adres dize geçmesi gerekir. Koordinatları eşlemek için boylam ve enlem geçmelidir koordinatları. Bing Haritalar altyapısı için bir anahtar da geçmelidir (ücretsiz kaydolarak, erişmenizi [Bing Haritalar geliştiriciler site](https://www.microsoft.com/maps/developers/web.aspx)). Benzer şekilde, diğer bir harita alt yapılarının yöntemler çalışır (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).

Eşleme sayfaları oluşturmak için:

1. Temelinde bir Web sitesi oluşturmayı **başlangıç sitesi** şablonu.
2. Adlı bir dosya oluşturun *MapAddress.cshtml* sitenin kök. Bu sayfayı, kendisine geçirdiğiniz bir adresini temel alarak bir harita oluşturur.
3. Aşağıdaki kod, var olan içeriğin üzerine dosyasına kopyalayın. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. Adlı bir dosya oluşturun  *\_MapLayout.cshtml* sitenin kök. Bu sayfa iki eşlemeyi sayfalarının düzen sayfası olacaktır.
5. Aşağıdaki kod, var olan içeriğin üzerine dosyasına kopyalayın. 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. Adlı bir dosya oluşturun *MapCoordinates.cshtml* sitenin kök. Bu sayfayı, kendisine geçirdiğiniz koordinatları kümesini temel bir harita oluşturur.
7. Aşağıdaki kod, var olan içeriğin üzerine dosyasına kopyalayın. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

Eşleme sayfalarınızı test etmek için:

1. Çalıştırırsanız *MapAddress.cshtml* dosya.
2. Posta adresi, durum veya il ve posta kodu gibi bir tam adres dizesi girin ve ardından **harita,** düğmesi. Sayfa Google Maps bir harita oluşturur: 

    [![topseven maphelper 1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. Belirli bir konumun enlem ve boylam koordinatları kümesini bulun.
4. Çalıştırırsanız *MapCoordinates.cshtml*. Koordinatları girin ve ardından **harita,** düğmesi. Sayfa Bing Haritalar'dan bir harita oluşturur: 

    [![topseven maphelper 2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>Web sayfaları uygulamalarını yan yana çalıştırma

Web Pages 2 uygulamalarını yan yana çalıştırma olanağı ekler. Bu, Web sayfaları 1 uygulamalarınızı çalıştırmak, yeni Web Pages 2 uygulamalar oluşturmak ve bunların tümünün aynı bilgisayarda çalıştırma devam sağlar.

WebMatrix ile Web sayfaları 2 Beta yüklediğinizde unutmayın gereken bazı noktalar şunlardır:

- Varsayılan olarak, bilgisayarınızda sürüm 2 uygulamaları olarak mevcut Web Pages uygulamaları çalıştırın. (Sürüm 2 için derlemeleri GAC'ye yüklenmiş ve otomatik olarak kullanılır.)
- Web Pages sürümünü 1 (varsayılan yerine, önceki noktaya olduğu gibi) kullanarak bir site çalıştırmak istiyorsanız, bunu yapmak için siteyi yapılandırabilirsiniz. Sitenizi zaten yoksa bir *web.config* dosya sitesinin kök dizininde, yeni bir tane oluşturun ve aşağıdaki XML'i dosyayı, var olan içeriğin üzerine kopyalayın. Site zaten varsa bir *web.config* ekleyin bir `<appSettings>` öğesi için aşağıdakine benzer `<configuration>` bölümü.

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  '-'Sürümünde belirtmezseniz *web.config* dosyası, bir site, bir sürüm 2 site olarak dağıtılır. (Sürüm 2 derlemeleri kopyalanır *bin* klasöründe sitesi dağıtıldı.)
- Web Matrix sürüm 2 Beta dahil Web sayfaları 2 sürüm derlemeleri sitenin site şablonları kullanarak oluşturduğunuz yeni uygulamalar *bin* klasör.

Genel olarak, her zaman uygun derlemelere sitenin yüklemek için NuGet kullanarak siteniz ile kullanmak için Web sayfaları hangi sürümünü denetleyebilir *bin* klasör. Paketler için ziyaret edin [NuGet.org](http://NuGet.org).

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>Mobil cihazlar için sayfa oluşturma

Web Pages 2, mobil veya diğer cihazlar üzerinde işleme içerik için özel görüntüler oluşturmanızı sağlar.

`System.Web.WebPages` Ad alanı, görüntü modları ile çalışmanıza olanak sağlayan aşağıdaki sınıfları içerir: `DefaultDisplayMode`, `DisplayInfo`, ve `DisplayModes`. Bu sınıfların doğrudan kullanabilir ve belirli cihazlar için doğru çıktıyı işlemeden kod yazın.

Alternatif olarak, cihaza özgü sayfaları böyle bir dosya adlandırma deseni kullanarak oluşturabileceğiniz: <em>FileName.</em> <em>Mobil</em><em>.cshtml</em>. Örneğin, bir sayfanın bir adlı iki sürümü oluşturabilirsiniz <em>MyFile.cshtml</em> adlı bir <em>MyFile.Mobile.cshtml</em>. Çalışma zamanında, bir mobil cihaz istediğinde <em>MyFile.cshtml</em>, Web sayfalarının içeriğini işler <em>MyFile.Mobile.cshtml</em>. Aksi takdirde, <em>MyFile.cshtml</em> işlenir.

Aşağıdaki örnek, mobil cihazlar için içerik sayfası ekleyerek mobil işleme olanağı gösterilmektedir. *Page1.cshtml* içerik gezinti kenar çubuğu içerir. *Page1.Mobile.cshtml* aynı içerik içeriyor, ancak kenar atlar.

Derleme ve kod örneğini çalıştırmak için:

1. Bir Web sayfaları sitesinde adlı bir dosya oluşturun *Page1.cshtml* ve kopyalama *Page1.cshtml* örnekte içine içerik.
2. Adlı bir dosya oluşturun *Page1.Mobile.cshtml* ve kopyalama *Page1.Mobile.cshtml* örnekte içine içerik. Sayfayı mobil sürümü daha küçük bir ekranda daha iyi işleme için Gezinti bölümde atlar dikkat edin.
3. Bir masaüstü tarayıcısı çalıştırın ve göz atın *Page1.cshtml*.
4. Bir mobil tarayıcı (veya bir mobil cihaz öykünücüsünü) çalıştırın ve göz atın *Page1.cshtml*. Sayfayı mobil sürümü bu kez Web sayfalarını işleyen dikkat edin. 

    > [!NOTE]
    > Mobil sayfalar test etmek için bir masaüstü bilgisayarda çalışan bir mobil cihaz simülatörünü kullanabilirsiniz. Bu aracı, mobil cihazlarda görüntülendiği şekilde, web sayfaları test sağlar (diğer bir deyişle, genellikle bir daha küçük alan görüntüler). Simülatör biri [kullanıcı aracısı değiştirici eklenti](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) Mozilla Firefox olanak sağlayan çeşitli mobil tarayıcılar Firefox Masaüstü sürümünden öykünmek.

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml* Masaüstü tarayıcısında işlenen:

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* Firefox tarayıcısı bir Apple iPhone simülatörü görünümünde görüntülenir. İstek için olsa da *Page1.cshtml*, uygulama işler *Page1.Mobile.cshtml*.

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

### <a name="aspnet-web-pages-1-resources"></a>ASP.NET Web sayfaları 1 kaynakları

> [!NOTE]
> Çoğu Web sayfaları 1 programlama ve API kaynaklarına yine de Web sayfaları 2 için geçerlidir.

- [ASP.NET Web sayfaları programlama giriş](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>WebMatrix kaynakları

- [WebMatrix 2 yenilikler nelerdir?](http://webmatrix.com/next)
- [Microsoft WebMatrix Site](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Microsoft WebMatrix ile Web geliştirme başlangıç](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(eksiksiz bir örnek Web sayfalarını uygulama içerir)
