---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: Ana sayfalar ve Site gezintisi (C#) | Microsoft Docs
author: rick-anderson
description: Bir ortak kullanıcı dostu Web siteleri bir tutarlı, site genelindeki sayfa düzeni ve gezinti düzeni sahip oldukları özelliğidir. Bu öğreticide adımları ele alınmaktadır y...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: 0ad86398122f3a47721bda689b7f5afc706047bc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816502"
---
<a name="master-pages-and-site-navigation-c"></a>Ana sayfalar ve Site gezintisi (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) veya [PDF olarak indirin](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> Bir ortak kullanıcı dostu Web siteleri bir tutarlı, site genelindeki sayfa düzeni ve gezinti düzeni sahip oldukları özelliğidir. Bu öğreticide nasıl tutarlı bir görünüm arasında kolayca güncelleştirilebilir tüm sayfaları oluşturabilirsiniz arar.


## <a name="introduction"></a>Giriş

Bir ortak kullanıcı dostu Web siteleri bir tutarlı, site genelindeki sayfa düzeni ve gezinti düzeni sahip oldukları özelliğidir. ASP.NET 2.0 uygulama hem bir site genelinde sayfa düzeni ve gezinti düzeni büyük ölçüde kolaylaştıran iki yeni özellik sunuyor: ana sayfalar ve site gezintisi. Ana sayfalar, belirlenen düzenlenebilir bir bölge ile bir site genelinde şablonu oluşturmak geliştiriciler için izin verin. Bu şablon daha sonra bu siteyi ASP.NET sayfaları için uygulanabilir. Ana sayfayı düzenlenebilir bir bölge belirtilen için gibi ASP.NET sayfaları yalnızca içeriği sağlamak zorunda ana sayfadaki diğer tüm biçimlendirme ana sayfa kullanan tüm ASP.NET sayfaları arasında aynıdır. Bu model, geliştiricilerin tanımlayın ve böylece kolayca güncelleştirilebilir tüm sayfaları arasında tutarlı bir görünüm oluşturmak kolaylaştıran bir site genelinde sayfa düzeni merkezileştirme olanak tanır.

[Site gezinti sistemi](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) hem site haritasını tanımlamak sayfa geliştiricileri için bir mekanizma ve o site haritası için programlı olarak Sorgulanacak bir API sağlar. Site haritası ortak bir gezinti kullanıcı arabirimi öğesi içinde bir kısmını veya tamamını menüsünde, ağaç görünümünde ve SiteMapPath kolay hale yeni gezinti Web denetimleri işleyin. Bizim site haritası bir XML biçimli dosya tanımlanması anlamına gelir varsayılan site gezinti sağlayıcısını kullanacağız.

Bu kavramları göstermek ve öğreticiler sitemizin daha kullanışlı hale getirmek için şimdi bu ders, site genelinde sayfa düzeni tanımlama, site haritası uygulama ve'nın gezinme Arabiriminin ekleyerek ayırın. Bu öğreticinin sonunda biz Eğitmen web sayfalarımızın oluşturmaya yönelik bir çarpıcı Web sitesinin tasarımına sahip olursunuz.


[![Bu öğreticinin son sonuç](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Şekil 1**: son sonucu, Bu öğretici ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-site-navigation-cs/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>1. adım: ana sayfa oluşturma

İlk adım, site için sayfayı oluşturmaktır. Yalnızca türü belirtilmiş veri kümesi Web sitemizi oluşur hemen (`Northwind.xsd`, `App_Code` klasör), BLL sınıfları (`ProductsBLL.cs`, `CategoriesBLL.cs`ve benzeri tümü `App_Code` klasör), veritabanı (`NORTHWND.MDF`, `App_Data` klasör), yapılandırma dosyası (`Web.config`) ve CSS stil sayfası dosyası (`Styles.css`). Bu sayfalar ve BLL ve DAL Biz bu örnekleri daha ayrıntılı sonraki öğreticilerde yeniden inceleme bu yana ilk iki öğreticilerden kullanılarak gösteren dosyalar temizlendi.


![Bizim projesindeki dosyalar](master-pages-and-site-navigation-cs/_static/image4.png)

**Şekil 2**: bizim projesindeki dosyalar


Ana sayfa oluşturmak için Çözüm Gezgini'nde proje adının üzerine sağ tıklayın ve Yeni Öğe Ekle öğesini seçin. Ardından ana sayfa türü şablonları listesinden seçin ve adlandırın `Site.master`.


[![Web sitesine yeni bir ana sayfa Ekle](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Şekil 3**: yeni bir ana sayfa Web sitesine ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-site-navigation-cs/_static/image7.png))


Site genelinde sayfa düzeni burada ana sayfasında tanımlayın. Tasarım görünümünü kullanın ve gereksinim duyduğunuz ne olursa olsun düzeni veya Web denetimleri ekleme ya da el ile kaynak görünümü biçimlendirme el ile ekleyebilirsiniz. Ana Sayfam kullanmam [geçişli stil sayfaları](http://www.w3schools.com/css/default.asp) konumlandırma ve dış dosyasında tanımlanan CSS ayarlarla stilleri `Style.css`. Aşağıda gösterilen biçimlendirmeden bildiremez, ancak CSS kurallarını tanımlanan şekilde gezinti `<div>`ait içerik mutlak konumlu soldaki bölmede görünür ve 200 piksel sabit bir genişliğe sahiptir.

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Ana sayfa hem statik sayfa düzeni hem de ana sayfa kullanan ASP.NET sayfaları tarafından düzenlenebilir bir bölge tanımlar. Bu içerik düzenlenebilir bölgesi içinde içerik görülebilir ContentPlaceHolder denetimi tarafından belirtilen `<div>`. Tek bir ContentPlaceHolder ana sayfamızı sahiptir (`MainContent`), ancak ana sayfanın birden çok ContentPlaceHolder sahip olabilir.

Yukarıda girilen biçimlendirme, Tasarım görünümüne geçiş, ana sayfanın düzenini gösterir. Bu ana sayfanın kullanan tüm ASP.NET sayfaları için biçimlendirme belirtme olanağı ile Tekdüzen bu düzen olacaktır `MainContent` bölge.


[![Ana Tasarım görünümü görüntülendiğinde sayfa](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Şekil 4**: ana sayfa, olduğunda görüntülenen aracılığıyla Tasarım görünümü ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-site-navigation-cs/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>2. adım: bir giriş sayfası Web sitesine ekleme

Tanımlanan ana sayfa ile Web sitesi için ASP.NET sayfaları eklemeye hazırız. Ekleyerek başlayalım `Default.aspx`, bizim Web sitesinin giriş sayfası. Çözüm Gezgini'nde proje adının üzerine sağ tıklayın ve Yeni Öğe Ekle öğesini seçin. Dosya adını ve şablon listesi Web formu seçeneğinden çekme `Default.aspx`. Ayrıca, "Ana sayfa seçin" onay kutusunu işaretleyin.


[![Select ana sayfaya onay kutusu denetimi yeni bir Web formu, ekleyin](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Şekil 5**: Select ana sayfaya onay kutusu denetimi yeni bir Web formu, ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-site-navigation-cs/_static/image13.png))


Tamam düğmesine tıklandıktan sonra size bu yeni ASP.NET sayfası kullanması gereken hangi ana sayfa seçin istenir. Projenizde birden çok ana sayfa olabilir ancak biz tek sahip.


[![Bu ASP.NET sayfası kullanması gereken ana sayfa seçin](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Şekil 6**: ASP.NET sayfası kullanması gerekir ana sayfa seçin ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-site-navigation-cs/_static/image16.png))


Ana sayfayı seçtikten sonra yeni ASP.NET sayfaları aşağıdaki biçimlendirme içerir:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

İçinde `@Page` yönergesi yok için kullanılan ana sayfa dosyası bir başvurudur (`MasterPageFile="~/Site.master"`), ve her bir denetimin ana sayfayla tanımlanan ContentPlaceHolder denetim için bir içerik denetimi ASP.NET sayfa biçimlendirmesini içeren `ContentPlaceHolderID` eşleme içeriği için belirli bir ContentPlaceHolder denetim. Biçimlendirme yerleştirdiğiniz içerik denetimi, karşılık gelen ContentPlaceHolder'görünmesini istediğiniz. Ayarlama `@Page` yönergesinin `Title` özniteliği için giriş ve bazı Karşılama içeriği içerik denetimine ekleyin:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

`Title` Özniteliğini `@Page` yönergesi verir bize olsa bile ASP.NET sayfasından başlığı ayarlanacak `<title>` öğesi, ana sayfada tanımlanır. Biz de başlığı programlı olarak kullanarak ayarlayabilirsiniz `Page.Title`. Ayrıca ana sayfanın başvuruları stil sayfaları (gibi `Style.css`) ASP.NET sayfası içinde ana sayfaya hangi dizin bağımsız olarak herhangi bir ASP.NET sayfasında çalıştıkları olacak şekilde otomatik olarak güncelleştirilir.

Sayfamızı bir tarayıcıda nasıl görüneceğini görebiliriz Tasarım görünümüne geçiş. Tasarım ana sayfasında tanımlanan ContentPlaceHolder olmayan biçimlendirme yalnızca düzenlenebilir içerik bölgeler düzenlenebilir ASP.NET sayfasını görüntülemek unutmayın gri gösterilir.


[![ASP.NET sayfası için Tasarım görünümü, düzenlenebilir ve düzenlenemez bölgeleri gösterir.](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Şekil 7**: ASP.NET sayfası gösterir hem düzenlenebilir için Tasarım görünümü ve düzenlenebilir olmayan bölgeleri ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-site-navigation-cs/_static/image19.png))


Zaman `Default.aspx` sayfası tarafından bir tarayıcı, ASP.NET altyapısı sayfanın ana sayfa içeriği ve ASP otomatik olarak birleştirir. NET, içerik ve birleştirilmiş içeriği isteyen tarayıcıya gönderilen son HTML'e işler. Ana sayfanın içeriğinin güncelleştirildiğinde, bu ana sayfanın kullanan tüm ASP.NET sayfaları edenler istedikleri sonraki açışınızda yeni ana sayfa ile içerik remerged içeriklerini olacaktır. Kısacası, ana sayfa modeli için bir tek sayfalı sağlar olması için Düzen şablonunu tanımlanan (ana sayfası) değişiklikleri tüm site genelindeki anında yansıtılır.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Ek ASP.NET sayfaları Web sitesine ekleme

Ek ASP.NET sayfası saptamalar çeşitli raporlama tanıtımları sonunda tutacak siteye eklemek için bir zaman ayırabiliriz. Daha fazla da olacaktır çok 35 tanıtımlar toplam, bu nedenle tüm saplama sayfaları şimdi oluşturmak yerine yalnızca ilk birkaç oluşturun. Daha iyi yönetmek için de olacağından tanıtımları çok sayıda kategorisi, tanıtımları kategorileri için bir klasör ekleyin. Şu an için aşağıdaki üç klasör ekleyin:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Son olarak, yeni dosyalar, Çözüm Gezgini'nde Şekil 8'de gösterildiği gibi ekleyin. Her dosya eklerken "Ana sayfa seçin" onay unutmayın.


![Aşağıdaki dosyaları Ekle](master-pages-and-site-navigation-cs/_static/image20.png)

**Şekil 8**: aşağıdaki dosyaları Ekle


## <a name="step-2-creating-a-site-map"></a>2. adım: bir Site Haritası oluşturma

Süre birkaç sayfaların oluşan bir Web sitesi yönetme sorunlarından biri, site içinde gezinmek ziyaretçiler için basit bir yol sunuyor. Başlangıç olarak, sitenin gezinme yapısı tanımlanmış olması gerekir. Ardından, bu yapı, menüler ya da içerik haritaları gibi gezilebilir kullanıcı arabirimi öğelerine çevrilmelidir. Son olarak, tüm bu işlem bakımı yapılan ve yeni sayfa site ve varolanları kaldırıldı eklendikçe güncelleştirilmiş gerekir. ASP.NET 2.0 önce geliştiriciler için kendi sitenin gezinme yapısı oluşturma, koruma ve içinde gezinebilir kullanıcı arabirimi öğeleri çevirme yoktu. ASP.NET 2. 0'da, ancak, geliştiricilerin çok esnek site gezinti sistemde yerleşik kullanabilir.

ASP.NET 2.0 site gezinti sistem site haritası tanımlayın ve ardından bu bilgileri programlı bir API aracılığıyla erişmek için bir geliştirici için bir yol sağlar. ASP.NET site haritası verileri belirli bir şekilde biçimlendirilmiş bir XML dosyasında depolanan bekliyor. bir site haritası sağlayıcısı ile birlikte gelir. Ancak, site gezinti sistemi kurulu olduğundan [sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) site haritası bilgileri serileştirmek için alternatif yollar destekleyecek şekilde genişletilebilir. Jeff Prosise'nın makale [SQL Site haritası sağlayıcısı, 've edilmemiş bekleyen için](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) gösterir nasıl bir SQL Server veritabanında site haritası depolayan bir site haritası sağlayıcısı oluşturma; başka bir seçenek oluşturmaktır [site haritası sağlayıcısı temel dosya sistemi yapısı](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

Bu öğreticide, ancak birlikte gelen varsayılan site haritası sağlayıcısı ile ASP.NET 2.0 kullanalım. Site haritası oluşturmak için basitçe Çözüm Gezgini'nde proje adının üzerine sağ tıklayın, yeni öğe Ekle öğesini seçin ve Site Haritası seçeneğini belirleyin. Adı olarak bırakın `Web.sitemap` ve Ekle düğmesine tıklayın.


[![Site Haritası projenize ekleyin.](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Şekil 9**: Site Haritası için projenize ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-site-navigation-cs/_static/image23.png))


Site haritası dosyasını bir XML dosyasıdır. Visual Studio IntelliSense için site haritası yapısı sağladığını unutmayın. Site haritası olmalıdır `<siteMap>` düğümü tam olarak bir içermelidir, kök düğümü olarak `<siteMapNode>` alt öğesi. Bu ilk `<siteMapNode>` descendent tercihe bağlı sayıda ardından ögesinin `<siteMapNode>` öğeleri.

Dosya sistemi yapısı taklit etmek için site haritası tanımlayın. Diğer bir deyişle, ekleme bir `<siteMapNode>` her üç klasör ve alt öğe `<siteMapNode>` öğelerin her biri bu klasörleri ASP.NET sayfaları için şu şekilde:

Birtakım


[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

Site haritası site çeşitli bölümlerini tanımlayan bir hiyerarşi Web sitesinin gezinti yapısını tanımlar. Her `<siteMapNode>` öğesinde `Web.sitemap` sitenin gezinme yapısı içinde bir bölümünü temsil eder.


[![Site Haritası, hiyerarşik bir gezinti yapısını temsil eder](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Şekil 10**: Site Haritası, hiyerarşik bir gezinti yapısını temsil eder ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-site-navigation-cs/_static/image26.png))


ASP.NET site haritası'nın yapı .NET Framework'ün ile sunan [SiteMap sınıfı](https://msdn.microsoft.com/library/system.web.sitemap.aspx). Bu sınıf sahip bir `CurrentNode` kullanıcı şu anda ziyaret; bölüm hakkında bilgi döndüren özellik `RootNode` özelliği site haritası kökünü döndürür (Home, bizim site haritası olarak). Hem `CurrentNode` ve `RootNode` özellikleri dönüş [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) özellikleri örneklere, gibi `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`ve benzeri için site haritası izin ver öğrendiniz için hiyerarşi.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>3. adım: Site haritasına dayalı bir menü olarak görüntüleme

ASP.NET 2.0 veri erişimi olabilir program aracılığıyla gerçekleştirilir, ASP.NET ister 1.x, isterse de bildirimli olarak, yeni [veri kaynağı denetimleri](https://msdn.microsoft.com/library/ms227679.aspx). Sınıfları ve diğer kaynaklardan verilerine erişmek için ObjectDataSource Denetimi, ilişkisel veritabanı veri erişim SqlDataSource denetimi gibi birkaç yerleşik veri kaynağı denetimleri vardır. Hatta kendi oluşturabilirsiniz [özel veri kaynağı denetimleri](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

Veri kaynağı denetimleri, ASP.NET sayfası ve temel alınan verileri arasında bir proxy işlevi görür. Bir veri kaynağı denetimin alınan verileri görüntülemek için biz genellikle sayfasına başka bir Web denetim ekleme ve veri kaynak denetimine bağlamak. Web denetimi bir veri kaynak denetimine bağlamak için Web denetimin ayarlamanız yeterlidir `DataSourceID` veri kaynağı denetimin değere `ID` özelliği.

Site haritanın verilerle çalışmaya yardımcı olmak için ASP.NET Web denetimi, Web sitesinin site haritası karşı bağlamak sağlıyor SiteMapDataSource denetimi içerir. İki Web denetimleri menü ve ağaç görünümünde, gezinti kullanıcı arabirim sağlamak üzere yaygın olarak kullanılır. Site haritası verileri bu iki denetimi birine bağlamanız için SiteMapDataSource TreeView birlikte sayfasına eklemeniz yeterlidir veya menü denetimi `DataSourceID` özelliğini uygun şekilde ayarlayın. Örneğin, size ana sayfaya aşağıdaki işaretlemeyi kullanarak bir menü denetimi ekleyebilirsiniz:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

Yayılan HTML denetime zahmetli için biz SiteMapDataSource denetimi Repeater denetiminde için bağlayabilirsiniz şu şekilde:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

SiteMapDataSource denetim site haritası bir hiyerarşi düzeyi teker teker kök site haritası düğümle başlayarak döndürür (Home, bizim site haritası olarak), ardından sonraki düzeyi (temel raporlama, filtreleme raporlar ve özelleştirilmiş biçimlendirme) ve benzeri. SiteMapDataSource için bir yineleyici bağlanırken, ilk düzeyini numaralandırır. başlatır ve döndürülen `ItemTemplate` her `SiteMapNode` ilk düzeyi örneği. Belirli bir özelliğine erişmek için `SiteMapNode`, kullanabiliriz `Eval(propertyName)`, her aldığımız nasıl olduğu `SiteMapNode`'s `Url` ve `Title` HyperLink denetimi için özellikler.

Yineleyici Yukarıdaki örnek aşağıdaki biçimlendirme işlenir:


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

Bu site haritası düğümler (temel raporlama, filtreleme raporlar ve özelleştirilmiş biçimlendirme) oluşturan *ikinci* düzeyini işlenirken, ilk site haritası. Bunun nedeni, SiteMapDataSource'nın `ShowStartingNode` kök site haritası düğümü atlayabilir ve bunun yerine site haritası hiyerarşide ikinci düzey döndürerek başlamak SiteMapDataSource neden özelliği False olarak ayarlanır.

Temel raporlama, filtreleme raporlar ve özelleştirilmiş biçimlendirme için alt görüntülenecek `SiteMapNode` s, başka bir yineleyici ilk yineleyici için 's ekleyebiliriz `ItemTemplate`. Bu ikinci Repeater bağlı `SiteMapNode` örneğinin `ChildNodes` özelliği şu şekilde:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Bu iki yineleyiciler (bazı biçimlendirme uzatmamak için kaldırılmıştır) aşağıdaki biçimlendirme neden:


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

Seçilen stiller CSS kullanma gelen [Rachel Andrew](http://www.rachelandrew.co.uk/)kullanıcının kitap [CSS Antoloji: 101 önemli ipuçları, püf noktalarını, &amp; yönlendirir](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), `<ul>` ve `<li>` öğeleri yerelleştirilirken gibi İşaretleme şu visual çıktıyı üretir:


![İki yineleyiciler ve bazı CSS oluşan bir menü](master-pages-and-site-navigation-cs/_static/image27.png)

**Şekil 11**: menü oluşan iki yineleyiciler ve bazı CSS


Bu menü ana sayfa ve içinde tanımlanan site haritası bağlı `Web.sitemap`seçeneğidir; yani sayfaları kullanan site haritası herhangi bir değişiklik hemen tüm yansıtılır, `Site.master` ana sayfa.

## <a name="disabling-viewstate"></a>ViewState devre dışı bırakma

Tüm ASP.NET denetimleri durumlarına isteğe bağlı olarak kalıcı hale [görüntüleme durumu](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), gizli bir form alanı İşlenmiş HTML olarak serileştirilmiş. Bir veri Web denetimine veri bağlama gibi görünüm durumu program aracılığıyla değiştirilmiş durumlarına geri göndermeler arasında unutmayın denetimler tarafından kullanılır. Görünüm durumu Geri göndermeler arasında anımsanacağını bilgilere sağlarken, istemciye gönderilen ve için ciddi bir sayfa şişmeye neden olabilecek değilse yakından izlenecek biçimlendirmeyi boyutunu artırır. Veri Web denetimleri özellikle GridView biçimlendirme ek kilobayt onlarca bir sayfasına eklemek için özellikle ticaret. Görünüm durumu artırmayı geniş bant veya intranet kullanıcıları için göz ardı edilebilir olmasını sağlarken gidiş dönüş çevirmeli kullanıcılar için birkaç saniye ekleyebilirsiniz.

Görüntüleme durumu, bir tarayıcıda bir sayfasını ziyaret edin ve ardından web sayfası tarafından gönderilen kaynağı görüntüleme etkisini görmek için (Internet Explorer Görünüm menüsüne gidin ve kaynak seçeneğini belirleyin). Ayrıca etkinleştirebilirsiniz [Sayfa izlemeyi](https://msdn.microsoft.com/library/sfbfw58f.aspx) her sayfadaki denetimleri tarafından kullanılan görünüm durumu ayırma görmek için. Görünüm durumu bilgilerini adlı gizli bir form alanı serileştirilmiş `__VIEWSTATE`içinde bulunan bir `<div>` öğesini hemen açılış `<form>` etiketi. Görünüm durumu, yalnızca kullanılan bir Web formu olduğunda kalıcıdır; ASP.NET sayfası içermiyorsa bir `<form runat="server">` olmaz, bildirim temelli söz diziminde bir `__VIEWSTATE` biçimlendirmenin gizli form alanı.

`__VIEWSTATE` Ana sayfası tarafından oluşturulan form alanı için oluşturulan işaretleme sayfanın kabaca 1800 baytı ekler. Bu ek Şişirme öncelikle Repeater denetiminde için son durumu görüntülemek için SiteMapDataSource denetimin içeriğini kalıcı olarak. Bir ek 1800 baytı çok GridView birçok alan ve kayıtlar kullanılırken, büyük bir heyecan almak gibi görünmeyebilir, ancak görünüm durumu 10 veya daha fazla faktörüyle swell kolayca.

Görünüm durumu devre dışı bırakılabilir sayfasının veya denetiminin düzeyinde ayarlayarak `EnableViewState` özelliğini `false`, böylece biçimlendirmenin boyutunu azaltır. Web veri görünüm durumu devre dışı bırakma, denetim her geri göndermede veri Geri göndermeler arasında Web denetimi veri bağlama denetimi devam ederse Web veri görünüm durumu itibaren veri bağlı olmalıdır. Verze technologie ASP.NET içinde 1.x bu sorumluluk; sayfa Geliştirici devlerin üzerinde altına düştü ASP.NET 2. 0'da, ancak veri Web denetimleri kendi veri kaynak denetimine her geri gerekirse yeniden bağlamanız.

Sayfanın görünüm durumu şimdi azaltmak için yineleyici denetimin ayarlamak `EnableViewState` özelliğini `false`. Bu Özellikler penceresinde Tasarımcısı'nda veya kaynağı görünümündeki bildirimli olarak aracılığıyla yapılabilir. Bu değişikliği yaptıktan sonra Repeater'ın bildirim temelli biçimlendirme gibi görünmelidir:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Bu değişiklik, sayfa ait durum boyutu yalnızca bir küçültülebilir görünüm işlenen sonra % 97 oranında tasarruf 52 bayt boyutu görünümünde durum! Bu seri boyunca öğreticilerde veri Web denetimleri görünüm durumu varsayılan olarak biçimlendirmenin boyutunu azaltmak için devre dışı bırakırız. Örneklerin çoğu `EnableViewState` özellik ayarlanacak `false` ve bunu Bahsetme bitti. Görünüm durumu ele senaryolarında olduğu sırada verileri etkinleştirilmeli olan yalnızca bir kez Web denetimi beklenen işlevselliği sağlamak için.

## <a name="step-4-adding-breadcrumb-navigation"></a>4. adım: İçerik haritalı gezinme ekleme

Ana sayfaya tamamlamak için bir içerik haritası gezinme UI öğesi her sayfaya ekleyelim. İçerik haritası, kullanıcılar geçerli konumlarına site hiyerarşisi içinde hızlı bir şekilde gösterir. Bir içerik haritası, ASP.NET 2.0 kolay yalnızca ekleme, sayfaya; bir SiteMapPath denetimi ekleyin kod gereklidir.

Sitemizi için bu denetim üst bilgiye ekleyin. `<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

İçerik haritası kullanıcı tüm kök en çok ziyaret site haritası hiyerarşisi yanı sıra o site haritası düğümün "öğelerinden," geçerli sayfayı gösterir (Home, bizim site haritası olarak).


![Geçerli sayfa içerik haritası görüntüler ve üst sitedeki hiyerarşisi eşleme](master-pages-and-site-navigation-cs/_static/image28.png)

**Şekil 12**: geçerli sayfa içerik haritası görüntüler ve üst sitedeki hiyerarşisi eşleme


## <a name="step-5-adding-the-default-page-for-each-section"></a>5. adım: her bölüm için varsayılan sayfa ekleme

Sitemizi öğreticilerde farklı kategorilere temel raporlama, filtreleme, özel biçimlendirme, vb. bir klasör için her kategorinin ve ASP.NET sayfaları, klasörün içindeki olarak ilgili öğreticiler ile ayrılır. Ayrıca, her bir klasör içeren bir `Default.aspx` sayfası. Bu varsayılan sayfa için şimdi geçerli bölüm için öğreticileri görüntüleyin. Diğer bir deyişle, için `Default.aspx` içinde `BasicReporting` klasör biz bağlantılar haritamın `SimpleDisplay.aspx`, `DeclarativeParams.aspx`, ve `ProgrammaticParams.aspx`. Burada, tekrar kullanabiliriz `SiteMap` sınıfı ve site haritası dayanarak bu bilgiyi görüntülemek için Web denetimi veri tanımlanan `Web.sitemap`.

Şimdi yeniden ancak bu kez, başlık ve açıklama öğreticiler görüntüleyeceğiz Repeater'da kullanma sırasız bir listesini görüntüler. Biçimlendirme ve bu işlem gerçekleştirmek için kod her yinelenmesi olması gerektiğinden `Default.aspx` sayfasında, biz kapsülleyen bu UI mantığı bir [kullanıcı denetimi](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Adlı Web sitesi bir klasör oluşturun `UserControls` ve yeni bir öğe türü adlı Web kullanıcı denetimi eklemek için `SectionLevelTutorialListing.ascx`, aşağıdaki işaretlemeyi ekleyin:


[![UserControls klasöre yeni bir Web kullanıcı denetimi Ekle](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Şekil 13**: yeni bir Web kullanıcı denetimi Ekle `UserControls` klasörü ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-site-navigation-cs/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs


[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

Önceki Repeater örnekte biz bağlı `SiteMap` Repeater veri bildirimli olarak; `SectionLevelTutorialListing` kullanıcı, ancak netimi Bunu programlı olarak. İçinde `Page_Load` olay işleyicisi, bir onay yapılır bu sayfayı URL ile eşleşen bir düğüme site haritası s emin olmak için. Karşılık gelen sahip olmayan bir sayfasında bu kullanıcı denetimi kullanılıyorsa `<siteMapNode>` girişi `SiteMap.CurrentNode` döndüreceği `null` ve yineleyici için hiçbir veri bağlanacak. Yaşıyoruz varsayılarak bir `CurrentNode`, biz bağlama kendi `ChildNodes` Repeater koleksiyonu. Bizim site haritası ayarlandığından şekilde `Default.aspx` sayfasıdır her bölümdeki öğreticiler konusu bölüm içindeki tüm üst düğümü, bu kodu bağlantıları ve açıklamaları tüm bölümün öğreticiler, aşağıdaki ekran görüntüsünde gösterildiği gibi görüntülenir.

Bu Repeater oluşturulduktan sonra açın `Default.aspx` klasörler sayfalarında Tasarım görünümüne gidin ve kullanıcı denetiminin tasarım yüzeyine Çözüm Gezgini'nden, öğretici listenin görünmesini istediğiniz sürüklemeniz yeterlidir.


[![Kullanıcı denetimi eklenmiş Default.aspx sahiptir.](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Şekil 14**: kullanıcı Denetim sahip eklenmiş `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-site-navigation-cs/_static/image34.png))


[![Temel raporlama öğreticileri listelenir](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Şekil 15**: temel raporlama öğreticiler listelenir ([tam boyutlu görüntüyü görmek için tıklatın](master-pages-and-site-navigation-cs/_static/image37.png))


## <a name="summary"></a>Özet

Şimdi, tanımlanan site haritası ve tam ana sayfa ile tutarlı sayfa düzeni ve gezinti düzeni için verilerle ilgili öğreticilerimizden sahip. Sitemize eklediğimiz kaç sayfaları bağımsız olarak, site genelinde sayfa düzeni veya site gezinti bilgilerini güncelleştirmek, bu Bilgi Merkezi nedeniyle, hızlı ve basit bir süreç kullanılır. Sayfa düzeni bilgileri ana sayfada özellikle tanımlı `Site.master` ve içinde harita site `Web.sitemap`. Yazılacak gerekmedi *herhangi* bu site genelinde sayfa düzeni ve gezinti mekanizması elde etmek için kod ve Visual Studio'da tam WYSIWYG tasarımcı desteği biz korur.

İş mantığı katmanı ve veri erişim katmanı tamamlamış ve tanımlanmış bir tutarlı sayfa düzeni ve site gezintisi sahip olmak, genel raporlama desenleri keşfetmeye başlamak hazırız. İçinde [sonraki](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) üç öğreticiler baktığımızda en temel raporlama görevleri GridView DetailsView, içinde BLL hizmetinden alınan verileri görüntüleme ve FormView denetler.

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET ana sayfaları genel bakış](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [ASP.NET 2.0 ana sayfalar](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2.0 tasarım şablonları](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [ASP.NET sitesi gezintiye genel bakış](https://msdn.microsoft.com/library/e468hxky.aspx)
- [ASP.NET 2.0 İnceleme kullanıcının Site gezintisi](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [ASP.NET 2.0 Site Gezinti özellikleri](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [ASP.NET görüntüleme durumu anlama](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Nasıl yapılır: ASP.NET sayfası için izlemeyi etkinleştirme](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [ASP.NET kullanıcı denetimleri](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Liz Shulok Dennis Patterson ve Hilton Giesenow yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](creating-a-business-logic-layer-cs.md)
> [İleri](creating-a-data-access-layer-vb.md)
