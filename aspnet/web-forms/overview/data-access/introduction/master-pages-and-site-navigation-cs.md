---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: Ana sayfalar ve Site gezintisi (C#) | Microsoft Docs
author: rick-anderson
description: "Bir ortak kullanıcı dostu Web siteleri tutarlı, site genelindeki sayfa düzeni ve gezinti düzeni sahip oldukları özelliğidir. Bu öğretici nasıl göründüğünü y..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: 32ddda8d883a99805d2448c9673e585bfe9ef2f4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="master-pages-and-site-navigation-c"></a>Ana sayfalar ve Site gezintisi (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) veya [PDF indirin](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> Bir ortak kullanıcı dostu Web siteleri tutarlı, site genelindeki sayfa düzeni ve gezinti düzeni sahip oldukları özelliğidir. Bu öğretici nasıl tutarlı bir görünüm kolayca güncelleştirilebilir tüm sayfalara oluşturabileceğiniz arar.


## <a name="introduction"></a>Giriş

Bir ortak kullanıcı dostu Web siteleri tutarlı, site genelindeki sayfa düzeni ve gezinti düzeni sahip oldukları özelliğidir. ASP.NET 2.0 hem bir site genelinde sayfa düzeni ve gezinti düzeni uygulama büyük ölçüde kolaylaştırma iki yeni özellik sunar: ana sayfalar ve site gezintisi. Ana sayfalar belirlenen düzenlenebilir bölgeleri ile site genelinde şablonu oluşturmak geliştiriciler izin verir. Bu şablon ASP.NET sayfaları sitedeki uygulanabilir. Ana sayfaya düzenlenebilir bölgeleri belirtilen için gibi ASP.NET sayfaları yalnızca içerik sağlamak zorunda tüm ana sayfa biçimlendirmede ana sayfa kullanan tüm ASP.NET sayfaları arasında aynıdır. Bu model, geliştiricilerin tanımlayın ve böylelikle kolayca güncelleştirilebilir tüm sayfaları arasında tutarlı bir görünüm oluşturmak daha kolay hale bir site genelinde sayfa düzeni merkezileştirme olanak tanır.

[Site gezinti sistemi](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) hem site haritası tanımlamak sayfa geliştiricileri için bir mekanizma ve o site haritası için programlı olarak Sorgulanacak bir API sağlar. Menü, TreeView ve SiteMapPath kolaylaştıran yeni gezinti Web denetimleri, ortak bir gezinti kullanıcı arabirimi öğesi site eşlemesinde bölümünü veya tümünü işleyebilir. Biz bizim site haritası bir XML biçimli dosyasında tanımlanan anlamı varsayılan site gezinti sağlayıcısı kullanırsınız.

Bu kavramları göstermek ve öğreticiler Web sitemizi daha kullanılabilir hale getirmek için şimdi bir site genelinde sayfa düzeni tanımlama, site haritası uygulama ve kullanıcı Arabirimi Gezinti ekleme Bu ders ayırın. Bu öğreticide sonuna kadar biz de öğretici web sayfalarında oluşturmaya yönelik bir çarpıcı Web sitesi tasarımı sahip olacaksınız.


[![Bu öğreticinin son sonucu](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Şekil 1**: sonuç, Bu öğretici ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-site-navigation-cs/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>1. adım: ana sayfa oluşturma

İlk adım, site için ana sayfa oluşturmaktır. Bizim Web sitesi yalnızca yazılan veri kaynağının veri kümesi oluşur hemen (`Northwind.xsd`, `App_Code` klasör), BLL sınıfları (`ProductsBLL.cs`, `CategoriesBLL.cs`ve benzeri tümü `App_Code` klasör), veritabanı (`NORTHWND.MDF`, `App_Data` klasör), yapılandırma dosyası (`Web.config`) ve bir CSS stil sayfası dosyası (`Styles.css`). Bu sayfaları ve biz Bu örnekler daha ayrıntılı sonraki öğreticilerde yeniden inceleme bu yana, DAL ve BLL ilk iki öğreticileri kullanılarak gösteren dosyalar temizlendi.


![Projemizin dosyaları](master-pages-and-site-navigation-cs/_static/image4.png)

**Şekil 2**: Projemizin dosyaları


Bir ana sayfa oluşturmak için Çözüm Gezgini'nde proje adına sağ tıklayın ve yeni öğe Ekle'yi seçin. Ardından ana sayfa şablonları listesinden seçin ve adlandırın `Site.master`.


[![Web sitesine yeni bir ana sayfa ekleyin](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Şekil 3**: yeni bir ana sayfa Web sitesine ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-site-navigation-cs/_static/image7.png))


Site genelinde sayfa düzeni burada ana sayfasında tanımlayın. Tasarım görünümünü kullanın ve ihtiyacınız ne olursa olsun düzeni veya Web denetimleri ekleme ya da el ile kaynak görünümü biçimlendirme el ile ekleyebilirsiniz. Ana sayfamda kullanmam [geçişli stil sayfası](http://www.w3schools.com/css/default.asp) konumlandırma ve dış dosyasında tanımlanan CSS ayarlarla stilleri için `Style.css`. Aşağıda gösterilen biçimlendirmeden bildiremez olsa da, CSS kuralları tanımlanan şekilde gezinti `<div>`ait içerik soldaki bölmede görünür ve sabit genişlik 200 piksel sahip olmasını mutlak olarak konumlandırılmış.

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Bir ana sayfa statik sayfa düzeni ve ana sayfayı kullanan ASP.NET sayfaları tarafından düzenlenebilir bölgeler tanımlar. Bu içerik düzenlenebilir bölgeleri içindeki içeriği görülebilir ContentPlaceHolder denetimi belirtilir `<div>`. Tek bir ContentPlaceHolder ana sayfamızı sahiptir (`MainContent`), ancak ana sayfanın birden çok ContentPlaceHolders için sahip olabilir.

Yukarıda girilen biçimlendirme ile Tasarım görünümüne geçiş yapmak ana sayfa düzeni gösterilir. Bu ana sayfa kullanan tüm ASP.NET sayfaları için biçimlendirme belirtme olanağı ile bu Tekdüzen düzeni olacaktır `MainContent` bölge.


[![Tasarım görünümü görüntülendiğinde ana sayfa,](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Şekil 4**: ana sayfa, ne zaman görüntülenebilir aracılığıyla tasarım görüntüleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-site-navigation-cs/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>2. adım: Web sitesine bir giriş sayfası ekleme

Tanımlanan ana sayfa ile biz Web sitesi için ASP.NET sayfaları eklemeye hazırsınız. Ekleyerek başlayalım `Default.aspx`, bizim Web sitesinin giriş sayfası. Çözüm Gezgini'nde proje adına sağ tıklayın ve yeni öğe Ekle'yi seçin. Dosyanın adı ve şablon listesi Web formu seçeneğinden çekme `Default.aspx`. Ayrıca, "Select ana sayfa" onay kutusunu işaretleyin.


[![Select ana sayfa onay kutusu denetimi yeni bir Web formu, ekleyin](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Şekil 5**: Select ana sayfa onay kutusu denetimi yeni bir Web formu, ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-site-navigation-cs/_static/image13.png))


Tamam düğmesine tıkladıktan sonra Biz bu yeni ASP.NET sayfa kullanması gereken hangi ana sayfa seçin istenir. Birden çok ana sayfa projenizde sahip olsa da, biz yalnızca bir sahip.


[![Bu ASP.NET sayfası kullanması gereken ana sayfa seçin](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Şekil 6**: Bu ASP.NET sayfası gereken kullanım ana sayfa seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-site-navigation-cs/_static/image16.png))


Ana sayfaya seçtikten sonra yeni ASP.NET sayfaları aşağıdaki biçimlendirme içerir:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

İçinde `@Page` yönergesi var. kullanılan ana sayfa dosyası başvurusudur (`MasterPageFile="~/Site.master"`), ve denetimin ana sayfayla tanımlanan ContentPlaceHolder denetimlerinin her biri için bir içerik denetimi ASP.NET sayfa biçimlendirmesini içeren `ContentPlaceHolderID` İçerik eşleştirme denetlemek için belirli bir ContentPlaceHolder. İçerik işaretleme nereye denetimidir karşılık gelen ContentPlaceHolder'görünmesini istediğiniz. Ayarlama `@Page` yönergesi'nın `Title` özniteliği için giriş ve bazı Karşılama içeriği içerik denetimine ekleyin:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

`Title` Özniteliğini `@Page` yönergesi verir bize sayfanın başlığı ASP.NET sayfasından rağmen ayarlamak `<title>` öğesi ana sayfasında tanımlanır. Biz de başlığı programlı olarak kullanarak ayarlayabilirsiniz `Page.Title`. Ayrıca ana sayfanın başvurular stil sayfaları (gibi `Style.css`) ASP.NET sayfası içindeki ana sayfa göre hangi dizin bağımsız olarak tüm ASP.NET sayfasındaki çalıştıkları şekilde otomatik olarak güncelleştirilir.

Bir tarayıcıda sayfamızı nasıl görüneceğine görebiliriz Tasarım görünümüne geçiş. Tasarım yalnızca içerik düzenlenebilir bölgeleri düzenlenebilir ASP.NET sayfası için ana sayfa içinde tanımlanan ContentPlaceHolder olmayan biçimlendirme görüntülemek unutmayın gri gösterilir.


[![ASP.NET sayfası için Tasarım görünümü düzenlenebilir ve düzenlenemeyen bölgeleri gösterir](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Şekil 7**: ASP.NET sayfası gösterilir hem düzenlenebilir için Tasarım görünümü ve olmayan düzenlenebilir bölgeler ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-site-navigation-cs/_static/image19.png))


Zaman `Default.aspx` bir tarayıcı tarafından sayfayı ziyaret ettiğinde, ASP.NET altyapısı sayfanın ana sayfa içeriğinin ve ASP otomatik olarak birleştirir. NET içerik ve istekte bulunan tarayıcıya gönderilen son HTML'e birleştirilmiş içeriği işler. Ana sayfanın içeriğinin güncelleştirildiğinde, bu ana sayfa kullanan tüm ASP.NET sayfaları talep edilen sonraki açışınızda yeni ana sayfa ile içerik remerged içeriğine sahip olur. Kısacası, ana sayfa model için tek bir sayfayla sağlar olması için Düzen şablonunu tanımlı (ana sayfa), değişiklikler tüm site genelinde hemen yansıtılır.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Web sitesine ek ASP.NET sayfaları ekleme

Şimdi ek ASP.NET sayfası saplamalar çeşitli raporlama gösterileri sonunda tutacak siteye eklemek için bir dakikanızı ayırın. Daha fazla da olacaktır 35 gösterileri toplam, böylece tüm saplama sayfaları şimdi oluşturmak yerine yalnızca daha ilk birkaç oluşturun. Daha iyi yönetebilmek için de olacağından gösterileri birçok kategorileri, gösterileri kategorileri için bir klasör ekleyin. Aşağıdaki üç klasörler için şimdi ekleyin:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Son olarak, yeni dosyalar, Çözüm Gezgini'nde Şekil 8'de gösterildiği gibi ekleyin. Her dosya eklerken "Select ana sayfa" onay unutmayın.


![Aşağıdaki dosyaları ekleyin](master-pages-and-site-navigation-cs/_static/image20.png)

**Şekil 8**: aşağıdaki dosyaları ekleyin


## <a name="step-2-creating-a-site-map"></a>2. adım: Site Haritası oluşturma

Birden çok sayıda sayfaların oluşan bir Web sitesi yönetme zorluklardan biri, site içinde gezinmek ziyaretçiler için basit bir yol sağlar. Sitenin gezinti yapısı başından itibaren tanımlanmış olması gerekir. Ardından, bu yapıyı menüleri veya içerik haritalarında gibi gezinebilir kullanıcı arabirimi öğelerine çevrilmesi gerekir. Son olarak, tüm bu işlem korunur ve yeni sayfa site ve kaldırılmış var olanları eklendikçe güncelleştirilmiş gerekir. ASP.NET 2.0 önce geliştiriciler için kendi sitenin gezinti yapısı, oluşturma, koruma ve gezinebilir kullanıcı arabirimi öğeleri çevirme yoktu. ASP.NET 2. 0'da, ancak, geliştiricilerin çok esnek site gezinti sisteminde yerleşik kullanabilirler.

ASP.NET 2.0 site gezinti sistem site haritası tanımlayın ve ardından bu bilgileri programlı bir API aracılığıyla erişmek için bir geliştirici için bir yol sağlar. ASP.NET, belirli bir şekilde biçimlendirilmiş bir XML dosyasında depolanması için site haritası verileri bekliyor site haritası sağlayıcısı ile birlikte gelir. Ancak, site gezinti sistemi kurulu olduğundan [sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) site haritası bilgileri serileştirmek için alternatif yolu desteklemek için genişletilebilir. Jeff Prosise'nın makale [SQL Site haritası sağlayıcısı, 've edilmiş bekleyen için](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) gösterir nasıl site haritası sağlayıcısı oluşturmak için site haritası bir SQL Server veritabanında depolar; oluşturmak için başka bir seçenektir [site haritası sağlayıcısı temel alarak dosya sistemi yapısı](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

Bu öğreticide, ancak birlikte gelen varsayılan site haritası sağlayıcısı ASP.NET 2.0 ile kullanalım. Site haritasını oluşturmak için yalnızca Çözüm Gezgini'nde proje adına sağ tıklayın, yeni öğe Ekle'ı seçin ve Site Haritası seçeneği belirleyin. Adı olarak bırakın `Web.sitemap` ve Ekle düğmesine tıklayın.


[![Bir Site Haritası projenize ekleyin](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Şekil 9**: Site Haritası bilgisayarınızı projeye ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-site-navigation-cs/_static/image23.png))


Site haritası dosyasını bir XML dosyasıdır. Visual Studio için site haritası yapısı IntelliSense sağladığını unutmayın. Site haritası olmalıdır `<siteMap>` düğümü tam olarak bir içermelidir, kök düğümü olarak `<siteMapNode>` alt öğesi. Bu ilk `<siteMapNode>` öğesi alt öğe rastgele sayıda sonra içerebilir `<siteMapNode>` öğeleri.

Dosya sistemi yapısı taklit etmek üzere site haritası tanımlayın. Diğer bir deyişle, ekleme bir `<siteMapNode>` her üç klasör ve alt öğesi `<siteMapNode>` her bu klasörleri ASP.NET sayfaları için öğeleri sözlüğüdür:

Birtakım


[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

Site haritasını site çeşitli bölümlerini açıklayan bir hiyerarşi Web sitesinin gezinme yapısını tanımlar. Her `<siteMapNode>` öğesinde `Web.sitemap` sitenin gezinti yapısında bir bölümünü temsil eder.


[![Site Haritasını gezinme hiyerarşik bir yapıyı temsil eder](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Şekil 10**: Site Haritası gezinme hiyerarşik bir yapıyı temsil eder ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-site-navigation-cs/_static/image26.png))


ASP.NET .NET Framework'ün aracılığıyla site haritanın yapısı sunan [site haritası sınıfı](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx). Bu sınıf bir `CurrentNode` kullanıcı şu anda ziyaret; bölümü hakkında bilgi verir özelliği `RootNode` özelliği site haritası kökünü döndürür (ev bizim site haritası içinde). Hem `CurrentNode` ve `RootNode` özellikleri return [SiteMapNode](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.aspx) özelliklerine sahip örnekleri gibi `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`ve benzeri için site haritası izin ver gitti hiyerarşisi.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>3. adım: Site Haritasını kullanan bir menü görüntüleme

ASP.NET 2.0 verilere olabilir program aracılığıyla gerçekleştirilir, ASP.NET ister 1.x, bildirimli olarak, aracılığıyla yeni veya [veri kaynağı denetimleri](https://msdn.microsoft.com/en-us/library/ms227679.aspx). İlişkisel veritabanı verileri, veri sınıfları ve diğerleri erişim için ObjectDataSource Denetimi erişmek için SqlDataSource denetimi gibi birkaç yerleşik veri kaynağı denetimleri vardır. Hatta kendi oluşturabileceğiniz [özel veri kaynağı denetimleri](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/en-us/dnvs05/html/DataSourceCon1.asp).

Veri kaynağı denetimleri, ASP.NET sayfası ve temel alınan veri arasındaki bir proxy olarak hizmet. Veri kaynağı denetiminin alınan verileri görüntülemek için biz genellikle başka bir Web denetimi sayfasına ekleyin ve veri kaynağı denetimi bağlamak. Bir veri kaynağı denetimi ile bir Web denetimi bağlamak için yalnızca Web denetimin ayarlamak `DataSourceID` özelliği veri kaynağı denetiminin değerine `ID` özelliği.

Site haritanın verilerle çalışma yardımcı olmak için ASP.NET Web denetimini bizim Web sitesinin site haritası karşı bağlama kurmamızı sağlayan SiteMapDataSource denetimini içerir. İki Web denetimleri TreeView ve menü yaygın olarak bir gezinti kullanıcı arabirimi için kullanılır. Site haritası verileri bu iki denetimleri birine bağlamak için yalnızca SiteMapDataSource TreeView birlikte sayfasına ekleyin veya menü denetim `DataSourceID` özelliğini uygun şekilde ayarlayın. Örneğin, biz aşağıdaki biçimlendirme kullanarak ana sayfaya bir menü denetimi ekleyebilirsiniz:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

İçin daha hassas bir denetime sahip verilmiş HTML, biz SiteMapDataSource denetimi yineleyici denetimine bağlayabilirsiniz sözlüğüdür:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

SiteMapDataSource denetimini site haritası bir hiyerarşi düzeyi kök site eşleme düğümü ile başlayan bir kerede döndürür (ev bizim site haritası içinde), ardından İleri düzey (temel raporlama, raporları filtreleme ve özelleştirilmiş biçimlendirme) ve benzeri. Bir yineleyici SiteMapDataSource bağlama sırasında ilk düzeyi numaralandırır döndürülür ve başlatır `ItemTemplate` her `SiteMapNode` bu ilk düzeyi örneği. Belirli bir özelliğe erişmek için `SiteMapNode`, biz kullanabilirsiniz `Eval(propertyName)`, biz her nereden olduğu `SiteMapNode`'s `Url` ve `Title` köprü denetimi için özellikler.

Yukarıdaki yineleyici örnek aşağıdaki biçimlendirme oluşturulur:


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

Bu site haritası düğümler (temel raporlama, filtreleme raporlar ve özelleştirilmiş biçimlendirme) oluşturan *ikinci* işlenirken, ilk site haritası düzeyini. Bunun nedeni, SiteMapDataSource's `ShowStartingNode` kök site haritası düğümü atlama ve bunun yerine site haritası hiyerarşisinde ikinci düzey döndürerek başlamak SiteMapDataSource neden özelliği False olarak ayarlanır.

Temel raporlama, filtreleme raporlar ve özelleştirilmiş biçimlendirme için alt öğelerini görüntülemek için `SiteMapNode` s, ilk yineleyici için kullanıcının başka bir yineleyici ekleyebiliriz `ItemTemplate`. Bu ikinci yineleyici bağlanacağı `SiteMapNode` örneğinin `ChildNodes` özelliği, şu şekilde:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Bu iki yineleyiciler (bazı biçimlendirme okumanızdır kaldırılmıştır) aşağıdaki biçimlendirmede neden:


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

Seçilen stiller CSS kullanma gelen [Rachel Barış](http://www.rachelandrew.co.uk/)kullanıcının kitap [CSS Anthology: 101 temel ipuçlarını, püf noktaları, &amp; çalma saldırma](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), `<ul>` ve `<li>` öğeleri stilde şekilde Biçimlendirme aşağıdaki visual çıkışı üretir:


![İki yineleyiciler ve bazı CSS oluşan bir menüsü](master-pages-and-site-navigation-cs/_static/image27.png)

**Şekil 11**: menü oluşan iki yineleyiciler ve bazı CSS


Bu menü ana sayfa ve içinde tanımlanan site haritası bağlı `Web.sitemap`seçeneğidir; yani site haritası herhangi bir değişiklik hemen tüm yansıtılır olduğunu kullanan sayfaları `Site.master` ana sayfa.

## <a name="disabling-viewstate"></a>ViewState devre dışı bırakma

Tüm ASP.NET denetimleri isteğe bağlı olarak durumlarına kalıcı [görüntülemek durumu](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), gizli bir form alanı işlenen HTML olarak serileştirilmiş. Veri bağlama için bir veri Web denetimi gibi görünüm durumu program aracılığıyla değiştirilmiş durumlarına geri göndermeler arasında unutmayın denetimleri tarafından kullanılır. Görünüm durumu üzerinde Geri göndermeler anımsanacağını bilgilere sağlarken, istemciye gönderilen ve ciddi sayfa oluşan şişirmeyi için yol açabilecek değilse yakından izlenen biçimlendirmeyi boyutu artar. Veri Web denetimleri özellikle GridView bir sayfaya ek kilobayt biçimlendirme düzinelerce eklemek için özellikle notorious. Bu tür bir artış geniş bant veya intranet kullanıcı için düşünülerek olsa da, Görünüm durumu gidiş dönüş çevirmeli kullanıcılar için birkaç saniye ekleyebilirsiniz.

Durumunu görüntülemek, bir tarayıcıda sayfasını ziyaret edin ve ardından web sayfası tarafından gönderilen kaynak görüntüleyin etkisini görmek için (Internet Explorer'da, Görünüm menüsüne gidin ve kaynak seçeneğini belirleyin). Ayrıca açabilirsiniz [Sayfa izlemeyi](https://msdn.microsoft.com/en-us/library/sfbfw58f.aspx) her sayfadaki denetimleri tarafından kullanılan görünüm durumu ayırma görmek için. Görünüm durumu bilgisini adlı gizli bir form alanı içinde serileştirilmiş `__VIEWSTATE`, bulunan bir `<div>` öğesi açtıktan hemen sonra `<form>` etiketi. Görünüm durumu, yalnızca kullanılan bir Web formu olduğunda kalıcıdır; ASP.NET sayfası içermiyorsa bir `<form runat="server">` olmayacaktır Bu, bildirim temelli sözdiziminde bir `__VIEWSTATE` oluşturulan biçimlendirmenin gizli form alanı.

`__VIEWSTATE` Ana sayfa tarafından oluşturulan form alanı kabaca 1800 baytı sayfanın oluşturulan biçimlendirme ekler. Bu ek oluşan şişirmeyi öncelikle yineleyici denetimine son SiteMapDataSource denetimini içeriğini durumunu görüntülemek için kalıcı olarak. Bir ek 1800 baytı GridView birçok alanları ve kayıtları kullanırken hakkında Çoğalması almak için çok gibi görünmeyebilir olsa da, Görünüm durumu kolayca 10 veya daha fazla faktörüyle swell.

Görünüm durumu devre dışı bırakılabilir sayfasının veya denetiminin düzeyinde ayarlayarak `EnableViewState` özelliğine `false`, böylece oluşturulan biçimlendirmenin boyutunu azaltma. Bir veri için görünüm durumuna devre dışı bırakma Web olduğunda denetim her geri göndermede denetim Geri göndermeler arasında Web denetimi verileri bağlı veri devam ederse Web veri için görünüm durumuna itibaren verileri bağlı olmalıdır. ASP.NET sürümünde bu sorumluluğu altına düştü sayfasında Geliştirici; fark etmeyecektir üzerinde 1.x ASP.NET 2. 0'da, ancak veri Web denetimleri her geri gönderme kendi veri kaynak denetimi için gerekirse rebind.

Sayfanın görünüm durumu şimdi azaltmak için yineleyici denetimin ayarlamak `EnableViewState` özelliğine `false`. Bu özellikler penceresini Tasarımcısı'nda veya bildirimli olarak kaynak görünümünde aracılığıyla yapılabilir. Bu değişikliği yaptıktan sonra yineleyici'nın bildirim temelli biçimlendirme gibi görünmelidir:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Bu değişiklik, sayfa ait görünüm durumu boyutu yalnızca bir küçültülebilir işlenen sonra 52 bayt cinsinden % 97 tasarruf boyutu görünümünde durum! Bu seri boyunca öğreticileri biz verilerin Web denetimleri görünüm durumu varsayılan olarak oluşturulan biçimlendirmenin boyutunu azaltmak için devre dışı bırak. Örnekler çoğu `EnableViewState` özellik ayarlanacak `false` ve bunu Bahsetme yapılır. Durumu incelenecektir burada veriler için sırayla etkinleştirilmeli senaryolarda görünümdür yalnızca bir kez Web denetimi beklenen işlevselliği sağlar.

## <a name="step-4-adding-breadcrumb-navigation"></a>4. adım: İçerik haritası Gezinti ekleme

Ana sayfaya tamamlamak için bir içerik haritası Gezinti kullanıcı Arabirimi öğesi her sayfaya ekleyelim. İçerik haritası, kullanıcıların geçerli konumlarına site hiyerarşisi içinde hızlı bir şekilde gösterir. ASP.NET 2.0 içerik haritası kolay yalnızca ekleme bir ASP sayfasına; kod gereklidir.

Sitemizi için bu denetim üstbilgisine ekleyin `<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

Kullanıcı Bu süreç boyunca tüm kadar kök ziyaret o site haritası düğümün "üst öğelerinden," yanı sıra site haritası hiyerarşisi içinde geçerli sayfa içerik haritası gösterir (ev bizim site haritası içinde).


![Geçerli sayfa içerik haritası görüntüler ve alt öğelerinden sitedeki hiyerarşi eşleyin](master-pages-and-site-navigation-cs/_static/image28.png)

**Şekil 12**: içerik haritası geçerli sayfasını görüntüler ve alt öğelerinden sitedeki hiyerarşi eşleyin


## <a name="step-5-adding-the-default-page-for-each-section"></a>5. adım: her bölüm için varsayılan sayfası ekleme

Sitemizi eğitimlerine kategorilere farklı temel raporlama, filtreleme, özel biçimlendirme, vb. her kategori ve o klasördeki ASP.NET sayfaları olarak karşılık gelen öğreticileri için bir klasör ile ayrılmıştır. Ayrıca, her bir klasör içeren bir `Default.aspx` sayfası. Bu varsayılan sayfa için şimdi öğreticileri geçerli bölüm için görüntüler. Diğer bir deyişle, için `Default.aspx` içinde `BasicReporting` klasörü sahibiz bağlantılar `SimpleDisplay.aspx`, `DeclarativeParams.aspx`, ve `ProgrammaticParams.aspx`. Burada, tekrar biz kullanabilirsiniz `SiteMap` sınıf ve veri site haritası dayanarak bu bilgiyi görüntülemek için Web denetimi tanımlanan `Web.sitemap`.

Şimdi yeniden ancak biz başlık ve öğreticiler açıklaması görüntülersiniz bu kez bir yineleyici kullanarak sırasız bir listesini görüntüler. Biçimlendirme ve kodun bu işlem gerçekleştirmek için her biri için yinelenen gerektiği `Default.aspx` sayfasında, biz kapsülleyen bu UI mantığı bir [kullanıcı denetimi](https://msdn.microsoft.com/en-us/library/y6wb1a0e.aspx). Adlı Web sitesi bir klasör oluşturun `UserControls` ve için yeni bir öğe türü adında bir Web kullanıcı denetimi Ekle `SectionLevelTutorialListing.ascx`ve aşağıdaki biçimlendirmeyi ekleyin:


[![UserControls kendi klasörüne yeni bir Web kullanıcı denetimi Ekle](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Şekil 13**: için yeni bir Web kullanıcı denetimi Ekle `UserControls` klasörü ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-site-navigation-cs/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs


[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

Önceki yineleyici örnekte biz bağlı `SiteMap` yineleyici veri bildirimli olarak; `SectionLevelTutorialListing` kullanıcı denetimi, ancak mu Bunu programlı olarak. İçinde `Page_Load` olay işleyicisi, bir onay yapılan bu sayfayı URL eşleyen bir düğüme site haritası s emin olmak için. Bu kullanıcı denetimi karşılık gelen sahip olmayan bir sayfasında kullandıysanız `<siteMapNode>` girişi `SiteMap.CurrentNode` döndürülecek `null` ve hiçbir veri yineleyici bağlanacak. Biz varsayarak bir `CurrentNode`, biz bağlamak kendi `ChildNodes` yineleyici koleksiyonuna. Bizim site haritası ayarlandığından şekilde `Default.aspx` sayfa her bölümde öğreticileri konusu bölüm içindeki tüm üst düğümü, bu kod bağlantılar ve tüm bölümün öğreticileri açıklamalarını ekran görüntüsü aşağıda gösterildiği gibi görüntülenir.

Bu yineleyici oluşturulduktan sonra açmak `Default.aspx` klasörlerinin her biri sayfalarında Tasarım görünümüne gidin ve sadece kullanıcı denetimi tasarım yüzeyine Çözüm Gezgini'nden öğretici listenin görünmesini istediğiniz yere sürükleyin.


[![Kullanıcı denetimi eklenmiş Default.aspx sahip](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Şekil 14**: kullanıcı Denetim sahip eklenmiş `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-site-navigation-cs/_static/image34.png))


[![Temel raporlama öğreticileri listelenir](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Şekil 15**: temel raporlama öğreticileri listelenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-pages-and-site-navigation-cs/_static/image37.png))


## <a name="summary"></a>Özet

Şimdi, tanımlı site haritası ve tam ana sayfa ile tutarlı sayfa düzeni ve gezinti düzeni için ilgili verileri öğreticilerimizi sahip. Bizim siteye eklediğimiz kaç sayfaları bağımsız olarak, site genelinde sayfa düzeni veya site gezinti bilgileri güncelleştirmek merkezi bu bilgileri nedeniyle hızlı ve basit bir süreç kullanılır. Özellikle, sayfa düzeni bilgileri ana sayfada tanımlı `Site.master` ve içinde eşleme site `Web.sitemap`. Yazılacak ihtiyacımız kaydetmedi *herhangi* bu site genelinde sayfa düzeni ve gezinti mekanizması elde etmek için kod ve biz Visual Studio'da tam WYSIWYG tasarımcı desteği korur.

İş mantığı katmanı ve veri erişim katmanı tamamlandı ve tanımlı bir tutarlı sayfa düzeni ve site gezintisi sahip, biz ortak raporlama desenler araştırmaya başlamak hazırsınız. İçinde [sonraki](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) üç öğreticileri biz bakabilir GridView, DetailsView, içinde BLL kaynağından alınan verileri görüntüleme temel raporlama görevleri ve FormView denetler.

Mutluluk programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET ana sayfalar genel bakış](https://msdn.microsoft.com/en-us/library/wtxbf3hh.aspx)
- [ASP.NET 2.0 ana sayfalar](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2.0 tasarım şablonları](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [ASP.NET sitesi gezinti genel bakış](https://msdn.microsoft.com/en-us/library/e468hxky.aspx)
- [ASP.NET 2.0 inceleniyor kullanıcının Site gezintisi](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [ASP.NET 2.0 Site Gezinti özellikleri](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [ASP.NET görünüm durumunu anlama](https://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnaspp/html/viewstate.asp)
- [Nasıl yapılır: bir ASP.NET sayfası için izlemeyi etkinleştirme](https://msdn.microsoft.com/en-us/library/94c55d08%28VS.80%29.aspx)
- [ASP.NET kullanıcı denetimleri](https://msdn.microsoft.com/en-us/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Liz Shulok, Dennis Patterson ve Hilton Giesenow yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](creating-a-business-logic-layer-cs.md)
[sonraki](creating-a-data-access-layer-vb.md)
