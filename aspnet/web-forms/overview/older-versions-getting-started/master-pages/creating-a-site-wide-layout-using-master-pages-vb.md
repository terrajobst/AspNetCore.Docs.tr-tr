---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: Ana sayfalar (VB) kullanarak Site geneli bir düzen oluşturma | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, ana sayfa temel bilgileri gösterir. Yani, ana sayfalar nelerdir nasıl yaptığını bir ana sayfa oluşturma, içerik yer tutucuları nelerdir nasıl yaptığını bir cr...
ms.author: aspnetcontent
ms.date: 05/21/2008
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 182f45c28dc37633b429fead333d401818299e36
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827828"
---
<a name="creating-a-site-wide-layout-using-master-pages-vb"></a>Ana sayfalar (VB) kullanarak Site geneli bir düzen oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> Bu öğreticide, ana sayfa temel bilgileri gösterir. Yani, ana sayfalar nedir, nasıl oluşturmak bir ana sayfa İçerik yer tutucu nedir, nasıl oluşturmak nasıl değiştirme ana sayfaya otomatik olarak kendi ilişkili içerik sayfalarını ve benzeri yansıtılan bir ana sayfa kullanan ASP.NET sayfası.


## <a name="introduction"></a>Giriş

İyi tasarlanmış bir Web sitesinin bir özelliği, bir site genelinde tutarlı sayfa düzeni değil. Örneğin www.asp.net Web sitesi uygulayın. Bu makalenin yazıldığı sırada, her sayfanın sayfasının altındaki ve üstündeki aynı içeriğe sahip. Şekil 1 gösterildiği gibi her sayfanın üstündeki bir gri çubukta Microsoft Communities listesini görüntüler. Diğer bir deyişle site logosu, içine site çevrilen dilleri ve çekirdek bölüm listesi altında: Giriş, Başlarken, öğrenin, indirmeler ve benzeri. Benzer şekilde, sayfanın www.asp.net, telif hakkı bildirimini ve gizlilik bildiriminin bağlantısını reklam hakkında bilgi içerir.


[![Www.asp.net Web sitesi tüm sayfalar arasında tutarlı bir görünüm kullanır.](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

<strong>Şekil 01</strong>: Web sitesi www.asp.net tutarlı ve tüm sayfaları arasında düşünüyorsanız kullanır ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))


İyi tasarlanmış bir sitenin başka bir öznitelik ile sitenin görünüm değiştirilebilir kolaylığıdır. Şekil 1 Mart 2008'den itibaren www.asp.net giriş sayfasını gösterir, ancak şimdi ve bu öğreticinin yayın arasında görünümünü değişmiş olabilir. Belki de MVC çerçevesi için yeni bir bölüm eklemek için üst menü öğelerini genişletir. Veya belki de önemli ölçüde yeni bir tasarım farklı renkleri, yazı tipleri ve düzeni ile unveiled olacaktır. Tüm site için bu tür değişiklikler uygulanırken, web sitesini oluşturan sayfalar binlerce değişiklik gerektirmeyen hızlı ve basit bir süreç olması gerekir.

ASP.NET'te site genelinde sayfası şablonu oluşturma kullanımının olası *ana sayfalar*. Ana sayfa tüm arasında ortak biçimlendirme tanımlayan ASP.NET sayfası özel bir tür içinde koysalar olan *içerik sayfalarının* temelinde içerik sayfası içerikli sayfa özelleştirilebilir olduğu bölgelerin yanı sıra. (Bir içerik ana sayfaya bağlı bir ASP.NET sayfasına sayfasıdır.) Bir ana sayfanın düzeni veya biçimlendirme değiştirildiğinde, tüm içerik sayfalarını çıkış benzer şekilde hemen güncelleştirilir, güncelleştirme ve dağıtma (yani, ana sayfası) tek bir dosya olarak kolayca site genelinde görünüm değişiklikler uygulanırken hale getirir.

Ana sayfaları kullanarak keşfetme öğreticileri serisinin ilk öğreticide budur. Bu öğretici serisinin boyunca ediyoruz:

- Ana sayfalar ve bunların ilişkili içerik sayfalarını inceleyin,
- İpuçları, püf noktalarını ve tuzakları çeşitli tartışın,
- Ortak ana sayfa Tuzaklar tanımlamak ve geçici çözümleri keşfedin
- Nasıl bir içerik sayfasının ve tersi bir ana sayfaya erişmek, bkz:
- Çalışma zamanında, bir içerik sayfasının ana sayfasını belirtmek öğrenin ve
- Diğer Gelişmiş Ana sayfa konuları.

Bu öğretici, kısa ve görsel olarak işleminde size kılavuzluk için yeterince ekran görüntüleri ile adım adım yönergeler sağlamak için sağlamıştır. Her öğretici, C# ve Visual Basic sürümlerinde kullanılabilir ve kullanılan tüm kod indirilmesini içerir.

Ana sayfa temel göz bülteninin açılış sayısına Öğreticisine başlar. Biz nasıl iş ana sayfa tartışın, bir ana sayfa ve ilişkili içerik sayfalarını Visual Web Developer kullanarak oluşturma konusunda arayın ve nasıl değişiklikler ana sayfa için içerik sayfalarını hemen yansıtılır bakın. Haydi başlayalım!

## <a name="understanding-how-master-pages-work"></a>Nasıl iş ana sayfa anlama

Bir site genelinde tutarlı sayfa düzeni ile bir Web sitesi oluşturmanın, her web sayfasını özel içeriği yanı sıra yaygın biçimlendirme biçimlendirme yayma gerektirir. Örneğin, her www.asp.net öğretici veya forum gönderisini kendi benzersiz içerik varken bu sayfaların her biri de işleme ortak bir dizi `<div>` en üst düzey bölüm bağlantıları görüntüleme öğeleri: Giriş, Başlarken, öğrenin ve benzeri.

Web sayfaları ile tutarlı bir görünümü ve deneyimini oluşturmaya yönelik teknikleri çeşitli vardır. Yalnızca kopyalayıp ortak yerleşim biçimlendirme tüm web sayfalarına naïve yaklaşım, ancak bu yaklaşım olumsuzlukları bir sayı. Yeni başlayanlar için her yeni sayfa oluşturulduğunda paylaşılan içerik sayfaya kopyalayıp unutmamanız gerekir. Yeni bir sayfaya yanlışlıkla paylaşılan biçimlendirme yalnızca bir alt kopyalayabilir gibi böyle kopyalama ve yapıştırma işlemleri için hata ekleyerek. Ve üst için varolan bir site genelinde görünümü gerçek sorunlu sitesindeki her sayfada yeni Azure görünümü ve deneyimini kullanmak için düzenlenmesi gerekir çünkü yeni bir tane ile değiştirerek bu yaklaşım sağlar.

ASP.NET 2.0 sürümünde önce sayfasında geliştiriciler genellikle ortak işaretlemede yerleştirilen [kullanıcı denetimleri](https://msdn.microsoft.com/library/y6wb1a0e.aspx) ve ardından bu kullanıcı denetimleri her sayfasına eklenir. Bu yaklaşım, sayfa geliştirici kullanıcı denetimleri her yeni sayfa için el ile eklemeyi unutmayın, ancak daha kolay site genelinde yapılan değişikliklerin yalnızca kullanıcı denetimleri ortak biçimlendirme güncelleştirirken değiştirilmesi gereken olduğundan izin gereklidir. Ne yazık ki, ASP.NET 1.x uygulamalar - kullanıcı denetimleri Tasarım görünümünde gri kutular olarak çizilir. oluşturmak için Visual Studio .NET 2002 ve 2003 - Visual Studio sürümlerinde kullanılabilir. Sonuç olarak, sayfa geliştiricileri bu yaklaşımı kullanarak bir WYSIWYG tasarım zamanı ortamında keyfini değil.

Giriş ile ASP.NET sürüm 2.0 ve Visual Studio 2005'te kullanıcı denetimleri kullanmanın yararlarını ele alınan *ana sayfalar*. Ana sayfa site genelinde biçimlendirme tanımlayan ASP.NET sayfası özel türüdür ve *bölgeleri* ilişkili burada *içerik sayfalarının* kendi özel biçimlendirme tanımlayın. Adım 1'de göreceğiz, bu bölgelerden ContentPlaceHolder denetimleri tarafından tanımlanır. ContentPlaceHolder denetimini yalnızca bir içerik sayfası tarafından özel içerik nerede yerleştirilebilir bir konumda ana sayfanın denetim hiyerarşisi gösterir.

> [!NOTE]
> Ana sayfalar işlevselliği ve temel kavramlar, ASP.NET 2.0 sürümünde bu yana değişmemiştir. Ancak, Visual Studio 2008 iç içe geçmiş ana sayfalar için tasarım zamanı desteği Visual Studio 2005'te eksik bir özellik sunar. Biz iç içe geçmiş ana sayfalar bir sonraki öğreticide kullanacaksınız.


Şekil 2 www.asp.net ana sayfaya aşağıdaki gibi görünmelidir gösterir. Ana sayfanın sol orta-tek tek her web sayfasına ilişkin benzersiz içeriğin bulunduğu, bir ContentPlaceHolder yanı sıra ortak site geneli bir düzen - üst, alt ve her sayfanın sağ biçimlendirme - tanımladığını unutmayın.


![Ana sayfa Site geneli bir düzen ve bölgeleri düzenlenebilir bir içerik sayfası içerik sayfası temelinde tanımlar](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**Şekil 02**: ana sayfa Site geneli bir düzen ve bölgeleri düzenlenebilir bir içerik sayfası içerik sayfası temelinde tanımlar


Ana sayfa oluşturulduktan sonra yeni ASP.NET sayfaları değerinde bir onay kutusu aracılığıyla bağlanabilir. Bu ASP.NET sayfaları - içerik sayfalarını çağrıldı - her bir ana sayfanın ContentPlaceHolder denetimler için bir içerik denetimi içerir. İçerik sayfası bir tarayıcıdan ziyaret edildiğinde ASP.NET altyapısı ana sayfanın denetim hiyerarşisi oluşturur ve içerik sayfasının denetim hiyerarşisi uygun yerlerde yerleştirir. Bu birleşik denetim hiyerarşisi işlenir ve sonuçta elde edilen HTML için son kullanıcının tarayıcı döndürülür. Sonuç olarak, içerik sayfası, kendi ana sayfasını ContentPlaceHolder denetimleri dışında tanımlanan ortak biçimlendirme hem kendi içerik denetimleri içinde tanımlanan sayfaya özgü biçimlendirmeyi gösterir. Şekil 3'te bu kavramı gösterir.


[![İstenen sayfa biçimlendirme, ana sayfaya çarpım](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**Şekil 03**: İstenen sayfa biçimlendirme ana sayfaya çarpım ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))


Nasıl iş ana sayfa Bahsettiğimiz, bir ana sayfa ve ilişkili içerik sayfalarını Visual Web Developer kullanarak oluşturmaya bir göz atalım.

> [!NOTE]
> Olabilecek en büyük hedef kitlesine ulaşmak için Bu öğretici serisinin ekleriz ASP.NET Web sitesi ASP.NET 3.5 Microsoft'un ücretsiz Visual Studio 2008 sürümüyle kullanılarak oluşturulacak [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Yok, henüz ASP.NET 3.5 için yükseltilmiş endişelenmeyin - bu öğreticileri işlerinde açıklanan kavramları eşit ile ASP.NET 2.0 ve Visual Studio 2005 yanı sıra. Ancak, bazı demo uygulamaları .NET Framework sürüm 3.5 yeni özellikleri kullanabilir; 3.5 özgü özellikler kullanıldığında, ı anlatılmaktadır sürüm 2.0 benzer işlevselliği uygulamak bir not ekleyin. Her öğretici hedef .NET sonuçlanır Framework sürüm 3.5, demo uygulamaları için kullanılabilir yüklediğiniz aklınızda bulundurun bir `Web.config` 3.5 özgü yapılandırma öğeleri içeren dosya. Henüz .NET 3.5, ardından indirilebilir web uygulamasını bilgisayarınıza yüklemek varsa kısa yazıyı 3.5 özgü biçimlendirmeden kaldırmadan çalışmaz `Web.config`. Bkz: [ayrıntıları ASP.NET sürüm 3.5's `Web.config` dosya](http://www.4guysfromrolla.com/articles/121207-1.aspx) Bu konu hakkında daha fazla bilgi için.


## <a name="step-1-creating-a-master-page"></a>1. adım: bir ana sayfa oluşturma

Biz oluşturma ve içeriği ve ana sayfaları kullanarak keşfedebilirsiniz önce ilk ASP.NET Web sitesi gerekiyor. Yeni bir dosya sistemi tabanlı ASP.NET Web sitesi oluşturmaya başlayın. Bunu yapmak için Visual Web Developer başlatın ve ardından Dosya menüsüne gidin ve yeni Web sitesi, yeni Web sitesi iletişim kutusunda görüntüleme seçin (bkz: Şekil 4) kutusunda. ASP.NET Web sitesi şablonu seçin, dosya sistem konumu aşağı açılan listesi olarak, web sitesine yerleştirmek için bir klasör seçin ve Visual Basic Dil ayarlayın. Bu yeni bir web sitesi oluşturmak bir `Default.aspx` ASP.NET sayfası bir `App_Data` klasöründe ve `Web.config` dosya.

> [!NOTE]
> Visual Studio, proje yönetimi iki modu destekler: Web sitesi projeleri ve Web Uygulama projeleri. Web sitesi projelerini Web Uygulama projeleri, Visual Studio .NET 2002/2003 proje mimarisi taklit - bir proje dosyası dahil etme ve yerleştirilen tek bir derleme içine projenin kaynak kod derlenmeye ise bir proje dosyası eksik `/bin` klasör. Service Pack 1'web uygulaması proje modeli yeniden olsa da visual Studio 2005 başlangıçta yalnızca desteklenen Web sitesi, proje; Visual Studio 2008 her iki proje modelleri sunar. Ancak, Visual Web Developer 2005 ve 2008 sürümleri, yalnızca Web sitesi projelerini destekler. Bu öğretici serisine my tanıtımları ben Web sitesi projesi modelini kullanır. Olmayan Express edition kullanıyorsanız ve kullanmak istediğiniz [Web uygulaması proje modeli](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) bunun yerine, bunu yapabilir; ancak olabileceğini bazı tutarsızlıklar ekranınızın ve karşı uygulayacağınız adımlar gördükleri arasında farkında çekinmeyin gösterilen ekran görüntüleri ve bu öğreticileri, sağlanan yönergeler.


[![Yeni bir dosya sistemi tabanlı Web sitesi oluşturma](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**Şekil 04**: New File System-Based Web sitesi oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))


Ardından, bir ana sayfa sitenin kök dizininde proje adına sağ tıklayın, yeni öğe Ekle seçme ve ana sayfa şablonu seçip ekleyin. Ana sayfalar uzantılıdır Not `.master`. Bu yeni bir ana sayfa adı `Site.master` ve Ekle'ye tıklayın.


[![Ana sayfa ekleyin ve Web sitesi Site.master adlı](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**Şekil 05**: bir ana sayfa adı ekleme `Site.master` Web sitesine ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))


Visual Web Developer ile yeni bir ana sayfa dosyası ekleme bir ana sayfa aşağıdaki bildirim temelli biçimlendirme oluşturur:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

Bildirim temelli biçimlendirmede ilk satırının [ `@Master` yönergesi](https://msdn.microsoft.com/library/ms228176.aspx). `@Master` Yönergesi benzer [ `@Page` yönergesi](https://msdn.microsoft.com/library/ydy4x04a.aspx) ASP.NET sayfaları görünür. Bu, konum ve ana sayfa arka plan kod sınıfı devralma hakkında bilgi ve sunucu tarafı dili (VB) tanımlar.

`DOCTYPE` Ve bildirim temelli işaretleme sayfanın altında görünür `@Master` yönergesi. Dört sunucu tarafı denetimlerdir birlikte statik HTML sayfası içerir:

- **Web formu ( `<form runat="server">`)** - tüm ASP.NET sayfaları, genellikle bir Web formu - sahip ve ana sayfaya Web Form içinde - görünmelidir Web denetimleri içerebilir, çünkü ana sayfanıza (yerine Web formu e ekleyerek Web formu eklediğinizden emin olun ACH içerik sayfası).
- **Adlı bir ContentPlaceHolder denetimi `ContentPlaceHolder1`**  -bu ContentPlaceHolder denetimi, Web formu içinde görünür ve bölge için içerik sayfasının kullanıcı arabirimi olarak görev yapar.
- **Sunucu tarafı `<head>` öğesi** - `<head>` öğesinin `runat="server"` özniteliği, sunucu tarafı kodu üzerinden erişilebilir hale getirme. `<head>` Öğesi, bu şekilde gerçekleştirilir böylece başlığı ve diğer `<head>`-biçimlendirme eklenebilir veya programlama yoluyla ayarlanan ilgili. Örneğin, bir ASP.NET sayfasının ayarlamak `Title` özellik değişiklikleri `<title>` öğe işlenen tarafından `<head>` sunucu denetimi.
- **Adlı bir ContentPlaceHolder denetimi `head`**  -içinde bu ContentPlaceHolder denetimi görünür `<head>` sunucu denetim ve bildirimli olarak içerik eklemek için kullanılabilir `<head>` öğesi.

Bu varsayılan ana sayfayı bildirim temelli biçimlendirme, kendi ana sayfalar tasarlamak için bir başlangıç noktası olarak görev yapar. HTML düzenleme veya ek Web denetimleri veya ContentPlaceHolder ana sayfasına eklemek için çekinmeyin.

> [!NOTE]
> Ana Sayfa tasarlama yaptığınızda emin, ana sayfa bir Web formu içeren ve bu Web formu içinde en az bir ContentPlaceHolder denetleyen görüntülenir.


### <a name="creating-a-simple-site-layout"></a>Basit bir Site bir düzen oluşturma

Şimdi genişletin `Site.master`ait tüm sayfaları paylaşacağı site bir düzen oluşturmak için bildirim temelli işaretleme varsayılan: ortak bir üst bilgi; gezinti, haber ve diğer site genelinde içerik; ve "Desteklenen tarafından Microsoft ASP.NET" simgesi görüntüleyen altbilgi bir sol sütunu. İçerik sayfalarını birini bir tarayıcıdan görüntülendiğinde Şekil 6 ana sayfaya nihai sonucu gösterir. Ziyaret sayfa kırmızı daire içinde Şekil 6 bölgede özgüdür (`Default.aspx`); diğer tüm içerik sayfalarının arasında ana sayfada tanımlıysa ve bu nedenle tutarlı içeriktir.


[![Ana sayfa biçimlendirme, üst, sol ve alt bölümleri tanımlar.](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**Şekil 06**: üst, sol ve altında kısımları için ana sayfa biçimlendirmeyi tanımlar ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image16.png))


Şekil 6'daki site düzenini elde etmek için başlangıç güncelleştirerek `Site.master` aşağıdaki bildirim temelli biçimlendirme içeren ana sayfa:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

Ana sayfanın düzeni, bir dizi kullanarak tanımlanır `<div>` HTML öğeleri. `topContent` `<div>` Her sayfanın üst kısmında görünür biçimlendirme içeriyor ancak `mainContent`, `leftContent`, ve `footerContent` `<div>` s sayfanın içeriğinin, sol sütunda ve "desteklenen tarafından Microsoft görüntülemek için kullanılır ASP.NET"simgesi, sırasıyla. Bu ekleme yanı sıra `<div>` öğeleri, ben ayrıca yeniden adlandırıldı `ID` birincil ContentPlaceHolder denetiminden özelliği `ContentPlaceHolder1` için `MainContent`.

Bu çeşitli biçimlendirme ve yerleştirme kuralları `<div>` öğeleri il içinde [geçişli stil sayfası (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) dosya `Styles.css`, aracılığıyla belirtilen bir `<link>` ana sayfanın öğesinde`<head>`öğesi. Görünümü her çeşitli kurallar tanımlamak `<div>` öğesi belirtildiği üstünde. Örneğin, `topContent` `<div>` "Ana sayfa öğreticiler" metin ve bağlantıyı gösteren öğesi var belirtilen biçimlendirme kurallarını `Styles.css` gibi:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

Bilgisayarınızda aşağıdaki, bu öğreticinin eşlik eden kod karşıdan yüklemek ve eklemek ihtiyacınız olacak `Styles.css` projenize bir dosya. Benzer şekilde, ayrıca adlı bir klasör oluşturmak için ihtiyacınız `Images` ve "Desteklenen tarafından Microsoft ASP.NET" simgesi indirilen demo Web sitesinden projenize kopyalayın.

> [!NOTE]
> Bu makalenin kapsamı dışındadır CSS ve web sayfası biçimlendirme bir tartışma olduğu. CSS hakkında daha fazla bilgi için kullanıma [CSS öğreticiler](http://www.w3schools.com/css/default.asp) adresindeki [W3Schools.com](http://www.w3schools.com/). Bu öğreticinin eşlik eden kodu indirin ve CSS ayarlar yürütmek için de öneriyoruz `Styles.css` farklı biçimlendirme kuralları etkilerini görmek için.


### <a name="creating-a-master-page-using-an-existing-design-template"></a>Mevcut bir tasarım şablonu kullanarak bir ana sayfa oluşturma

Yıllar içinde miyim birkaç küçük için orta ölçekli şirketler için ASP.NET web uygulaması oluşturdunuz. My olan istemcilerin bazılarını kullanmak istedikleri var olan bir site düzenini vardı; Başkalarının yetkin Grafik Tasarımcısı işe. Birkaç Web sitesini düzenini tasarlamak için bana tasarlamaları. Şekil 6 ile söyleyebilirsiniz gibi bir Web sitesinin düzenini tasarlamak için programcı görev, doktor, vergileri yaslanın open-heart surgery gerçekleştirmek, uzak sahip olarak genellikle olarak akıllıca olur.

Neyse ki, ücretsiz HTML tasarım şablonları sunmaya innumerous Web siteleri vardır - Google ilişkin arama terimi "ücretsiz Web sitesi şablonları" 6 milyondan fazla sonuç döndürdü. Sık kullanılan my olanları biri [OpenDesigns.org](http://opendesigns.org/). İstediğiniz bir Web sitesi şablonu bulduğunuzda, CSS dosyaları ve görüntüleri Web sitesi projenize ekleyin ve şablona ait HTML ana sayfanıza tümleştirin.

> [!NOTE]
> Microsoft ayrıca, bir dizi sunar [ASP.NET tasarım başlangıç Seti şablonları serbest](https://msdn.microsoft.com/asp.net/aa336613.aspx) Visual Studio'da yeni bir Web sitesi iletişim kutusuna tümleştirin.


## <a name="step-2-creating-associated-content-pages"></a>2. adım: İlişkili içerik sayfaları oluşturma

Oluşturulan ana sayfa ile ana sayfaya bağlı olan ASP.NET sayfaları oluşturmaya başlamak hazırız. Bu sayfaları denir *içerik sayfalarının*.

Şimdi yeni bir ASP.NET sayfası projeye ekleyin ve öğeyi `Site.master` ana sayfa. Çözüm Gezgini'nde proje adının üzerine sağ tıklayın ve Yeni Öğe Ekle seçeneğini belirleyin. Web formu şablonunu seçin, bir ad girin `About.aspx`ve ardından Şekil 7'de gösterildiği gibi "Ana sayfa seçin" onay kutusunu işaretleyin. Bunun yapılması görüntüler seçin bir ana sayfa iletişim kullanmak için ana sayfa seçebileceğiniz gelen kutusuna (bkz. Şekil 8).

> [!NOTE]
> ASP.NET Web sitesi yerine Web sitesi proje modeli Web uygulaması projesi modelini kullanarak oluşturduysanız, Şekil 7'de gösterilen Yeni Öğe Ekle iletişim kutusunda "ana sayfa seçin" onay kutusunu görmezsiniz. Bir içerik oluşturmak için Web uygulaması projesi modelini kullanırken, sayfası yerine Web Görünümü Web içeriği formu şablon seçmeniz gerekir. Web içeriği formu şablonunu seçme ve Ekle seçeneğine tıkladıktan sonra aynı Şekil 8'de gösterilen iletişim kutusu görünür ana sayfa seçin.


[![Yeni bir içerik sayfası Ekle](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**Şekil 07**: yeni bir içerik sayfası ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))


[![Site.master ana sayfa seçin](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**Şekil 08**: seçin `Site.master` ana sayfa ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))


Aşağıdaki bildirim temelli biçimlendirme gösterildiği gibi yeni bir içerik sayfasını içeren bir `@Page` noktaları için ana sayfa ve içerik denetimi her ana sayfanın ContentPlaceHolder denetimleri için yedeklemenizi yönergesi.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> "Basit Site düzen oluşturma" bölümünde 1. Adım'ı yeniden adlandırıldı `ContentPlaceHolder1` için `MainContent`. Bu ContentPlaceHolder denetimin değil adlandırırsanız `ID` aynı şekilde, içerik sayfanızın bildirim temelli biçimlendirme biraz yukarıda gösterilen biçimlendirmeden farklılık gösterir. Yani, ikinci içerik denetimin `ContentPlaceHolderID` yansıtır `ID` ana sayfanızda karşılık gelen ContentPlaceHolder denetim.


İçerik sayfası oluştururken, ASP.NET altyapısı sayfanın Sigortası içerik denetimleri ile onun ana sayfanın ContentPlaceHolder denetimleri. ASP.NET altyapısı içerik sayfasının ana sayfadan belirler `@Page` yönergesinin `MasterPageFile` özniteliği. Yukarıdaki biçimlendirme gösterildiği gibi bu içerik sayfası bağlı `~/Site.master`.

Ana sayfaya iki ContentPlaceHolder denetimleri - olduğundan `head` ve `MainContent` -Visual Web Developer oluşturulan iki içerik denetimi. Her bir içerik denetimi aracılığıyla belirli bir ContentPlaceHolder başvuran kendi `ContentPlaceHolderID` özelliği.

Ana sayfalar parlama efektlerini önceki site genelinde şablon teknikleri üzerinden kendi tasarım zamanı desteği olduğu. Şekil 9 gösterir `About.aspx` Visual Web Developer'ın Tasarım görünümü görüntülendiğinde içerik sayfası. Not görünür ana sayfa içeriği olmakla birlikte, gri renkte ve değiştirilemez. Ana sayfanın ContentPlaceHolder için karşılık gelen içerik denetimleri, ancak düzenlenebilir. Ve tıpkı diğer ASP.NET sayfası ile içerik sayfasının arabirimi kaynak veya tasarım görünümleri aracılığıyla Web denetimleri ekleyerek oluşturabilirsiniz.


[![Sayfaya özgü ve ana sayfa içeriği içerik sayfasının Tasarım görünümü görüntüler](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**Şekil 09**: içerik sayfasının Tasarım görünümü görüntüler hem sayfaya özgü ve ana sayfa içeriği ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>İçerik sayfası için işaretleme ve Web denetimleri ekleme

Bazı içerik oluşturmak için birkaç dakikanızı `About.aspx` sayfası. Şekil 10'da "Yazar hakkında" başlığını ve birkaç paragraf metni girdiğim görebilir, ancak Web denetimleri çok eklemekten çekinmeyin gibi. Bu arabirim oluşturduktan sonra ziyaret `About.aspx` tarayıcısından sayfası.


[![Bir tarayıcı aracılığıyla About.aspx sayfasını ziyaret edin](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**Şekil 10**: ziyaret `About.aspx` sayfası aracılığıyla bir tarayıcı ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))


İstenen içerik sayfası ve onun ilişkili ana sayfa çarpım ve tamamen web sunucusunda bir bütün olarak işlenen anlamak önemlidir. Son kullanıcının tarayıcı sonra elde edilen, çarpım HTML olarak gönderilir. Bunu doğrulamak için Görünüm menüsüne giderek ve kaynağı seçme tarayıcı tarafından alınan HTML görüntüleyin. Çerçeve yok ya da tek bir pencerede iki farklı web sayfalarını görüntüleme için tüm diğer özel teknikleri olduğunu unutmayın.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Ana sayfa için mevcut bir ASP.NET sayfasına bağlama

Bu adımda gördüğümüz gibi ASP.NET web uygulaması için yeni bir içerik sayfası ekleme "ana sayfa seçin" onay kutusu denetimi ve ana sayfa çekme oldukça kolaydır. Ne yazık ki, mevcut bir ASP.NET sayfasına bir ana sayfaya dönüştürme gibi kolay değildir.

Mevcut bir ASP.NET sayfasına bir ana sayfa bağlamak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Ekleme `MasterPageFile` ASP.NET sayfasına ait öznitelik `@Page` uygun ana sayfaya işaret yönergesi.
2. Her ana sayfasında ContentPlaceHolder içerik denetimleri ekleme.
3. Seçmeli olarak kesin ve ASP.NET sayfanın var olan içeriğinin uygun içerik denetimleri yapıştırın. Ben "seçmeli olarak" Burada ASP.NET sayfasında nedeni büyük olasılıkla içeren ana sayfa tarafından gibi ifade zaten biçimlendirme söyleyin `DOCTYPE`, `<html>` öğesi ve Web formu.

Ekran görüntüleri ile birlikte bu işlemi adım adım yönergeler için kullanıma [Scott Guthrie](https://weblogs.asp.net/scottgu/)'s [kullanarak ana sayfalar ve Site gezintisi](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx) öğretici. "Güncelleştirme `Default.aspx` ve `DataSample.aspx` ana sayfada kullanılacak" bölümde adımları açıklanmaktadır.

Mevcut ASP.NET sayfaları, içerik sayfalarına dönüştürmek için daha yeni içerik sayfaları oluşturmak çok daha kolay olduğundan, ben oluşturduğunuzda yeni bir ASP.NET Web sitesi ana sayfa siteye eklemenizi öneririz. Tüm yeni ASP.NET sayfaları için bu ana sayfanın bağlayın. İlk ana sayfa çok basit veya düz ise endişelenmeyin; ana sayfayı daha sonra güncelleştirebilirsiniz.

> [!NOTE]
> Yeni bir ASP.NET uygulaması oluştururken, Visual Web Developer ekler bir `Default.aspx` ana sayfaya bağlı olmayan bir sayfa. Mevcut bir ASP.NET sayfasına bir içerik sayfasına dönüştürme uygulama istiyorsanız, bir tane ile bunu `Default.aspx`. Alternatif olarak, silebilirsiniz `Default.aspx` ve, ancak bu kez "Ana sayfa seçin" onay kutusu denetimi yeniden ekleyin.


## <a name="step-3-updating-the-master-pages-markup"></a>3. adım: asıl sayfa biçimlendirme güncelleştiriliyor

Ana sayfalar başlıca yararları tek bir ana sayfa sitesinde çok sayıda sayfaları için genel düzenini tanımlamak için kullanılabilir olmasıdır. Bu nedenle, sitenin görünüme güncelleştirme tek dosya - ana sayfaya güncelleştirilmesini gerektirir.

Bu davranış göstermek için şimdi ana sayfamızı üst kısmında, sol sütunda geçerli tarihi içerecek şekilde güncelleştirin. Adlı bir etiket ekleyin `DateDisplay` için `leftContent` `<div>`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

Ardından, oluşturun bir `Page_Load` olay işleyicisi ana sayfa ve aşağıdaki kodu ekleyin:

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

Yukarıdaki kod etiketin ayarlar `Text` özelliğine geçerli bir tarih ve saat, haftanın günü, ay ve iki basamaklı gün adını biçimlendirilmiş (bkz. Şekil 11). Bu değişiklik, içerik sayfalarınızdan birini yeniden ziyaret edin. Şekil 11 gösterildiği gibi sonuçta elde edilen biçimlendirme değişikliği ana sayfasına eklemek için hemen güncelleştirilir.


[![Yansıtılan zaman görüntüleme değişiklikler ana sayfa için içerik sayfası](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**Şekil 11**: değişiklikler ana sayfasına yansıyan ne zaman görüntüleme içerik sayfası ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))


> [!NOTE]
> Bu örnekte gösterildiği gibi sunucu tarafı Web denetimleri, kod ve olay işleyicileri ana sayfalar içerebilir.


## <a name="summary"></a>Özet

Ana sayfaları, ASP.NET geliştiricilerine güncelleştirilemez kolayca tutarlı bir site genelinde düzenini tasarlamak etkinleştirin. Visual Web Developer zengin tasarım zamanı desteği sunan gibi ana sayfalar ve bunların ilişkili içerik sayfalarını oluşturma standart ASP.NET sayfaları, oluşturma olarak kadar kolaydır.

Bu öğreticide oluşturduğumuz ana sayfası örneği iki ContentPlaceHolder denetimleri, head ve MainContent vardı. Biz yalnızca MainContent ContentPlaceHolder denetimi için biçimlendirme içerik sayfamızı ancak belirtildi. Sonraki öğreticide birden çok içerik kullanarak olan baktığımızda içerik sayfasındaki denetimleri. Ayrıca varsayılan tanımlama görüyoruz varsayılan kullanma arasında geçiş yapmak için biçimlendirme ana veritabanında sayfa ve içerik sayfasından özel biçimlendirme sağlayan tanımlanan nasıl biçimlendirme içeriği için ana sayfa içinde de denetler.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET tasarımcılarına yönelik: ücretsiz tasarım şablonları ve Kılavuzu Web standartlarını kullanarak ASP.NET Web siteleri oluşturmaya](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [ASP.NET ana sayfaları genel bakış](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Geçişli stil sayfaları (CSS) öğreticiler](http://www.w3schools.com/css/default.asp)
- [Başlığı dinamik olarak ayarlama](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [ASP.NET ana sayfaları](http://www.odetocode.com/articles/419.aspx)
- [Hızlı Başlangıç öğreticileri ana sayfa](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar 1998'de bu yana birden çok ASP/ASP.NET books ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileri ile çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott, konumunda ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Önceki](nested-master-pages-cs.md)
> [İleri](multiple-contentplaceholders-and-default-content-vb.md)
