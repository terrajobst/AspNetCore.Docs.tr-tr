---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: Üst özellikleri ASP.NET Web Pages 2 | Microsoft Docs
author: microsoft
description: Bu konu ile WebMatr bulunur ve basit bir web programlama çerçevesi ASP.NET Web Pages 2'deki en yeni özelliklere genel bakış sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: e8fc758936953970ff3e9ba289516925dee9ef45
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>ASP.NET Web Pages 2 üst özellikleri
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu makalede ASP.NET Web Pages 2 RC ile birlikte bir basit bir web programlama çerçevesi içinde en yeni özelliklere genel bakış sağlanmaktadır [Microsoft WebMatrix RC 2](https://www.microsoft.com/web/).
> 
> **Neleri kapsar:** 
> 
> - [WebMatrix yükleme](#install)
> - [Yeni ve Gelişmiş Özellikler](#New_and_Enhanced_Features)
> 
>     - [RC sürüm için değişiklikler](#Changes_for_the_RC_Version)
>     - [Beta sürümü için değişiklikleri](#Changes_for_the_Beta_Version)
>     - [Yeni ve güncelleştirilmiş Site şablonlarını kullanma](#templates)
>     - [Kullanıcı girişini doğrulama](#validation)
>     - [Facebook ve OAuth ve Openıd kullanarak diğer sitelere oturum açmayı etkinleştirme](#oauthsetup)
>     - [Maps Yardımcısını kullanarak eşlemeleri ekleme](#maphelper)
>     - [Web Pages uygulamaları yan yana çalıştırma](#sidebyside)
>     - [Mobil cihazlar için sayfaları oluşturma](#mobile)
> - [Ek kaynaklar](#resources)
> 
> > [!NOTE]
> > Bu konuda, WebMatrix, ASP.NET Web Pages 2 kod ile çalışmak için kullandığınız varsayılır. Web sayfaları 1 ile Visual Studio'yu kullanarak Web Pages 2 Web siteleri de oluşturabilirsiniz gibi ancak, sağlayan IntelliSense özellikleri ve hata ayıklama geliştirilmiştir. Visual Studio'da Web sayfaları ile çalışmak için önce Visual Studio 2010 SP1, Visual Web Developer Express 2010 SP1 veya Visual Studio 11 Beta yüklemeniz gerekir. Daha sonra şablonları ve Visual Studio'da ASP.NET MVC 4 ve Web Pages 2 uygulamaları oluşturmak için Araçlar içerir ASP.NET MVC 4 Beta yükleyin.
> 
> 
> *Son güncelleştirme: 18 Haziran 2012*


<a id="install"></a>
## <a name="installing-webmatrix"></a>WebMatrix yükleme

Web sayfalarını yüklemek için Microsoft Web Platformu yükleyicisi yükleme ve yapılandırma web ilgili teknolojileri daha kolay hale getirir ücretsiz bir uygulama olduğu kullanabilirsiniz. Web Pages 2 Beta içerir WebMatrix 2 Beta yükler.

1. Web Platformu Yükleyicisi'nın en son sürümünü yükleme sayfasına göz atın:

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > WebMatrix 1 zaten yüklüyse, bu yükleme için WebMatrix 2 Beta güncelleştirir. 1 veya 2 sürümünü aynı bilgisayara kullanılarak oluşturulan Web siteleri çalıştırabilirsiniz. Daha fazla bilgi için bölümüne bakarak [çalışan Web Pages uygulamaları yan yana](#sidebyside).
2. Seçin **Şimdi Yükle**. 

    Internet Explorer kullanıyorsanız, sonraki adıma gidin. Mozilla Firefox veya Google Chrome gibi farklı bir tarayıcı kullanıyorsanız kaydetmeniz istenir *Webmatrix.exe* bilgisayarınıza dosya. Dosyayı kaydedin ve yükleyici başlatmak için tıklatın.
3. Yükleyiciyi çalıştırmak ve seçmek **yükleme** düğmesi. Bu, WebMatrix ve Web sayfalarını yükler.

## <a id="New_and_Enhanced_Features"></a>Yeni ve Gelişmiş Özellikler

### <a id="Changes_for_the_RC_Version"></a>RC sürüm (Haziran 2012) değişiklikleri

Haziran 2012 RC sürüm sürümde Mart 2012'de serbest Beta sürümü yenileme bazı değişiklikler vardır. Bu değişiklikler şunlardır:

- A `Validation.AddFormError` yöntemi eklendiği `Validation` Yardımcısı. Bu doğrulama el ile yapmanız durumunda faydalı olur (örneğin, sorgu dizesinde geçirilen bir değer doğrulamak) ve tarafından görüntülenen hata iletisine eklemek istediğiniz `Html.ValidationSummary` yöntemi. Daha fazla bilgi için bkz [doğrulama veri olduğunu değil gelen doğrudan kullanıcıların](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) içinde [ASP.NET Web sayfaları (Razor) siteleri uygulamasında kullanıcı girdisi doğrulama](https://go.microsoft.com/fwlink/?LinkId=253002).
- Paketleme ve küçültme için işlevsellik çekirdek ASP.NET Web Pages 2 derlemelerden kaldırılmıştır. Sonuç olarak `Assets` bu belgenin sonraki bölümlerinde listelenen Yardımcısı kullanılabilir değil. Bunun yerine, yüklemeniz gereken [ASP.NET iyileştirme](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) NuGet paketi. Daha fazla bilgi için bkz: [paketleme ve küçültme varlıklar bir ASP.NET Web sayfaları (Razor) sitesinde](https://go.microsoft.com/fwlink/?LinkId=255373).
- ASP.NET Web Pages 2 desteklemek için ek derlemeler eklenmiştir. Bir sitenin daha fazla derlemelerde görebilirsiniz bu değişikliği yalnızca belirgin etkisi olan *bin* bir site oluşturun ya da bir site için bir barındırma sağlayıcısına dağıtmak sonra klasör.

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Beta sürümü (Şubat 2012) değişiklikleri

Şubat 2012'de serbest Beta sürümü yalnızca birkaç değişiklik aralık 2011 yayımlanan Beta sürümünden içerir. Bu değişiklikler şunlardır:

- Razor artık koşullu öznitelikler destekler. Bir HTML öğesi bir öznitelik için bir değer, ayarlarsanız, sunucu kodu çözümlenen `false` veya `null`, ASP.NET değil işlemek öznitelik hiç. Örneğin, bir onay kutusu için aşağıdaki biçimlendirme olduğunu düşünün:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    Varsa değerini `checked1` çözümler `false` veya `null`, `checked` özniteliği değil çizilir. Önemli bir değişiklik budur.
- `Validation.GetHtml` Yöntemi adlandırılmıştır `Validation.For`. Önemli bir değişiklik budur; `Validation.GetHtml` ' nin Beta sürümünde çalışmaz.
- Şimdi içerebilir `~` kullanmadan bir site kökünde başvuru işleci biçimlendirmede `Href` işlevi. (Diğer bir deyişle, Razor ayrıştırıcısı Şimdi Bul çözümlemek ve `~` bir açık yöntem çağrısı gerektirmeden işleci `Href`.) `Href` Yöntemi çalışmaya devam ettiğinden, bu önemli bir değişiklik değildir.

    Örneğin daha önce sahip olduğunuz biçimlendirme şuna benzer:

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    Artık böyle biçimlendirme kullanabilirsiniz:

    `<a href="~/Default.cshtml">Home</a>`
- `Scripts` Varlıklar (kaynak) yönetim için Yardımcısı ile değiştirilmiştir `Assets` Yardımcısı, aşağıdaki gibi biraz farklı yöntemler vardır:

    - İçin `Scripts.Add`, kullanın`Assets.AddScript`
    - İçin `Scripts.GetScriptTags`, kullanın`Assets.GetScripts`

    Önemli bir değişiklik budur; `Scripts` sınıfı'nin Beta sürümünde kullanılamaz. Bu değişiklikle Varlık Yönetimi'ni kullanın, bu belgenin kod örneklerinde güncelleştirildi.

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>Yeni ve güncelleştirilmiş Site şablonlarını kullanma

**Başlangıç sitesi** şablonu güncelleştirildi böylece Web Pages 2 üzerinde çalışan varsayılan olarak. Ayrıca aşağıdaki yeni özellikleri içerir:

- Mobil dostu sayfa işleme. CSS stilleri kullanımıyla ve `@media` Seçicisi **başlangıç sitesi** mobil cihaz ekranlar dahil olmak üzere küçük ekranlarda sayfaları geliştirilmiş işlenmesini sağlar.
- Gelişmiş üyelik ve kimlik doğrulama seçenekleri. Kullanıcıların oturum gibi diğer sitelerdeki sosyal ağ, Twitter, Facebook ve Windows Live hesaplarını kullanarak siteniz uygulamasına izin verebilirsiniz. Daha fazla bilgi için bkz: [etkinleştirme oturumları Facebook ve diğer OAuth ve Openıd kullanarak sitelerden](#oauthsetup) bölümü.
- HTML5 öğeleri.

Yeni **Kişisel Site** şablon kişisel bir blog, bir fotoğraf sayfasına ve Twitter sayfasını içeren bir Web sitesinin oluşturmanızı sağlar. Temel bir site özelleştirebilirsiniz **Kişisel Site** aşağıdakileri yaparak şablonu:

- Düzen dosyasını düzenleyerek site görünümünü değiştirme (*\_SiteLayout.cshtml*) ve stiller dosya (*Site.css*).
- Sitenize işlevselliği ekleme NuGet paketlerini yükleyin. Paketleri yükleme hakkında daha fazla bilgi için ASP.NET Web Yardımcıları kitaplığı da dahil olmak üzere bkz öğretici [Yardımcıları yükleme](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).

Erişim için **Kişisel Site** şablonunu seçin **şablonları** WebMatrix üzerinde **Hızlı Başlangıç** ekran.

[![topseven personalsite 1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

İçinde **şablonları** iletişim kutusunda, seçin **Kişisel Site** şablonu.

[![topseven personalsite 2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

Giriş sayfasının **Kişisel Site** sayfa ve fotoğraf sayfasına Twitter şablon sağlar, blogunuz ayarlamak için bağlantıları izleyin.

[![topseven personalsite 3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>Kullanıcı girişini doğrulama

Web sayfaları 1'de, kullanıcı girişi gönderilen formlarda doğrulamak için kullandığınız `System.Web.WebPages.Html.ModelState` sınıfı. (Bu kod örnekleri çeşitli başlıklı Web sayfaları 1 öğreticide gösterilmiştir [verilerle çalışma](../data/5-working-with-data.md).) Web Pages 2'de bu yaklaşımı kullanmaya devam edebilirsiniz. Ancak, Web Pages 2 kullanıcı girişini doğrulama için Gelişmiş araçlar sunar:

- Yeni doğrulama sınıflar `System.Web.WebPages.ValidationHelper` ve `System.Web.WebPages.Validator`, imkan sağlayan birkaç satır kod ile güçlü doğrulama görevleri yapın.
- İsteğe bağlı olarak, istemci tarafı doğrulama, doğrulama hataları için denetlenecek sunucuya gidiş dönüş gerektiren yerine kullanıcıya anında geri bildirim sağlar. (Güvenlik nedeniyle, denetimleri istemcisinde önceden gerçekleştirilmiş olsa bile doğrulama sunucu üzerinde gerçekleştirilir.)

Yeni doğrulama özellikleri kullanmak için aşağıdakileri yapın:

Sayfanın kodda yöntemlerini kullanarak doğrulanması için bir öğe kaydetmek `Validation` Yardımcısı: `Validation.RequireField`, `Validation.RequireFields` (gerekli olması birden çok öğe kaydetmek için), veya `Validation.Add`. `Add` Yöntemi veri denetleme, farklı alanları, dize uzunluğu denetimleri girişlerinde karşılaştırma türü gibi doğrulama denetimlerini diğer türleri belirtmenize olanak tanır ve düzenleri (normal ifadeler kullanarak). Bazı örnekler şunlardır:

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

Alana özgü hata görüntülemek için arama `Html.ValidationMessage` Doğrulanmakta olan her öğe için biçimlendirme içinde:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

Bir Özet görüntülemek için (`<ul>` listesi) sayfasında, tüm hataların `Html.ValidationSummary` biçimlendirmede:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

Bu adım sunucu tarafında doğrulama uygulamak için yeterli değildir. İstemci tarafı doğrulama eklemek istiyorsanız, aşağıdakileri ayrıca yapın.

İçinde aşağıdaki komut dosyası başvuruları ekleyin `<head>` bir web sayfasının bölümünde. İlk iki komut dosyası başvuruları bir içerik teslim ağı (CDN) sunucuda uzak dosyaları üzerine gelin. Üçüncü başvuru yerel komut dosyasına işaret eder.

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

Yerel bir kopyasını almak için en kolay yolu *jquery.validate.unobtrusive.min.js* kitaplığıdır (örneğin, başlangıç sitesi) site şablonları birini temel alarak yeni bir Web sayfaları site oluşturmak için. Şablon tarafından oluşturulan sitesi içeren *jquery.validate.unobtrusive.js* içinden kopyalayabilirsiniz, sitenize betikleri klasörü, dosyasında.

Web sitenizi kullanıyorsa, bir*\_SiteLayout* sayfa düzeni denetlemek için sayfasında, doğrulama tüm içerik sayfalarına kullanılabilir olmasını sağlamak, bu komut dosyası başvuruları bu sayfadaki ekleyebilirsiniz. Yalnızca belirli sayfalarında doğrulama yapmak istiyorsanız, komut dosyaları yalnızca bu sayfalarda kaydetmek için varlıklar Yöneticisi'ni kullanabilirsiniz. Bunu yapmak için arama `Assets.AddScript(path)` doğrulamak ve her komut dosyalarını bir başvuru istediğiniz sayfasında. Ardından bir çağrı ekleyin `Assets.GetScripts` içinde  *\_SiteLayout* kayıtlı işlemek için sayfa `<script>` etiketler. Daha fazla bilgi için bkz [varlıklar Yöneticisi'ni kaydetme kodlarla](#resmanagement).

Tek bir öğe için biçimlendirme çağrı `Validation.For` yöntemi. Bu yöntem öznitelikleri yayar, jQuery istemci tarafı doğrulama sağlamak için bağlayın. Örneğin:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

Aşağıdaki örnekte bir form üzerinde kullanıcı girişini doğrulayan bir sayfada görüntülenir. Ve bu doğrulama kodu test çalıştırmak için şunu yapın:

1. İçeren WebMatrix 2 sitesi şablonlarından birini kullanarak yeni bir web sitesi oluşturma bir *betikleri* klasörü gibi **başlangıç sitesi** şablonu.
2. Yeni site yeni bir oluşturma *.cshtml* sayfası ve sayfanın içeriğini aşağıdaki kodla değiştirin.
3. Bir tarayıcıda. Sayfayı çalıştırın. Doğrulama etkileri görmek için geçersiz ve geçerli değerleri girin. Örneğin, gerekli bir alan boş bırakın veya bir harf ile girin **krediler** alan.


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

Bir kullanıcı geçerli giriş gönderdiğinde sayfa şöyledir:

[![topSeven geçerli-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

Bir kullanıcı bunu boş sol için gerekli bir alan gönderdiğinde sayfa şöyledir:

[![topSeven geçerli-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

İşte sayfa bir kullanıcı, bir tam sayı dışında bir şey ile gönderdiğinde **krediler** alan:

[![topSeven geçerli-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

Daha fazla bilgi için aşağıdaki blog gönderileri bakın:

- [Güncelleştirilen Web sayfaları v2 doğrulama](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) doğrulama kullanarak ekleme temellerini `Validation` Yardımcısı (yalnızca sunucu tarafı)
- [Güncelleştirilen Web sayfaları v2, bölüm 2 doğrulama](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) istemci tarafı doğrulama ekleme.
- [Güncelleştirilen Web sayfaları v2, bölümü 3 doğrulama](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) doğrulama hataları biçimlendirme.

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>Varlık Yöneticisi'ni kullanarak komut dosyaları kaydetme

Varlık Yöneticisi'ni sunucu kodunda kaydetmek ve istemci komut dosyalarını işlemek için kullanabileceğiniz yeni bir özelliktir. Bu özellik, tek bir sayfada çalışma zamanında birleştirilir birden çok dosya (örneğin, Düzen sayfaları, içerik sayfaları, yardımcıların, vb.) koddan ile çalışırken yararlıdır. Varlık Yöneticisi'ni komut dosyaları doğru şekilde başvurulduğundan ve verimli bir şekilde işlenmiş sayfasında, hangi kod dosyaları bağımsız olarak bunlar gelen adlandırılır kaç kez bunlar olarak adlandırılır emin olun veya kaynak dosyaları düzenler. Varlık Yöneticisi'ni de işler `<script>` böylece sayfayı hızlı bir şekilde (komut dosyaları işleme sırasında indirmeden) yükleyebilir ve komut dosyaları işlemeden önce çağrıldıklarında oluşabilecek hatalarını önlemek için tamamlandıktan doğru yerde etiketler.

Örneğin, bir JavaScript dosyası çağıran özel bir yardımcı oluşturun ve bu yardımcı içerik sayfa kodunuzdaki üç farklı konumlarda çağırın varsayalım. Kaydetmek için varlıklar Yöneticisi'ni kullanmazsanız yardımcı, üç farklı komut dosyasını çağıran `<script>` aynı komut dosyasını tüm noktasına işlenmiş sayfanızda görünecektir etiketler. Ayrıca, nerede bağlı olarak `<script>` etiketleri oluşturulan sayfada eklenir, hataları, komut dosyası sayfa tam yüklenmeden önce belirli sayfa öğelerini erişmeye çalıştığında oluşabilir. Komut dosyasını kaydetmek için varlıklar Yöneticisi kullanıyorsanız, bu sorunları kaçının.

Bunu yaparak, bir komut dosyası varlıklar Yöneticisi ile kaydedebilirsiniz:

- Komut dosyası başvurmalıdır kodda çağrı `Assets.AddScript` yöntemi.
- İçinde bir  *\_SiteLayout* sayfasında, çağrı `Assets.GetScripts` işlenecek yöntemi `<script>` etiketler. 

    > [!NOTE]
    > Çağrı put `Assets.GetScripts` çok son öğesi olarak `<body>` öğesinin  *\_SiteLayout* sayfası. Bu sayfa yardımcı daha hızlı yüklenir ve komut dosyası hataları önlemenize yardımcı olur.

Aşağıdaki örnek, varlıklar Yöneticisi'ni nasıl çalıştığını gösterir. Kod aşağıdaki öğeleri içerir:

- Adlı özel bir yardımcı `MakeNote`. Bu yardımcı kaydırma tarafından kutu içinde bir dize oluşturur. bir `div` çevresinde öğesi, ekleyerek bir sınır ile biçimlendirilmiş &quot;Not:&quot; ona. Yardımcı ayrıca çalışma zamanı davranışı notu ekler bir JavaScript dosyası çağırır. Komut dosyası ile başvuru yerine bir `<script>` etiketi, yardımcı çağırarak betik kaydeder `Assets.AddScript` .
- Bir JavaScript dosyası. Bu yardımcı tarafından çağrılan dosyasıdır ve geçici olarak sırasında Not öğelerinin yazı tipi boyutunu artırır bir `mouseover` olay.
- Başvuruda bulunan bir içerik sayfasını*\_SiteLayout* sayfasında, bazı içerikler gövdesinde oluşturur ve ardından çağırır `MakeNote` Yardımcısı.
- A  *\_SiteLayout* sayfası. Bu sayfa, ortak bir üstbilgisi ve sayfa düzeni yapısını sağlar. Ayrıca bir çağrı içerir `Assets.GetScripts`, varlıklar Yöneticisi'ni komut dosyası nasıl işlediğini olduğu bir sayfasında çağırır.

Örneği çalıştırmak için:

1. Boş bir Web Pages 2 Web sitesi oluşturun. WebMatrix kullanabilirsiniz **boş Site** bu şablonu.
2. Adlı bir klasör oluşturun *betikleri* sitedeki.
3. İçinde *betikleri* klasörünü adlı bir dosya oluşturun *Test.js*, kopya *Test.js* içine örnekten içerik ve dosyayı kaydedin...
4. Adlı bir klasör oluşturun *uygulama\_kod* sitedeki.
5. İçinde *uygulama\_kod* klasörünü adlı bir dosya oluşturun *Helpers.cshtml*, örnek kodu buraya kopyalayın ve adlı bir klasöre kaydedin *uygulama\_kod*kök klasöründe.
6. Sitenin kök klasöründe adlı bir dosya oluşturun  *\_SiteLayout.cshtml,* örneği içine kopyalayın ve dosyayı kaydedin.
7. Site kök adlı bir dosya oluşturun *ContentPage.cshtml*, örnek kodu ekleyin ve dosyayı kaydedin.
8. Çalıştırma *ContentPage* bir tarayıcıda. Geçirilen için dize `MakeNote` Yardımcısı, paketlenmiş bir not olarak işlenir.
9. Fare işaretçisini Not geçirin. Komut dosyası geçici olarak Not yazı tipi boyutunu artırır.
10. İşlenen sayfanın kaynağı görüntüleyin. Çağrı yerleştirdiğiniz nedeniyle `Assets.GetScripts`, işlenen `<script>` çağırır etiketi *Test.js* sayfasının gövdesindeki çok son öğedir.

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

Aşağıdaki ekran görüntüsü gösterildiği *ContentPage.cshtml* bir tarayıcıda Not fare işaretçisini tutun:

[![topSeven resmgr 1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Facebook ve OAuth ve Openıd kullanarak diğer sitelere oturum açmayı etkinleştirme

Web Pages 2 üyelik ve kimlik doğrulama için Gelişmiş seçenekler sağlar. Ana olduğunu yeni yeniliktir [OAuth](http://oauth.net/) ve [Openıd](http://openid.net/) sağlayıcıları. Bu sağlayıcılar kullanarak, sitenizi Facebook, Twitter, Windows Live, Google ve Yahoo varolan kimlik bilgilerini kullanarak içine kullanıcılar oturum sağlayabilirsiniz. Örneğin, bir Facebook hesabı kullanarak oturum açın, kullanıcılar yalnızca kendi kullanıcı bilgilerinin nerede girmeleri Facebook oturum açma sayfasına yönlendiren bir Facebook simgesi seçebilirsiniz. Bunlar daha sonra Facebook oturum açma hesabıyla sitenizdeki ilişkilendirebilirsiniz. Bir ilişkili Web Pages üyelik özelliklerine kullanıcıları (sosyal ağ sitelerinde oturum açmayı dahil) çoklu oturum açma bilgileri ilişkilendirebilirsiniz Web sitenizde tek bir hesap yeniliktir.

Bu görüntü oturum açma sayfasından gösterir **başlangıç sitesi** şablonu, bir kullanıcı bir dış hesapla günlük kaydını etkinleştirmek için bir Facebook, Twitter veya Windows Live simgesi seçim burada:

[![topSeven oauth 1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

OAuth ve Openıd üyeliği birkaç satırlık bir kod kullanarak etkinleştirebilirsiniz. Özellikler ve yöntemler OAuth çalışmak için kullanın ve Openıd sağlayıcılarının olan `WebMatrix.Security.OAuthWebSecurity` sınıfı.

Ancak, diğer siteleri oturum açmayı etkinleştirmek için kod kullanmak yerine, yeni sağlayıcıları ile çalışmaya başlamak için önerilen yöntem yeni kullanmaktır **başlangıç sitesi** WebMatrix 2 Beta ile dahil olan şablon. **Başlangıç sitesi** şablonu tam üyelik altyapı içerir, kullanıcıların oturum yerel kimlik bilgileri veya bunları başka bir siteden kullanarak sitenizi uygulamasına izin vermeniz gerekiyor bir oturum açma sayfası, üyelik veritabanının ve tüm kodu ile tamamlandı .

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>Oturum açmalar OAuth ve Openıd sağlayıcılarının kullanarak etkinleştirme

Bu bölümde temel alan bir site dış sitelerden (Facebook, Twitter, Windows Live, Google veya Yahoo) oturum açmasına izin vermek nasıl bir örnek sağlar **başlangıç sitesi** şablonu. Başlangıç sitesi oluşturduktan sonra bu (ayrıntıları izleyin) yapın:

- Bir OAuth sağlayıcısı (Facebook, Twitter ve Windows Live) kullanan siteleri için dış sitesinde bir uygulama oluşturun. Bu sitelere yönelik oturum açma özelliği çağırmak için gerekir uygulama anahtarları sağlar. Bir Openıd sağlayıcısı (Google, Yahoo) kullanan siteler için bir uygulama oluşturmak zorunda değildir. Bu sitelerin tümünde için size bir hesap oturum açma için ve geliştirici uygulamaları oluşturmak için olmalıdır. 

    > [!NOTE]
    > Oturum açma bilgileri test etmek için bir yerel Web sitesi URL'si kullanamazlar Windows Live uygulamalar yalnızca bir çalışma Web sitesi için Canlı bir URL kabul edin.
- Web sitenizi birkaç dosyalarında, uygun kimlik doğrulama sağlayıcısını belirtmek için ve oturum açma, kullanmak istediğiniz siteye göndermek için düzenleyin.

**Google ve Yahoo oturum açmayı etkinleştirmek için**:

1. Web sitenizdeki Düzenle  *\_AppStart.cshtml* sayfasında ve çağrısından sonra Razor kod bloğunda kod aşağıdaki iki satırı ekleyin `WebSecurity.InitializeDatabaseConnection` yöntemi. Bu kod Google ve Yahoo Openıd sağlayıcılarının sağlar. 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. İçinde *~/Account/Login.cshtml* sayfasında, aşağıdakiler arasından yorumları Kaldır `<fieldset>` Sayfa sonlarında biçimlendirme bloğu. Kodun açıklamasını kaldırın için kaldırın `@*` koyun ve izleyin karakterler `<fieldset>` bloğu. Sonuçta elde edilen kod bloğu şöyle görünür:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. Ekleme bir `<input>` öğesi Google veya Yahoo sağlayıcının `<fieldset>` grubu *~/Account/Login.cshtml* sayfası. Güncelleştirilmiş `<fieldset>` grubu `<input>` öğeleri Google ve Yahoo görünümler için aşağıdaki örnekteki gibi: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. İçinde *~/Account/AssociateServiceAccount.cshtml* sayfasında, eklemek `<input>` Google veya Yahoo için için öğeleri `<fieldset>` dosyasının sonuna yakın grup. Aynı kopyalayabilirsiniz `<input>` için eklediğiniz öğeleri `<fieldset>` bölümüne *~/Account/Login.cshtml* sayfası. 

    *~/Account/AssociateServiceAccount.cshtml* üzerinde kullanıcıları ilişkilendirebilirsiniz çoklu oturum açma bilgileri diğer sitelerdeki tek bir hesap, Web sayfası oluşturmak istiyorsanız, başlangıç sitesi şablonunda sayfa kullanılabilir.

Artık Google ve Yahoo oturum açmayı test edebilirsiniz.

1. Çalıştırma *default.cshtml* sitenizin sayfasını seçin **oturum** düğmesi.
2. Üzerinde *oturum açma* sayfasında **oturum açmak için başka bir hizmet kullanın** bölümünde, ya da seçin **Google** veya **Yahoo** Gönder düğmesi. Bu örnek, Google oturum açma kullanır. 

    Web sayfasının isteği Google oturum açma sayfasına yönlendirir.

    [![topSeven oauth 6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. Varolan bir Google hesabı kimlik bilgilerini girin.
4. Google hesabı bilgilerini kullanmak için ' ı tıklatın Localhost izin vermek istediğiniz isterse **izin**.

    Kod kullanıcının kimliğini doğrulamak için Google belirtecini kullanır ve ardından bu sayfaya, Web sitenizde döndürür. Bu sayfa, kullanıcıların kendi Google oturum açma Web sitenizde var olan bir hesapla ilişkilendirmek sağlar. veya yeni bir hesap ile dış oturum açma ilişkilendirmek için sitenizde kaydedebilirsiniz.

    [![topSeven oauth 5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. Seçin **ilişkilendirmek** düğmesi. Tarayıcı, uygulamanızın giriş sayfasına döndürür.

    [![topSeven oauth 3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven oauth 3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Facebook oturum açma bilgileri etkinleştirmek için**:

1. Git [Facebook geliştiriciler site](https://developers.facebook.com/apps) (henüz oturum açmadıysanız oturum açma).
2. Seçin **yeni uygulama oluşturma** düğmesine tıklayın ve ardından ad ve yeni uygulama oluşturmak için istemleri izleyin.
3. Bölümünde **Facebook ile uygulamanızı nasıl tümleştirecek seçin**, seçin **Web sitesi** bölümü.
4. Doldurmak **Site URL'si** alan sitenizin URL'si (örneğin, [ `http://www.example.com` ](http://www.example.com)). **Etki alanı** alan isteğe bağlıdır; tüm etki alanı için kimlik doğrulaması sağlamak için bunu kullanabilirsiniz (gibi *example.com*). 

    > [!NOTE]
    > Yerel bilgisayarınızda bir URL ile bir site çalıştırıyorsanız, ister `http://localhost:12345` (numarasının olduğu bir yerel bağlantı noktası numarası), bu değer eklemek **Site URL'si** sitenizi test etmek için alan. Ancak, yerel site değişikliklerinizi bağlantı noktası numarasını kurduğunda, güncelleştirmeniz gerekecektir **Site URL'si** uygulamanızın alan.
5. Seçin **Değişiklikleri Kaydet** düğmesi.
6. Seçin **uygulamaları** yeniden sekmesini tıklatın ve ardından, uygulamanız için başlangıç sayfasını görüntüleyin.
7. Kopya **uygulama kimliği** ve **uygulama gizli anahtarı** uygulamanız için değerleri ve bir geçici metin dosyasına yapıştırın. Bu değerler, Web sitesi kodunuzda Facebook sağlayıcıya geçer.
8. Facebook Geliştirici site çıkın.

Böylece kullanıcılar olacak şimdi iki sayfalara siteniz siteye, Facebook hesaplarını kullanarak oturum açabilecek değişiklik.

1. Web sitenizdeki Düzenle  *\_AppStart.cshtml* sayfasında ve Facebook OAuth sağlayıcısı için kodun açıklamasını kaldırın. Uncommented kod bloğunu aşağıdaki gibi görünür: 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. Kopya **uygulama kimliği** değeri olarak Facebook uygulaması değerinden `consumerKey` parametre (tırnak içinde).
3. Kopya **uygulama gizli anahtarı** Facebook uygulaması olarak değerinden `consumerSecret` parametre değeri.
4. Dosyayı kaydedin ve kapatın.
5. Düzenleme *~/Account/Login.cshtml* sayfasında ve açıklamalar kaldırmak `<fieldset>` Sayfa sonlarında bloğu. Kodun açıklamasını kaldırın için kaldırın `@*` koyun ve izleyin karakterler `<fieldset>` bloğu. Açıklamaları olan kod bloğunu aşağıdaki gibi görünüyor kaldırıldı: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. Dosyayı kaydedin ve kapatın.

Artık Facebook oturum açma test edebilirsiniz.

1. Sitenin çalışması *default.cshtml* sayfasında ve seçin **oturum açma** düğmesi.
2. Üzerinde *oturum açma* sayfasında **oturum açmak için başka bir hizmet kullanın** bölümünde, seçin **Facebook** simgesi. 

    Web sayfasının isteği Facebook oturum açma sayfasına yönlendirir.

    [![topSeven oauth 2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Bir Facebook hesabıyla oturum açın. 

    Kod, kimlik doğrulaması için Facebook belirtecini kullanır ve ardından bir sayfaya nerede Facebook oturum açma, sitenizin oturum açma ile ilişkilendirebilirsiniz döndürür. Kullanıcı adınız veya e-posta adresiniz içine girilir **e-posta** form üzerinde alan.

    [![topSeven oauth 5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. Seçin **ilişkilendirmek** düğmesi. 

    Giriş sayfasına tarayıcı döndürür ve kaydedilir.

    [![topSeven oauth 3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Twitter oturumları etkinleştirmek için:** 

1. Gözat [Twitter geliştiriciler site](https://dev.twitter.com/).
2. Seçin **bir uygulama oluşturmak** bağlamak ve siteye oturum açın.
3. Üzerinde **uygulama oluşturma** formunda, doldurmak **adı** ve **açıklama** alanları.
4. İçinde **Web sitesi** alanında, sitenizin URL'sini girin (örneğin, [ `http://www.example.com` ](http://www.example.com)). 

    > [!NOTE]
    > Siteniz yerel olarak test ediyorsanız (gibi bir URL'yi kullanarak `http://localhost:12345`), Twitter URL kabul. Ancak, yerel bir geri döngü IP adresi kullanmak mümkün olabilir (örneğin `http://127.0.0.1:12345`). Uygulamanızı yerel olarak test sürecini basitleştirir. Ancak, yerel site bağlantı noktası numarasını her değiştiğinde güncelleştirmeniz gerekecek **Web sitesi** uygulamanızın alan.
5. İçinde **geri çağırma URL'si** alan, kullanıcıların Twitter oturum açtıktan sonra dönmek için istediğiniz Web sayfası için bir URL girin. Örneğin, kullanıcıların (hangi oturum açma durumlarını algılar) başlangıç sitesi giriş sayfasına göndermek için girdiğiniz aynı URL'yi girin **Web sitesi** alan.
6. Koşulları kabul edin ve seçin **Twitter uygulamanızı oluşturma** düğmesi.
7. Üzerinde **uygulamalarım** giriş sayfasında, oluşturduğunuz uygulamayı seçin.
8. Üzerinde **ayrıntıları** sekmesinde alt kısmına kaydırın ve seçin **oluşturma My erişim belirteci** düğmesi.
9. Üzerinde **ayrıntıları** sekmesinde, kopya **tüketici anahtarı** ve **tüketici gizli** uygulamanız için değerleri ve bir geçici metin dosyasına yapıştırın. Web sitesi kodunuzda bu değerleri Twitter sağlayıcıya geçmesi.
10. Twitter site çıkın.

Kullanıcıların siteye Twitter hesaplarını kullanarak oturum mümkün olmayacaktır şimdi iki sayfalara Web sitenizde değişiklik.

1. Web sitenizdeki Düzenle  *\_AppStart.cshtml* sayfasında ve Twitter OAuth sağlayıcısı için kodun açıklamasını kaldırın. Uncommented kod bloğunu şöyle görünür: 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. Kopya **tüketici anahtarı** değeri olarak Twitter uygulama değerinden `consumerKey` parametre (tırnak içinde).
3. Kopya **tüketici gizli** değeri olarak Twitter uygulama değerinden `consumerSecret` parametresi.
4. Dosyayı kaydedin ve kapatın.
5. Düzenleme *~/Account/Login.cshtml* sayfasında ve açıklamalar kaldırmak `<fieldset>` Sayfa sonlarında bloğu. Kodun açıklamasını kaldırın için kaldırın `@*` koyun ve izleyin karakterler `<fieldset>` bloğu. Açıklamaları olan kod bloğunu aşağıdaki gibi görünüyor kaldırıldı: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. Dosyayı kaydedin ve kapatın.

Artık Twitter oturum açma test edebilirsiniz.

1. Çalıştırma *default.cshtml* sitenizin sayfasını ve **oturum açma** düğmesi.
2. Üzerinde *oturum açma* sayfasında **oturum açmak için başka bir hizmet kullanın** bölümünde, seçin **Twitter** simgesi. 

    Web sayfasının isteği için oluşturduğunuz uygulamayı bir Twitter oturum açma sayfasına yönlendirir.

    [![topSeven oauth 4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Twitter hesabıyla oturum açın.
4. Kod kullanıcının kimliğini doğrulamak için Twitter belirtecini kullanır ve ardından, bir sayfaya burada ilişkilendirebilirsiniz Web sitesi hesabınızla oturum açma döndürür. Ad veya e-posta adresinizi içine girilir **e-posta** form üzerinde alan.

    [![topSeven oauth 5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. Seçin **ilişkilendirmek** düğmesi. 

    Giriş sayfasına tarayıcı döndürür ve kaydedilir.

    [![topSeven oauth 3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>Maps Yardımcısını kullanarak eşlemeleri ekleme

Web Pages 2 eklentileri Web sayfaları site için bir paket, ASP.NET Web Yardımcıları kitaplığı eklemeleri içerir. Bunlardan biri tarafından sağlanan bir bileşenidir eşleme `Microsoft.Web.Helpers.Maps` sınıfı. Kullanabileceğiniz `Maps` bir adres veya boylam ve enlem koordinatları kümesi üzerinde alan eşlemeleri oluşturmak için sınıfı. `Maps` Sınıf doğrudan Bing, Google, MapQuest ve Yahoo gibi popüler harita motorları çağrı sağlar.

Yeni `Maps` sınıfı, Web sitenizdeki önce Web Yardımcıları kitaplığı 2 sürümünü yüklemeniz gerekir. Bunu yapmak için şu anda yayımlanmış sürümü yüklemek için yönergeleri gidin [ASP.NET Web Yardımcıları Kitaplığı](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) ve sürüm 2 yükleyin.

Bir sayfaya eşleme ekleme adımları hangi harita motorları bağımsız olarak çağrı aynıdır. Yalnızca bir JavaScript dosyası başvurusu eşleme sayfanıza ekleyin ve sonra işleyen bir çağrı ekleyin `<script>` sayfanızda etiketler. Ardından, eşleme sayfasında, kullanmak istediğiniz harita altyapısı çağırın.

Aşağıdaki örnekte bir adresini temel alan bir harita işleyen bir sayfa ve boylam ve enlem koordinatlarına göre bir harita oluşturur başka bir sayfaya nasıl oluşturulacağını gösterir. Adres eşleme örneği Google haritalar, ve Bing Haritalar koordinat eşleme örneği kullanır. Kodda aşağıdaki öğelere dikkat edin:

- Çağrı `Assets.AddScript` bu iki eşleme sayfanın üstünde. Bu yöntem bir başvuru ekler *jquery 1.6.2.min.js* bulunan dosya **başlangıç sitesi** şablon tarafından gereken `Maps` sınıfı.
- Çağrı `Assets.GetScripts` düzeni dosyasındaki yöntemi. Bu yöntem işler `<script>` iki eşleme sayfalarında etiketi.
- Çağrı `@Maps.GetGoogleHtml` ve `@Maps.GetBingHtml` eşleme sayfaları yöntemleri. Bir adresi eşlemek için bir adres dizesinin geçmesi gerekir. Koordinatları eşlemek için boylam ve enlem geçmesi gereken koordinatları. Bing Haritalar altyapısı için bir anahtar da geçmelidir (hangi ücretsiz kaydolarak adresindeki alma [Bing Haritalar geliştiriciler site](https://www.microsoft.com/maps/developers/web.aspx)). Bir harita altyapıları için yöntemler benzer şekilde çalışır (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).

Eşleme sayfaları oluşturmak için:

1. Temel bir Web sitesi oluşturmak **başlangıç sitesi** şablonu.
2. Adlı bir dosya oluşturun *MapAddress.cshtml* sitenin kök. Bu sayfa için geçirdiğiniz bir adresi göre bir harita oluşturur.
3. Aşağıdaki kod, var olan içeriğin üzerine dosyasına kopyalayın. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. Adlı bir dosya oluşturun  *\_MapLayout.cshtml* sitenin kök. Bu sayfa, iki eşleme sayfaları Düzen sayfasını olacaktır.
5. Aşağıdaki kod, var olan içeriğin üzerine dosyasına kopyalayın. 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. Adlı bir dosya oluşturun *MapCoordinates.cshtml* sitenin kök. Bu sayfa için geçirdiğiniz koordinatları dizi göre bir harita oluşturur.
7. Aşağıdaki kod, var olan içeriğin üzerine dosyasına kopyalayın. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

Eşleme sayfalarınızı test etmek için:

1. Sayfayı çalıştırmak *MapAddress.cshtml* dosya.
2. Sokak adresi, eyalet veya il ve posta kodu gibi bir tam adresi dizesi girin ve ardından **Map It** düğmesi. Sayfası Google haritaları bir harita oluşturur: 

    [![topseven maphelper 1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. Enlem ve boylam koordinatları kümesi için belirli bir konum bulun.
4. Sayfayı çalıştırmak *MapCoordinates.cshtml*. Koordinatları girin ve ardından **Map It** düğmesi. Sayfası Bing Haritalar'dan bir harita oluşturur: 

    [![topseven maphelper 2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>Web Pages uygulamaları yan yana çalıştırma

Web Pages 2 uygulamalarını yan yana çalıştırma yeteneği ekler. Bu, Web sayfaları 1 uygulamalarınızı çalıştırmak, yeni Web Pages 2 uygulamaları geliştirmek ve bunların tümünün aynı bilgisayarda çalışan devam sağlar.

WebMatrix ile Web Pages 2 Beta yüklediğinizde unutmamanız gereken bazı noktalar şunlardır:

- Varsayılan olarak, bilgisayarınızda sürüm 2 uygulamaları olarak mevcut Web Pages uygulamaları çalıştırın. (Sürüm 2 derlemelerde GAC'ye yüklenir ve otomatik olarak kullanılır.)
- Web sayfaları sürüm 1 (varsayılan yerine, önceki noktası olduğu gibi) kullanarak bir site çalıştırmak istiyorsanız, bunu yapmak için site yapılandırabilirsiniz. Sitenizi zaten yoksa bir *web.config* dosya sitesinin kök dizininde, yeni bir tane oluşturun ve aşağıdaki XML dosyasını buraya var olan içeriğin üzerine kopyalayın. Site zaten varsa bir *web.config* dosya, ekleme bir `<appSettings>` öğesi için aşağıdakine benzer `<configuration>` bölümü.

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
'-'Sürümünde belirtmezseniz *web.config* dosya, bir site bir sürüm 2 site olarak dağıtılır. (Sürüm 2 derlemeler kopyalanır *bin* sitesi dağıtıldı klasöründe.)
- Web Matrix sürüm 2 Beta dahil Web sayfaları sürüm 2 derlemeler sitenin içinde site şablonları kullanarak oluşturduğunuz yeni uygulamalar *bin* klasör.

Genel olarak, her zaman uygun derlemelerini sitenin yüklemek için NuGet kullanarak sitenizle kullanmak için Web sayfalarının hangi sürümü kontrol edebilirsiniz *bin* klasör. Paketler için ziyaret edin [NuGet.org](http://NuGet.org).

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>Mobil cihazlar için sayfaları oluşturma

Web Pages 2, cep telefonu numarası veya diğer cihazları işleme içerik için özel görüntüler oluşturmanızı sağlar.

`System.Web.WebPages` Ad alanı, görüntü modları ile çalışmanıza izin veren aşağıdaki sınıflar içerir: `DefaultDisplayMode`, `DisplayInfo`, ve `DisplayModes`. Bu sınıfları doğrudan kullanabilir ve belirli cihazlar için doğru çıktıyı işlemeden kod yazın.

Böyle bir dosya adlandırma deseni kullanarak aygıta özgü sayfaları alternatif olarak, oluşturabilirsiniz: *FileName.* *Mobil**.cshtml*. Örneğin, bir sayfa, bir adlı iki sürümlerini oluşturabilirsiniz *MyFile.cshtml* ve adlı bir *MyFile.Mobile.cshtml*. Çalışma zamanında, bir mobil cihaz isteğinde bulunduğunda *MyFile.cshtml*, Web sayfalarını işleyen içerikten *MyFile.Mobile.cshtml*. Aksi takdirde, *MyFile.cshtml* işlenir.

Aşağıdaki örnek, mobil cihazlar için bir içerik sayfasını ekleyerek mobil işleme etkinleştirmek gösterilmiştir. *Page1.cshtml* içerik ek olarak bir gezinti Kenar Çubuğu'nu içerir. *Page1.Mobile.cshtml* aynı içerik içeriyor, ancak kenar atlar.

Derleme ve kod örneği çalıştırmak için:

1. Bir Web Pages sitesinde adlı bir dosya oluşturun *Page1.cshtml* ve kopyalama *Page1.cshtml* örneğindeki içine içerik.
2. Adlı bir dosya oluşturun *Page1.Mobile.cshtml* ve kopyalama *Page1.Mobile.cshtml* örneğindeki içine içerik. Sayfanın mobil sürümünü daha küçük bir ekran üzerinde daha iyi işleme için gezinti bölümüne atlar dikkat edin.
3. Bir masaüstü tarayıcısı çalıştırın ve Gözat *Page1.cshtml*.
4. Bir mobil tarayıcı (veya bir mobil aygıt benzeticisi) çalıştıran ve Gözat *Page1.cshtml*. Sayfanın mobil sürümü bu süre Web sayfalarını işleyen dikkat edin. 

    > [!NOTE]
    > Mobil sayfalar sınamak için bir masaüstü bilgisayar üzerinde çalışan bir mobil aygıt benzeticisi kullanabilirsiniz. Bu araç, mobil cihazlarda görüneceği şekilde web sayfalarını test sağlar (diğer bir deyişle, genellikle bir daha küçük alan gösterir). Bir simulator bir örnektir [kullanıcı aracısı değiştirici eklenti](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) Mozilla Firefox olanak sağlayan, Firefox Masaüstü sürümünden çeşitli mobil tarayıcılar öykünmek.

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml* bir masaüstü tarayıcısı çizilir:

[![topseven displaymodes 1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* Firefox tarayıcısı bir Apple iPhone benzeticisi görünümünde görüntülenir. İstek için olsa da *Page1.cshtml*, uygulama işler *Page1.Mobile.cshtml*.

[![topseven displaymodes 2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

### <a name="aspnet-web-pages-1-resources"></a>ASP.NET Web Pages 1 kaynaklar

> [!NOTE]
> Çoğu Web sayfaları 1 programlama ve API kaynakları hala Web Pages 2 için geçerlidir.

- [ASP.NET Web sayfaları programlama giriş](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>WebMatrix kaynakları

- [WebMatrix 2 yenilikler](http://webmatrix.com/next)
- [Microsoft WebMatrix Site](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Microsoft WebMatrix ile Web geliştirme başlangıç](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(uzun süreli örnek Web sayfalarını uygulama içerir)
