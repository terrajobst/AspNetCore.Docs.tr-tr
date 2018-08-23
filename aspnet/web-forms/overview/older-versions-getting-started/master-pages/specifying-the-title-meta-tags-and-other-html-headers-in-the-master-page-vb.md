---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: (VB) ana sayfada başlık, Meta etiketler ve diğer HTML üst bilgilerini belirtme | Microsoft Docs
author: rick-anderson
description: Çeşitli tanımlamak için farklı teknik bakan &lt;baş&gt; içerik sayfasından ana sayfa öğeleri.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 1b28e6df0e0ab25e8292b6523c9ad7482301a511
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753944"
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>(VB) ana sayfada başlık, Meta etiketler ve diğer HTML üst bilgilerini belirtme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> Çeşitli tanımlamak için farklı teknik bakan &lt;baş&gt; içerik sayfasından ana sayfa öğeleri.


## <a name="introduction"></a>Giriş

Visual Studio 2008'de oluşturulan yeni ana sayfa varsa, varsayılan olarak, iki ContentPlaceHolder denetimi: adlı bir `head`, bulunur `<head>` öğesi; ve adlı bir `ContentPlaceHolder1`, Web formu içinde yapıldı. Amacı `ContentPlaceHolder1` sayfa tarafından temelinde özelleştirilebilir Web formunda bir bölge tanımlamaktır. `head` ContentPlaceHolder sağlayan özel içeriğinizi sayfalarına `<head>` bölümü. (Kuşkusuz, bu iki ContentPlaceHolder değiştirilebilir veya kaldırıldı ve ek ContentPlaceHolder ana sayfasına eklenebilir. Ana sayfamızı `Site.master`, şu anda dört ContentPlaceHolder denetim yok.)

HTML `<head>` öğesi belgesi bir parçası değil web sayfası belge hakkında bilgiler için bir depo olarak hizmet verir. Dosya meta bilgileri arama motorları veya iç gezginler ve RSS akışları, JavaScript ve CSS gibi dış kaynaklara yönelik bağlantılar tarafından kullanılan, bu web sayfasının başlığı gibi bilgileri içerir. Bu bilgilerin bazıları, Web sitesindeki tüm sayfalara ilgili olabilir. Örneğin, genel olarak aynı CSS kurallarını ve her ASP.NET sayfası için JavaScript dosyaları almak isteyebilirsiniz. Ancak, bazı bölümleri vardır `<head>` sayfaya özgü öğesi. Sayfa başlığı birincil bir örnektir.

Bu öğreticide, genel ve özel sayfa nasıl tanımlanacağını inceleyeceğiz `<head>` ana sayfa ve içerik sayfalarını bölüm biçimlendirme.

## <a name="examining-the-master-pagesheadsection"></a>Ana sayfanın İnceleme`<head>`bölümü

Visual Studio 2008 tarafından oluşturulan varsayılan ana sayfa dosyası aşağıdaki biçimlendirmede içeren kendi `<head>` bölümü:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

Dikkat `<head>` öğesi içeren bir `runat="server"` özniteliği, bir sunucu denetimi (yerine statik HTML) olduğunu gösterir. Tüm ASP.NET sayfaları türetilmesi [ `Page` sınıfı](https://msdn.microsoft.com/library/system.web.ui.page.aspx), bulunan `System.Web.UI` ad alanı. Bu sınıf içeren bir [ `Header` özelliği](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) sayfanın erişim sağlayan `<head>` bölge. Kullanarak `Header` biz bir ASP.NET sayfasının başlığı ayarlayın veya ek biçimlendirme işlenmiş ekleme özelliği `<head>` bölümü. Ardından, bir içerik sayfasının özelleştirmek için mümkündür `<head>` sayfanın içinde biraz kod yazarak öğe `Page_Load` olay işleyicisi. 1. adımda sayfanın başlığı programlanarak nasıl ayarlanacağını inceleyeceğiz.

Gösterilen biçimlendirme `<head>` yukarıdaki öğesi de içerir adlı ContentPlaceHolder denetiminin `head`. Özel içerik için içerik sayfalarına ekleyebilirsiniz gibi bu ContentPlaceHolder denetim gerekli değil `<head>` öğesi programlı olarak. Bununla birlikte, statik biçimlendirme eklemek için bir içerik sayfasının gereken yere durumlarda kullanışlı olduğunu `<head>` öğesi static işaretleme olarak eklenebilir bildirimli olarak karşılık gelen içerik denetimi yerine programlı olarak.

Ek olarak `<title>` öğesi ve `head` ContentPlaceHolder, ana sayfaya ait `<head>` öğe herhangi içermelidir `<head>`-tüm sayfalar için ortak düzeyi biçimlendirme. Sitemizin, tüm sayfaları içinde tanımlanan CSS kurallarını kullanın. `Styles.css` dosya. Sonuç olarak, güncelleştirdik `<head>` öğesinde [ *ana sayfalarıyla Site geneli bir düzen oluşturma* ](creating-a-site-wide-layout-using-master-pages-vb.md) karşılık gelen bir ekleme için öğreticiyi `<link>` öğesi. Bizim `Site.master` ana sayfa geçerli `<head>` biçimlendirme aşağıda gösterilmektedir.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>1. adım: bir içerik sayfasının başlığı ayarlama

Web sayfasının başlığı aracılığıyla belirtilen `<title>` öğesi. Her sayfanın başlığını uygun bir değere ayarlamak önemlidir. Bir sayfasını ziyaret ederek başlığını tarayıcının başlık çubuğunda görüntülenir. Ayrıca, bir sayfa yer işareti ekleme, tarayıcılar başlığı önerilen adı için yer işareti olarak kullanın. Ayrıca, birçok arama motorları, arama sonuçlarını görüntülerken başlığı gösterir.

> [!NOTE]
> Varsayılan olarak, Visual Studio ayarlar `<title>` "Adsız Sayfa" ana sayfasına öğe. Benzer şekilde, yeni ASP.NET sayfaları sahip kullanıcıların `<title>` "Adsız sayfasına" çok ayarlayın. Bu sayfanın başlığını uygun bir değere ayarlamak unutmak çok kolaydır bu nedenle, çok sayıda sayfa var. "Adsız Sayfa" başlıklı Internet Bu konu başlığı ile web sayfaları için Google arama kabaca 2,460,000 sonuçlarını döndürür. Microsoft'un bile, "Adsız Sayfa" başlıklı web sayfaları yayımlama açıktır. Bu makalenin yazıldığı sırada, Google arama Microsoft.com etki alanında böyle 236 web sayfaları bildirdi.


Bir ASP.NET sayfasının başlığı aşağıdaki yollardan birini belirtebilirsiniz:

- Değeri içinde doğrudan yerleştirerek `<title>` öğesi
- Kullanarak `Title` özniteliğini `<%@ Page %>` yönergesi
- Sayfanın programlı olarak ayarlama `Title` kullanan kod gibi özellik `Page.Title="title"` veya `Page.Header.Title="title"`.

İçerik sayfaları sahip değilseniz bir `<title>` öğesi, ana sayfada tanımlanır. Bu nedenle, bir içerik sayfasının başlığı ayarlamak için kullanabilir `<%@ Page %>` yönergesinin `Title` özniteliği veya programlı olarak ayarlayın.

### <a name="setting-the-pages-title-declaratively"></a>Başlığı bildirimli olarak ayarlama

Bir içerik sayfasının başlığı bildirimli olarak aracılığıyla ayarlanabilir `Title` özniteliği [ `<%@ Page %>` yönergesi](https://msdn.microsoft.com/library/ydy4x04a.aspx). Bu özellik, doğrudan değiştirerek ayarlanabilir `<%@ Page %>` yönerge veya Özellikler penceresi aracılığıyla. Her iki yaklaşım göz atalım.

Kaynağı görünümünden bulun `<%@ Page %>` bildirim temelli işaretleme sayfanın üst kısmındaki yönergesi. `<%@ Page %>` Yönergesi `Default.aspx` izler:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

`<%@ Page %>` Yönergesi, ayrıştırma ve derleme sayfası ASP.NET altyapısı tarafından kullanılan bir sayfaya özgü öznitelikleri belirtir. Bu, ana sayfa dosyası, kendi kod dosyasını ve diğer bilgileri arasında başlığını konumunu içerir.

Visual Studio ayarlar yeni bir içerik sayfası oluştururken, varsayılan olarak `Title` "Adsız Sayfa" özniteliği. Değişiklik `Default.aspx`'s `Title` "Ana sayfa öğreticilere" özniteliği "Adsız sayfasından" ve ardından sayfanın bir tarayıcı aracılığıyla görüntüleyin. Şekil 1 yeni sayfa başlığının yansıtan, tarayıcının başlık çubuğunda gösterilir.


![Tarayıcının başlık çubuğunda gösterdiğini &quot;ana sayfa öğreticiler&quot; yerine &quot;adsız sayfa&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**Şekil 01**: tarayıcının başlık çubuğunda artık "Adsız Sayfa" yerine "ana sayfa öğreticiler" gösterilir


Başlığı, Özellikler penceresinden de ayarlanabilir. Özellikler penceresinden belge içeren yük sayfa düzeyi özellikleri için aşağı açılan listeden seçin `Title` özelliği. Şekil 2 gösteren Özellikler penceresi sonra `Title` "Ana sayfa öğreticilerine" ayarlayın.


![Özellikler penceresinde başlık çok yapılandırabilirsiniz](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**Şekil 02**: Başlık Özellikler penceresinden çok yapılandırabilirsiniz


### <a name="setting-the-pages-title-programmatically"></a>Başlığı programlı olarak ayarlama

Ana sayfanın `<head runat="server">` biçimlendirme çevrilir bir [ `HtmlHead` sınıfı](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) sayfa ASP.NET altyapısı tarafından işlendiğinde örneği. `HtmlHead` Sınıfında bir [ `Title` özelliği](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) değeri yansıtılır işlenen içinde `<title>` öğesi. Bu özellik, bir ASP.NET sayfasının arka plan kod sınıftan erişilebilir `Page.Header.Title`; bu aynı özelliği de erişilebilir aracılığıyla `Page.Title`.

Uygulama Başlığı programlı olarak ayarlama, gitmek için `About.aspx` sayfa arka plan kod sınıfı ve sayfa için bir olay işleyicisi oluşturun `Load` olay. Ardından, başlığı ayarlayın "ana sayfa öğreticiler:: hakkında:: *tarih*" burada *tarih* geçerli tarihtir. Bu kodu ekledikten sonra `Page_Load` olay işleyicisi aşağıdaki gibi görünmelidir:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

Şekil 3 gösterir tarayıcının başlık çubuğunda ziyaret `About.aspx` sayfası.


![Başlığı programlı olarak ayarlama ve geçerli tarih içerir](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**Şekil 03**: başlığı programlı olarak ayarlama ve geçerli tarih içerir


## <a name="step-2-automatically-assigning-a-page-title"></a>2. adım: Sayfa başlığı otomatik olarak atama

1. adımda gördüğümüz gibi bir başlığı bildirimli olarak veya programlama yoluyla ayarlanabilir. Ancak, açıkça bir şey daha açıklayıcı başlığını değiştirme unutursanız, sayfanız "Adsız Sayfa" varsayılan başlık olacaktır. Biz açıkça değerini belirtmeyin olay, ideal olarak, başlığı otomatik olarak bizim için ayarlanır. Örneğin, çalışma zamanında, başlığı "Adsız Sayfa" ise, biz ASP.NET sayfanın dosya adı ile aynı olacak şekilde otomatik olarak güncelleştirilen başlığı isteyebilirsiniz. Güzel bir haberimiz var biraz otomatik olarak atanan başlığı olması mümkündür önceden çalışmanın ile olmasıdır.

Tüm ASP.NET web sayfaları türetilmesi `Page` System.Web.UI ad alanı sınıf. `Page` Sınıfı gibi temel özellikleri gösterir ve ASP.NET sayfası tarafından gereken en düşük işlevi tanımlar `IsPostBack`, `IsValid`, `Request`, ve `Response`, birçok diğerlerinin yanı sıra. Aktardığınızda genellikle, bir web uygulamasındaki her sayfa, ek özellikler veya işlevsellikler gerektirir. Bu sağlama yaygın bir yolu, bir özel taban sayfası sınıfı oluşturmaktır. Türetilen bir sınıf oluşturduğunuz özel taban sayfası sınıftır `Page` sınıfı ve ek işlevler içerir. Bu temel sınıf oluşturulduktan sonra ASP.NET sayfaları türetmeniz olabilir (yerine `Page` sınıfı), böylece, ASP.NET sayfaları için genişletilmiş işlevselliği sunar.

Bu adımda başlığı yoksa açıkça ayarlanmamışsa başlığı ASP.NET sayfa dosya adına otomatik olarak ayarlar temel bir sayfa oluşturacağız. Adım 3, site haritasına dayalı başlığı ayarlanırken arar.

> [!NOTE]
> Bu öğretici serisinin oluşturma ve özel taban sayfası sınıflarını kullanarak kapsamlı bir incelemesini kapsamıdır. Daha fazla bilgi için okuma [bilgisayarınızı ASP.NET sayfalarının arka plan kod sınıfları için özel bir temel sınıf kullanarak](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Taban sayfası sınıfı oluşturma

Bizim ilk görev genişleten bir sınıfı, bir taban sayfası sınıfı oluşturmaktır `Page` sınıfı. Başlangıç ekleyerek bir `App_Code` klasör Çözüm Gezgini'nde proje adının üzerine sağ tıklayın, ASP.NET klasörü Ekle'ı seçerek ve ardından seçerek projenize `App_Code`. Ardından, sağ `App_Code` klasörü ve adlı yeni bir sınıf ekleyin `BasePage.vb`. Şekil 4'te gösterildiği sonra Çözüm Gezgini `App_Code` klasörü ve `BasePage.vb` sınıfı eklendi.


![App_Code klasörünü ve BasePage adlı bir sınıf ekleyin](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**Şekil 04**: ekleme bir `App_Code` klasörü ve adlı bir sınıf `BasePage`


> [!NOTE]
> Visual Studio, proje yönetimi iki modu destekler: Web sitesi projeleri ve Web Uygulama projeleri. `App_Code` Klasör Web sitesi projesi modeliyle kullanılmak üzere tasarlanmıştır. Web uygulaması proje modeli kullanıyorsanız yerleştirmeniz `BasePage.vb` sınıf dışında bir şey adlı bir klasörde `App_Code`, gibi `Classes`. Bu konu hakkında daha fazla bilgi için [bir Web uygulaması projesi için bir Web sitesi projesi geçirme](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx).


ASP.NET sayfalarının arka plan kod sınıflar için temel sınıf olarak özel taban sayfası proxy'sisi işlevi görmesi nedeniyle, genişletmek ihtiyaç duyduğu `Page` sınıfı.


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

Bir ASP.NET sayfasına istenen her aşama, istenen sayfayı HTML'e işlenen olarak sonuçlanan bir dizi boyunca devam eder. Geçersiz kılarak biz bir aşamada dokunabilir `Page` sınıfın `OnEvent` yöntemi. Tabanımızda sayfasında şimdi onu açıkça tarafından belirtilmemiş başlığını otomatik olarak ayarlayın `LoadComplete` aşama (hangi tahmin, sistemdeki sonra `Load` aşama).

Bunu gerçekleştirmek için geçersiz kılma `OnLoadComplete` yöntemi ve aşağıdaki kodu girin:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

`OnLoadComplete` Yöntemi başlatır, belirleyerek `Title` özelliği henüz açıkça ayarlanmamış. Varsa `Title` özelliği `Nothing`, boş bir dize ya da "Adsız Sayfa" değerine sahip istenen ASP.NET sayfası dosya adı için atanır. İstenen ASP.NET sayfası - fiziksel yolu `C:\MySites\Tutorial03\Login.aspx`, örneğin - aracılığıyla erişilebilir durumda `Request.PhysicalPath` özelliği. `Path.GetFileNameWithoutExtension` Yöntemi yalnızca dosya adı kısmını çekmek için kullanılır ve bu dosya daha sonra atanan `Page.Title` özelliği.

> [!NOTE]
> Başlık biçimi geliştirmek için bu mantıksal geliştirmek için davet ettiğim. Örneğin, sayfanın dosya adı ise `Company-Products.aspx`, yukarıdaki kod, başlığı "Şirket-Ürün" oluşturur, ancak "Şirket ürünleri" olduğu gibi bir boşluk ile dash ideal geçecekti. Ayrıca, büyük/küçük bir değişiklik olduğunda, bir boşluk eklemeyi düşünün. Diğer bir deyişle, dosya adı dönüştüren kod eklemeyi göz önünde bulundurun `OurBusinessHours.aspx` için bir başlık, "Mız iş saatleri".


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Taban sayfası sınıfı içerik sayfalarını sahip

Biz artık özel bir ana sayfasından türetmek için sitemizi ASP.NET sayfaları güncelleştirmeniz gerekiyor (`BasePage`) yerine `Page` sınıfı. Her kod arkası sınıfı bu Git gerçekleştirmek ve sınıf bildiriminden değiştirmek için:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

Hedef:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

Bunu yaptıktan sonra bir tarayıcı aracılığıyla sitesini ziyaret edin. Ayarlanmış başlık açıkça ayarlandığında, gibi bir sayfasını ziyaret edin, `Default.aspx` veya `About.aspx`, açıkça belirtilen başlık kullanılır. Ancak bu, varsayılan ("Adsız Sayfa") ayarlanmış başlık değiştirilmediğinden bir sayfasını ziyaret edin, taban sayfası sınıfı başlığı sayfanın dosya adı için ayarlar.

Şekil 5 gösterir `MultipleContentPlaceHolders.aspx` sayfasında bir tarayıcıdan görüntülendiğinde. Başlık tam olarak sayfanın dosya adı (daha az uzantılı) olduğunu unutmayın "MultipleContentPlaceHolders".


[![Bir başlık açıkça belirtilmezse, sayfanın dosya adı otomatik olarak kullanılır](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**Şekil 05**: bir başlık açıkça belirtilmezse, sayfanın dosya adı otomatik olarak kullanılır ([tam boyutlu görüntüyü görmek için tıklatın](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>3. adım: sayfa başlığının Site haritada taşınmaz.

ASP.NET sayfası geliştiricilerinin birlikte (örneğin, SiteMapPath site haritası hakkında bilgi görüntülemek için Web denetimleri (örneğin, bir XML dosyası veya veritabanı tablo) bir dış kaynağa hiyerarşik site haritası tanımlamak bir güçlü site haritası altyapı sunar. Menü ve TreeView denetimleri).

Site haritası yapısı, bir ASP.NET sayfasının arka plan kod sınıfı da bir program aracılığıyla erişilebilir. Bu şekilde size otomatik olarak bir başlığı, ilgili düğüm başlığına site eşlemesinde ayarlayabilirsiniz. Şimdi geliştirmek `BasePage` sınıfı bu işlevselliği sunar, böylece 2. adımda oluşturmuştunuz. Ancak önce site için site haritası oluşturmak oluşturmamız gerekir.

> [!NOTE]
> Bu öğreticide, okuyucu zaten ASP ile alışkın olduğu varsayılır. NET site haritası özellikleri. Site haritası kullanma hakkında daha fazla bilgi için Bilgisayarım çok parçalı makale serisi başvurun [ASP inceleniyor. NET Site gezintisi](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Site Haritası oluşturma

Site haritası sistemi üzerine kurulmuştur [sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), bellek ve kalıcı bir depoya arasında site haritası bilgileri serileştiren mantıksal site haritası API ayırır. .NET Framework ile birlikte gelen [ `XmlSiteMapProvider` sınıfı](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), varsayılan site haritası sağlayıcısı olduğu. Adından da anlaşılacağı gibi `XmlSiteMapProvider` kendi site haritası deposu olarak bir XML dosyası kullanır. Bizim site haritası tanımlamak için bu sağlayıcıyı kullanalım.

Adlı Web sitesinin kök klasöründe bir site haritası dosyası oluşturarak başlayın `Web.sitemap`. Bunu yapmak için Çözüm Gezgini'nde Web sitesi adına sağ tıklayın, yeni öğe Ekle öğesini seçin ve Site Haritası şablonu seçin. Dosyanın nasıl adlandırıldığı emin olun `Web.sitemap` ve Ekle'ye tıklayın.


[![Web sitenizin kök klasörüne birtakım adlı bir dosya ekleyin](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**Şekil 06**: adlı bir dosya ekleme `Web.sitemap` Web sitesinin kök klasöre ([tam boyutlu görüntüyü görmek için tıklatın](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))


Eklemek için aşağıdaki XML `Web.sitemap` dosyası:


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

Bu XML Şekil 7'de gösterilen hiyerarşik site haritası yapısını tanımlar.


![Şu anda oluşan üç Site Haritası düğümlerin Site Haritası](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**Şekil 07**: Site Haritası, şu anda oluşan, üç Site Haritası düğümü


Yeni örnekler eklediğimiz gibi sonraki öğreticilerde site haritası yapısı güncelleştireceğiz.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Ana sayfa gezinti Web denetimleri içerecek şekilde güncelleştirme

Tanımlanan bir site haritası sahibiz, gezinti Web denetimleri içerecek şekilde ana sayfaya güncelleştirelim. Özellikle, bir ListView denetimi sol sütuna sırasız bir listesini içeren bir liste öğesi site eşlemesi içinde tanımlanan her düğüm için işler dersleri bölümünde ekleyelim.

> [!NOTE]
> ListView denetimi, sürüm 3.5 ASP.NET için yeni bir özelliktir. Repeater denetimiyle, ASP.NET'in önceki bir sürümünü kullanıyorsanız, bunun yerine kullanın. ListView denetimi hakkında daha fazla bilgi için bkz. [kullanarak ASP.NET 3.5'ın ListView ve DataPager denetimleri](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Mevcut sırasız liste biçimlendirme dersleri bölümünden kaldırarak başlayın. Ardından, bir ListView denetimi araç kutusundan sürükleyip bırakın dersleri başlığı. ListView araç, bir görünüm denetimleri yanı sıra veri bölümünde bulunur: GridView DetailsView ve FormView. ListView'ın ayarlamak `ID` özelliğini `LessonsList`.

ListView adlı yeni bir SiteMapDataSource denetimine bağlamak veri kaynağı Yapılandırma Sihirbazı ' seçin `LessonsDataSource`. SiteMapDataSource denetim site haritası sisteminden hiyerarşik yapısı döndürür.


[![Bir SiteMapDataSource denetimini LessonsList ListView denetimine bağlama](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**Şekil 08**: SiteMapDataSource denetimini LessonsList ListView denetimine Bağla ([tam boyutlu görüntüyü görmek için tıklatın](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))


SiteMapDataSource denetimi oluşturduktan sonra sırasız bir listesini içeren bir liste öğesi SiteMapDataSource control tarafından döndürülen her düğüm için işler, böylece ListView'ın şablonlarını tanımlamak ihtiyacımız var. Bu, aşağıdaki şablon biçimlendirme kullanarak gerçekleştirilebilir:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

`LayoutTemplate` Sırasız bir listesini için biçimlendirme oluşturur (`<ul>...</ul>`) sırada `ItemTemplate` SiteMapDataSource olarak bir liste öğesi tarafından döndürülen her bir öğe işler (`<li>`), belirli Ders bir bağlantı içerir.

ListView'ın şablonları yapılandırdıktan sonra Web sitesini ziyaret edin. Şekil 9 gösterildiği gibi tek bir madde işaretli öğe, giriş dersleri bölümü içerir. Hakkında ve birden çok ContentPlaceHolder denetimleri dersleri kullanarak nerede? SiteMapDataSource hiyerarşik bir veri kümesini döndürmek için tasarlanmıştır ancak ListView denetimi yalnızca tek bir hiyerarşi düzeyi görüntüleyebilirsiniz. Sonuç olarak, yalnızca ilk düzeyi SiteMapDataSource tarafından döndürülen site haritası düğümlerinin görüntülenir.


[![Tek bir liste öğesi dersleri bölümü içerir](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**Şekil 09**: tek bir liste öğesi dersleri bölümü içerir ([tam boyutlu görüntüyü görmek için tıklatın](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))


Birden çok düzeyi görüntülemek için biz birden çok ListViews içinde iç içe geçirebilirsiniz `ItemTemplate`. Bu teknik, denetlenen [ *ana sayfalar ve Site gezintisi* öğretici](../../data-access/introduction/master-pages-and-site-navigation-vb.md) , my [çalışma ile verileri öğretici serisinin](../../data-access/index.md). Ancak, Bu öğretici dizisinde bizim site haritası yalnızca iki düzeyi içerir: ana (üst düzey); ve her Ders bir alt giriş öğesi olarak. İç içe geçmiş bir ListView oluşturmak yerine, biz bunun yerine başlangıç düğümünün ayarlayarak değil döndürülecek SiteMapDataSource bildirebilirsiniz kendi [ `ShowStartingNode` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) için `False`. Site haritası düğümlerinin ikinci katman döndürerek SiteMapDataSource başlar net etkisidir.

Bu değişikliğe ListView hakkında için madde işaretlerini görüntüler ve birden çok ContentPlaceHolder denetimleri kullanarak dersler, ancak giriş için madde işareti öğesi atlar. Bu sorunu gidermek için açıkça bir madde işareti öğesi için giriş sayfasında ekleyebiliriz `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

Başlangıç düğümünün atlamak için SiteMapDataSource yapılandırma ve açıkça bir giriş madde işareti öğesi ekleme dersleri bölümü artık hedeflenen çıkış görüntüler.


[![Ders bölümüne bir madde işareti öğesi, giriş ve her alt düğüm içerir.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**Şekil 10**: dersleri bölümüne bir madde işareti öğesi, giriş ve her alt düğüm içerir. ([tam boyutlu görüntüyü görmek için tıklatın](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Site haritasına dayalı başlık ayarlama

Site haritası ile yerinde güncelleştiriyoruz bizim `BasePage` site eşlemesinde belirtilen başlık kullanılacak sınıfı. Adım 2'de yaptığımız gibi yalnızca başlığı sayfasında geliştirici tarafından açıkça ayarlanmadı site haritası düğümün başlık kullanılacak istiyoruz. İstenen sayfa açıkça ayarlanmış bir yoksa sayfa başlığı ve 2. adımda yaptığımız gibi biz istenen sayfanın dosya adı (daha az uzantılı) kullanmaya geri döner sonra site haritası içinde bulunamadı. Şekil 11 bu karar alma sürecini gösterir.


![Bir açıkça ayarlamak sayfa başlığı olmaması durumunda karşılık gelen Site Haritası düğümün başlık kullanılır](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**Şekil 11**: bir açıkça ayarlamak sayfa başlığı olmaması durumunda karşılık gelen Site Haritası düğümün başlık kullanılır


Güncelleştirme `BasePage` sınıfın `OnLoadComplete` yöntemi aşağıdaki kodu ekleyin:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

Önceki örneklerde olduğu gibi `OnLoadComplete` yöntemi başlığı açıkça ayarlanmış olup olmadığını belirleyerek başlatır. Varsa `Page.Title` olduğu `Nothing`, boş bir dize veya kod otomatik olarak bir değer atar sonra "Adsız Sayfa" değeri atanır `Page.Title`.

Kullanılacak başlığı belirlemek için kod başvurarak başlatır [ `SiteMap` sınıfı](https://msdn.microsoft.com/library/system.web.sitemap.aspx)'s [ `CurrentNode` özelliği](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx). `CurrentNode` döndürür [ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) örneği şu anda istenen sayfaya karşılık gelen site haritası. Şu anda istenen sayfa varsayılarak site haritası içinde bulundu `SiteMapNode`'s `Title` özellik başlığı için atanır. Şu anda istenen sayfa site eşlemesinde değilse `CurrentNode` döndürür `Nothing` ve (2. adımda yapıldığı gibi) istenen sayfanın dosya adı başlığı olarak kullanılır.

Şekil 12 gösterir `MultipleContentPlaceHolders.aspx` sayfasında bir tarayıcıdan görüntülendiğinde. Bu başlığı açıkça ayarlanmadığından, karşılık gelen site haritası düğümün başlık yerine kullanılır.


![Site Haritası çekilen MultipleContentPlaceHolders.aspx başlığı](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**Şekil 12**: MultipleContentPlaceHolders.aspx başlığı çekilmesini Site Haritası


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>4. adım: Diğer sayfaya özgü biçimlendirme ekleme`<head>`bölümü

Adım 1, 2 ve 3 baktığı özelleştirme sırasında `<title>` sayfa sayfa başına öğe. Ek olarak `<title>`, `<head>` bölüm içerebilir `<meta>` öğeleri ve `<link>` öğeleri. Bu öğreticide daha önce belirtildiği gibi `Site.master`'s `<head>` bölümü içeren bir `<link>` öğesine `Styles.css`. Çünkü bu `<link>` öğe ana sayfasında tanımlanan, bunun içerdiği `<head>` tüm içerik sayfalarının bölümü. Ancak nasıl biz izlememiz ekleme hakkında `<meta>` ve `<link>` sayfa sayfa başına öğe?

Sayfaya özel içeriği eklemek için en kolay yolu `<head>` bölümdür ana sayfasında ContentPlaceHolder denetiminin oluşturarak. Zaten bir tür ContentPlaceHolder sahibiz (adlı `head`). Bu nedenle, özel eklemek için `<head>` biçimlendirme, karşılık gelen oluşturma biçimlendirme oraya ve içerik sayfasındaki denetimi.

Ekleme özel göstermek için `<head>` biçimlendirme bir sayfaya şimdi dahil bir `<meta>` description öğesi geçerli kümemizdeki içerik sayfaları için. `<meta>` Arama sonuçlarını görüntülerken bu bilgileri formunda çoğu arama motorları birleştirmek; description öğesi web sayfası hakkında kısa bir açıklama sağlar.

A `<meta>` description öğesi aşağıdaki biçime sahiptir:


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

Bu işaretleme bir içerik eklemek için yukarıdaki metin eşleyen ana sayfa için içerik denetimi eklemek `head` ContentPlaceHolder. Örneğin, tanımlamak için bir `<meta>` description öğesi için `Default.aspx`, aşağıdaki işaretlemeyi ekleyin:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

Çünkü `head` ContentPlaceHolder HTML sayfasının gövdesi içinde değil, eklenen içerik denetimi için biçimlendirme Tasarım görünümünde görüntülenmez. Görmek için `<meta>` açıklaması öğe ziyaret `Default.aspx` bir tarayıcı aracılığıyla. Sayfası yüklendikten sonra kaynağı görüntüleyin ve unutmayın `<head>` bölüm içerik denetimi belirtilen biçimlendirme içerir.

Eklemek için birkaç dakikanızı `<meta>` açıklama öğeleri `About.aspx`, `MultipleContentPlaceHolders.aspx`, ve `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Program aracılığıyla biçimlendirme ekleme`<head>`bölge

`head` ContentPlaceHolder aktarımlardaki bildirimli olarak ana sayfa için özel biçimlendirme eklemek bize `<head>` bölge. Özel biçimlendirme programlı olarak da eklenebilir. Bu geri çağırma `Page` sınıfın `Header` özelliği döndürür `HtmlHead` ana sayfasında tanımlanan örneği ( `<head runat="server">`).

Programlı olarak içerik ekleme `<head>` bölge, içerik eklemek için dinamik olduğunda yararlıdır. Belki de sayfasını ziyaret ederek kullanıcı temel alır. belki de, bir veritabanından alınır. Nedeni ne olursa olsun, içerik ekleyebilirsiniz `HtmlHead` denetimler ekleyerek kendi `Controls` koleksiyonu şu şekilde:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

Yukarıdaki kod ekler `<meta>` keywords öğesi `<head>` bölgenin, ki bu sayfayı tanımlayan anahtar sözcükleri virgülle ayrılmış bir listesini sağlar. Eklemek için unutmayın bir `<meta>` oluşturduğunuz etiketi bir [ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) örnek olarak ayarlayın, `Name` ve `Content` özellikleri ve eklemeniz `Header`'s `Controls` koleksiyonu. Benzer şekilde, programlı olarak eklemek için bir `<link>` öğesi oluşturmak bir [ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) nesne özelliklerini ayarlayın ve ardından ekleyin `Header`'s `Controls` koleksiyonu.

> [!NOTE]
> Rastgele biçimlendirme eklemek için oluşturun bir [ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) örnek olarak ayarlayın, `Text` özelliği ve ardından ekleyin `Header`'s `Controls` koleksiyonu.


## <a name="summary"></a>Özet

Bu öğreticide eklemek için yol çeşitli incelemiştik `<head>` sayfa sayfa başına bölge biçimlendirme. Ana sayfa içermelidir bir `HtmlHead` örneği (`<head runat="server">`) ile bir ContentPlaceHolder. `HtmlHead` Örneğinin içerik sayfalarını program aracılığıyla erişmek için izin verdiği `<head>` bölge ve kullanıcının sayfası bildirimli ve programlı olarak ayarlamak için başlık; ContentPlaceHolder denetimini eklenecek özel biçimlendirme sağlayan `<head>` bir içerik denetimi bildirimli olarak bölümü.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Başlığı ASP.NET'te dinamik olarak ayarlama](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [ASP inceleniyor. NET Site gezintisi](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [HTML Meta etiketler kullanma](http://searchenginewatch.com/showPage.html?page=2167931)
- [ASP.NET ana sayfaları](http://www.odetocode.com/articles/419.aspx)
- [ASP.NET 3.5'ın kullanarak ListView ve DataPager denetimleri](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [ASP.NET sayfalarının arka plan kod sınıfları için özel bir temel sınıf kullanma](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar 1998'de bu yana birden çok ASP/ASP.NET books ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileri ile çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott, konumunda ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Zack Jones ve Suchi Banerjee yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Önceki](multiple-contentplaceholders-and-default-content-vb.md)
> [İleri](urls-in-master-pages-vb.md)
