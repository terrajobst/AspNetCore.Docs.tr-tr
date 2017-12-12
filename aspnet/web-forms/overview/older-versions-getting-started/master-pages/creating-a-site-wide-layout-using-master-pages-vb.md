---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: "Ana sayfalar (VB) kullanarak bir Site genelinde düzen oluşturma | Microsoft Docs"
author: rick-anderson
description: "Bu öğretici, ana sayfa temel bilgileri gösterir. Yani, ana sayfalar nelerdir nasıl mu bir ana sayfa oluşturma, içerik yer tutucu nelerdir nasıl bir cr mu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 69f73085198c79c01988aab9e63f3ce9e7647034
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-site-wide-layout-using-master-pages-vb"></a>Ana sayfalar (VB) kullanarak bir Site genelinde düzeni oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip) veya [PDF indirin](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> Bu öğretici, ana sayfa temel bilgileri gösterir. Yani, ana sayfalar nelerdir, nasıl oluşturmak bir ana sayfa İçerik yer tutucu nelerdir, nasıl oluşturun, nasıl değiştirme ana sayfaya otomatik olarak kendi ilişkili içerik sayfaları ve benzeri yansıtılır bir ana sayfa kullanan bir ASP.NET sayfası.


## <a name="introduction"></a>Giriş

Bir iyi tasarlanmış bir Web sitesinin tutarlı site genelinde sayfa düzeni özniteliğidir. Örneğin www.asp.net Web sitesinde alın. Bu yazma zaman her sayfanın en üstündeki ve sayfanın aynı içeriğe sahip. Şekil 1'de görüldüğü gibi her sayfanın üstündeki gri çubukta Microsoft Communities listesini görüntüler. Diğer bir deyişle site logosu, içine site çevrilen dilleri ve çekirdek bölümleri listesi altında: Giriş, Başlarken, daha fazla bilgi edinin, indirmeler ve benzeri. Benzer şekilde, sayfanın www.asp.net, telif hakkı bildirimi ve gizlilik bildirimine bir bağlantı reklam hakkında bilgi içerir.


[![Tüm sayfalar arasında tutarlı bir görünüm www.asp.net Web sitesi uygular](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

**Şekil 01**: Web sitesi www.asp.net tutarlı aramak ve tüm sayfaları arasında eşitleyerek kullanır ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))


Başka bir iyi tasarlanmış bir sitenin site görünümünü değiştirilebilmesi için kolay bir özniteliktir. Şekil 1 Mart 2008 itibariyle www.asp.net giriş sayfası gösterir, ancak şimdi ve bu öğreticinin yayın arasında görünüm değişmiş olabilir. Belki de üstünde menü öğeleri, MVC çerçevesi için yeni bir bölüm eklemek için genişletilir. Veya belki farklı renkleri, yazı tipleri ve düzeni tüketicisinin yeni tasarımla unveiled olacaktır. Tüm site gibi değişiklikler uygulanırken sitesini oluşturan web sayfaları binlerce değiştirme gerektirmez hızlı ve basit bir süreç olmalıdır.

Site genelinde sayfa şablon ASP.NET oluşturma kullanılarak olası *ana sayfalar*. Buna koysalar bir ana sayfa tüm arasında ortak olan biçimlendirme tanımlar ASP.NET sayfası özel türüdür *içerik sayfalarının* temelinde içerik sayfası içeriği sayfa özelleştirilebilir bölgeler yanı sıra. (Bir içerik ana sayfaya bağlı bir ASP.NET sayfası sayfasıdır.) Ana sayfanın düzenini veya biçimlendirme değiştirildiğinde, kendi içerik sayfaları çıktısı tümünün benzer şekilde hemen güncelleştirilir, site genelinde görünüm değişiklikleri güncelleştirme ve tek bir dosya (yani, ana sayfa) dağıtımı kolay uygulama hale getirir.

Ana sayfalar kullanarak araştırmaya eğitim serileri ilk öğreticide budur. Bu öğretici seri seyri üzerinden biz:

- Ana sayfalar ve bunların ilişkili içerik sayfaları oluşturma inceleyin,
- Çeşitli ipuçlarını, püf noktaları ve tuzakları ele,
- Ortak ana sayfa Tuzaklar tanımlamak ve geçici çözümler keşfedin
- Bir içerik sayfasını ve tersi bir ana sayfaya erişmek bkz,
- Çalışma zamanında bir içerik sayfasının ana sayfasında belirleme konusunda bilgi edinin ve
- Diğer ana sayfa konuları Gelişmiş.

Bu öğreticileri, kısa ve görsel olarak işleminde size kılavuzluk için yeterince ekran görüntüleri ile adım adım yönergeler sağlamak için sağlamıştır. Her öğretici C# ve Visual Basic sürümlerinde kullanılabilir ve kullanılan tam kod yüklenmesini içerir.

Ana sayfa temel göz bülteninin açılış sayısına Bu öğretici başlar. Biz nasıl iş ana sayfa ele almaktadır, bir ana sayfa ve Visual Web Developer kullanarak ilişkili içerik sayfaları oluşturma sırasında arayın ve nasıl değişiklikler bir ana sayfa için içerik sayfalarında hemen yansıtılır bakın. Haydi başlayalım!

## <a name="understanding-how-master-pages-work"></a>Ana iş nasıl sayfa anlama

Tutarlı site genelinde sayfa düzeni ile bir Web sitesi oluşturmanın her web sayfası özel içeriği yanı sıra ortak biçimlendirme biçimlendirme yayma gerektirir. Örneğin, her Öğreticisi veya forum gönderisi www.asp.net konusunda benzersiz içeriklerini varken bu sayfaların her biri de işlemek ortak bir dizi `<div>` en üst düzey bölüm bağlantılar görüntüler öğeleri: Giriş, Başlarken, daha fazla bilgi edinin ve benzeri.

Web sayfaları ile tutarlı bir görünüm ve kullanımında oluşturma teknikleri çeşitli vardır. Naïve yaklaşım yalnızca kopyalamak ve tüm web sayfalarına ortak yerleşim biçimlendirme yapıştırmak için ancak bu yaklaşım olumsuzlukları bir sayı. Yeni başlayanlar, yeni bir sayfa her oluşturulduğunda kopyalayıp paylaşılan içerik sayfaya yapıştırın unutmamanız gerekir. Yalnızca bir alt paylaşılan biçimlendirme yeni bir sayfaya yanlışlıkla kopyalamak gibi böyle kopyalama ve yapıştırma işlemlerini hatası için hazır. Ve üst için bu yaklaşım gerçek sorun sitesindeki her sayfada yeni görünüm kullanmak için düzenlenmesi gerekir çünkü ile yeni bir tane var olan site genelinde görünümünü değiştirme yapar.

ASP.NET 2.0 sürümünde önce sayfasında geliştiriciler genellikle ortak biçimlendirmede yerleştirilen [kullanıcı denetimleri](https://msdn.microsoft.com/en-us/library/y6wb1a0e.aspx) ve ardından bu kullanıcı denetimleri her sayfasına eklenen. Bu yaklaşım sayfasında geliştirici kullanıcı denetimleri için her yeni sayfa el ile eklemeyi unutmayın, ancak yalnızca kullanıcı denetimleri ortak biçimlendirme güncelleştirirken değiştirilmesi gereken olduğundan daha kolay site genelinde değişiklikler için izin gerekiyor. Ne yazık ki, ASP.NET 1.x uygulamalar kullanıcı denetimleri Tasarım görünümünde gri kutuları olarak çizilir.-oluşturmak için Visual Studio .NET 2002 ve 2003 - Visual Studio sürümlerinde kullanılabilir. Sonuç olarak, bu yaklaşımı kullanarak sayfa geliştiricileri WYSIWYG tasarım zamanı ortamı keyifli değil.

Kullanıcı denetimleri kullanarak eksiklikleri ASP.NET sürüm 2.0 ve Visual Studio 2005 başlanmasıyla ele alınan *ana sayfalar*. Bir ana sayfa site genelinde işaretleme tanımlar ASP.NET sayfası özel türüdür ve *bölgeleri* ilişkili olduğu *içerik sayfalarının* kendi özel biçimlendirme tanımlayın. Adım 1'de göreceğiz gibi bu bölgeler ContentPlaceHolder denetimleri tarafından tanımlanır. ContentPlaceHolder denetimini yalnızca bir içerik sayfası tarafından özel içerik nerede yerleştirilebilir ana sayfanın denetim hiyerarşisi konumda gösterir.

> [!NOTE]
> Ana sayfalar işlevselliği ve temel kavramlar, ASP.NET 2.0 sürümünde bu yana değişmemiştir. Ancak, Visual Studio 2008 iç içe geçmiş ana sayfalar için tasarım zamanı desteği Visual Studio 2005'te eksik bir özellik sunar. İç içe geçmiş ana sayfalar gelecekteki bir öğreticide kullanarak ele alacağız.


Şekil 2 www.asp.net ana sayfaya nasıl görünebileceği gösterilmiştir. Ana sayfaya ContentPlaceHolder Orta-tek tek her web sayfası için benzersiz içeriğin bulunduğu sol, hem de genel site genelinde düzenini - üstünde, alt ve sağ her sayfanın biçimlendirme - tanımlar unutmayın.


![Bir içerik sayfası içeriği sayfa temelinde bir ana sayfa Site genelinde düzeni ve düzenlenebilir bölgeler tanımlar](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**Şekil 02**: bir içerik sayfası içeriği sayfa temelinde bir ana sayfa Site genelinde düzeni ve düzenlenebilir bölgeler tanımlar


Bir ana sayfa tanımlandıktan sonra bir onay kutusu onay aracılığıyla yeni ASP.NET sayfaları için bağlanabilir. -İçerik sayfaları adlı - bu ASP.NET sayfaları her ana sayfanın ContentPlaceHolder denetimleri için içerik denetimi içerir. Bir tarayıcı üzerinden içerik sayfasını ziyaret edildiğinde ASP.NET altyapısı ana sayfanın denetim hiyerarşisi oluşturur ve içerik sayfasının denetim hiyerarşisi uygun yerlerde yerleştirir. Bu birleşik denetim hiyerarşisi işlenir ve sonuçta elde edilen HTML son kullanıcının tarayıcıya döndürülür. Sonuç olarak, içerik sayfasını kendi ana sayfası ContentPlaceHolder denetimleri dışında tanımlanan ortak biçimlendirme ve kendi içerik denetimleri içinde tanımlanan sayfaya özgü biçimlendirme yayar. Şekil 3'te bu kavramı gösterir.


[![İstenen sayfanın biçimlendirme ana sayfası'na Sigortalı](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**Şekil 03**: İstenen sayfanın biçimlendirme ana sayfası'na Sigortalı ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))


Biz nasıl iş ana sayfa ele, bir ana sayfa ve Visual Web Developer kullanarak ilişkili içerik sayfaları oluşturma bir bakalım.

> [!NOTE]
> Geniş olası hedef kitle ulaşabilmeniz için biz derleme Bu öğretici seri ASP.NET Web sitesi ASP.NET 3.5 Microsoft'un ücretsiz Visual Studio 2008 sürümüyle kullanılarak oluşturulacak [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Sahip değilse henüz ASP.NET 3.5 için yükseltilmiş endişelenmeyin - bu öğreticileri işlerinde açıklanan kavramları eşit ile ASP.NET 2.0 ve Visual Studio 2005 yanı sıra. Ancak, bazı demo uygulamalar için .NET Framework sürüm 3.5 yeni özellikleri kullanabilir; 3.5 özgü özellikler kullanıldığında, benzer bir işlevsellik sürüm 2.0 uygulamak nasıl ele Not içerir. Tanıtım uygulamaları için kullanılabilir öğretici her hedef .NET sonuçlanır Framework sürüm 3.5, indirme göz önünde bulundurmanız bir `Web.config` 3.5 özel yapılandırma öğeleri içeren dosya. Yazıyı henüz sonra indirilebilir web uygulaması bilgisayarınızda yüklü .NET 3.5 yüklemek varsa kısa 3.5 özgü biçimlendirmeden kaldırmadan çalışmaz `Web.config`. Bkz: [Dissecting ASP.NET sürüm 3.5's `Web.config` dosya](http://www.4guysfromrolla.com/articles/121207-1.aspx) Bu konu hakkında daha fazla bilgi için.


## <a name="step-1-creating-a-master-page"></a>1. adım: bir ana sayfa oluşturma

Biz oluşturma ve ana ve içerik sayfalarını kullanarak keşfedebilirsiniz önce ilk ASP.NET Web sitesi gerekiyor. Yeni bir dosya sistemi tabanlı ASP.NET Web sitesi oluşturmaya başlayın. Bunu başarmak için Visual Web Developer başlatın ve sonra Dosya menüsüne gidin ve yeni Web sitesi, yeni Web sitesi iletişim kutusu görüntüleniyor seçin (bkz: Şekil 4) kutusunda. ASP.NET Web sitesi şablonunu seçin, dosya sistemine konum aşağı açılan listesi ayarlamak, web sitesi yerleştirmek için bir klasör seçin ve Visual Basic Dil ayarlayın. Bu yeni bir web sitesi ile oluşturacak bir `Default.aspx` ASP.NET sayfası, bir `App_Data` klasörünü ve bir `Web.config` dosya.

> [!NOTE]
> Visual Studio Proje yönetimi iki modlarını destekler: Web sitesi projeleri ve Web Uygulama projeleri. Web sitesi projeleri, Web uygulaması projelerine taklit proje mimarisi, Visual Studio .NET 2002/2003 - proje dosyasını içerir ve yerleştirilir tek bir derleme halinde projenin kaynak kodu derleme ise bir proje dosyası eksikliği `/bin` klasör. Web uygulama projesi modeli Service Pack 1'yeniden olsa da visual Studio 2005 başlangıçta yalnızca desteklenen Web sitesi, projeleri; Visual Studio 2008 her iki proje modelleri sunar. Ancak, Visual Web Developer 2005 ve 2008 sürümlerinde, yalnızca Web sitesi projeleri destekler. Bu öğretici serisinde my gösterileri ı Web sitesi projesini modelini kullanır. Express olmayan sürüm kullanıyorsanız ve kullanmak istediğiniz [Web uygulama projesi modeli](https://msdn.microsoft.com/en-us/library/aa730880(vs.80).aspx) bunun yerine, bunu ancak olabileceğini bazı tutarsızlıklar ekranınızı ve karşı gerçekleştirmeniz gereken adımlar gördükleri arasında unutmayın çekinmeyin gösterilen ekran görüntüleri ve bu öğreticileri sağlanan yönergeleri.


[![Yeni bir dosya sistemi tabanlı Web sitesi oluşturma](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**Şekil 04**: New File System-Based Web sitesi oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))


Ardından, bir ana sayfa sitenin kök dizininde proje adına sağ tıklatın, yeni öğe Ekle seçme ve ana sayfa şablonu seçilmesi ekleyin. Ana sayfalar uzantısıyla biten Not `.master`. Bu yeni ana sayfa adı `Site.master` ve Ekle'yi tıklatın.


[![Bir ana sayfa eklemek Site.master Web sitesine adlı](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**Şekil 05**: bir ana sayfa adı eklemek `Site.master` Web sitesine ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))


Visual Web Developer aracılığıyla yeni bir ana sayfa dosyası ekleme bir ana sayfa ile aşağıdaki bildirim temelli biçimlendirme oluşturur:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

Bildirim temelli biçimlendirmede ilk satır [ `@Master` yönergesi](https://msdn.microsoft.com/en-us/library/ms228176.aspx). `@Master` Yönergesi benzer [ `@Page` yönergesi](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx) ASP.NET sayfaları görüntülenir. Sunucu tarafı dili (VB) ve ana sayfa arka plan kodu sınıfın devralma ve konumu hakkında bilgi tanımlar.

`DOCTYPE` Ve sayfanın bildirim temelli biçimlendirme altında görünür `@Master` yönergesi. Dört sunucu tarafı denetimlerle birlikte statik HTML sayfası içerir:

- **Bir Web formu ( `<form runat="server">`)** - tüm ASP.NET sayfaları genellikle Web formu - sahiptir ve ana sayfa bir Web Form içinde - gösterilmesi gerekir Web denetimleri içerebilir çünkü ana sayfanızın (bir Web formu e eklemek yerine için Web formu eklediğinizden emin olun çünkü ACH içerik sayfası).
- **Adlı bir ContentPlaceHolder denetimi `ContentPlaceHolder1`**  -bu ContentPlaceHolder denetimi, Web Form içinde görünür ve içerik sayfasının kullanıcı arabirimi için bölge görevi görür.
- **Sunucu tarafı `<head>` öğesi** - `<head>` öğeye sahip `runat="server"` özniteliği, sunucu tarafı kodu üzerinden erişilebilir hale getirme. `<head>` Öğesi, bu şekilde gerçekleştirilir böylece sayfanın başlığı ve diğer `<head>`-ilgili biçimlendirme eklenen veya program aracılığıyla ayarlanır. Örneğin, bir ASP.NET sayfasının ayarlama `Title` özellik değişikliklerini `<title>` öğesi çizilir tarafından `<head>` sunucu denetimi.
- **Adlı bir ContentPlaceHolder denetimi `head`**  -içinde bu ContentPlaceHolder denetimi görünür `<head>` sunucu denetlemek ve bildirimli olarak içeriği eklemek için kullanılan `<head>` öğesi.

Bu varsayılan ana sayfa tanımlayıcı biçimlendirme kendi ana sayfalar tasarlamak için bir başlangıç noktası olarak görev yapar. Ek Web denetimleri veya ContentPlaceHolders için ana sayfaya eklemek için veya HTML düzenlemek için çekinmeyin.

> [!NOTE]
> Bir ana sayfa tasarlama yaparken emin, ana sayfa bir Web formu içerir ve bu Web formu içinde en az bir ContentPlaceHolder denetleyen görünür.


### <a name="creating-a-simple-site-layout"></a>Basit Site düzenini oluşturma

Şimdi genişletin `Site.master`ait olduğu tüm sayfaları paylaşmak site düzenini oluşturmak için varsayılan bildirim temelli biçimlendirme: ortak üstbilgisi; gezinti, haber ve diğer site genelinde içeriği; ve "Gücü tarafından Microsoft ASP.NET" simgesi görüntüleyen altbilgi sol sütun. Bir içerik sayfalarını bir tarayıcıdan görüntülendiğinde Şekil 6 ana sayfa sonu sonucunu gösterir. Şekil 6 kırmızı daire içinde bölgede ziyaret sayfa belirli (`Default.aspx`); diğer tüm içerik sayfalardaki ana sayfada tanımlıysa ve bu nedenle tutarlı içeriktir.


[![Ana sayfa işaretleme, üst, sol ve alt bölümleri tanımlar.](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**Şekil 06**: üst, sol ve alt bölümleri için ana sayfa biçimlendirme tanımlar ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image16.png))


Şekil 6'daki site düzenini elde etmek için başlangıç güncelleştirerek `Site.master` aşağıdaki bildirim temelli biçimlendirme içeren ana sayfa:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

Ana sayfanın düzenini bir dizi kullanılarak tanımlanan `<div>` HTML öğeleri. `topContent` `<div>` Her sayfanın en üstünde görünür biçimlendirme içeriyor ancak `mainContent`, `leftContent`, ve `footerContent` `<div>` s sayfanın içeriğinin, sol sütunda ve "gücü tarafından Microsoft görüntülemek için kullanılır ASP.NET"simgesi, sırasıyla. Bunlar ekleme yanı sıra `<div>` öğeleri, ı adlandırılmasını `ID` birincil ContentPlaceHolder denetiminden özelliğinin `ContentPlaceHolder1` için `MainContent`.

Bu getirilebilir biçimlendirme ve yerleştirme kuralları `<div>` öğeleri yazıyla içinde [geçişli stil sayfası (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) dosya `Styles.css`, aracılığıyla belirtilen bir `<link>` ana sayfa öğesinde`<head>`öğesi. Görünüm ve yapısını her çeşitli kurallar tanımlamak `<div>` öğesi not ettiğiniz üstünde. Örneğin, `topContent` `<div>` "Ana sayfa öğreticileri" metin ve bağlantıyı görüntüler, öğeye sahip belirtilen biçimlendirme kurallarını `Styles.css` gibi:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

Bilgisayarınızda aşağıdaki, bu öğreticinin eşlik eden kodu indirin ve eklemek gerekir `Styles.css` projenize dosya. Benzer şekilde, siz de adlı bir klasör oluşturmanız gerekir `Images` ve "Gücü tarafından Microsoft ASP.NET" simgesini indirilen demo Web sitesinden projenize kopyalayın.

> [!NOTE]
> CSS ve web sayfası biçimlendirme tartışması bu makalenin kapsamı dışındadır ' dir. CSS hakkında daha fazla bilgi için kullanıma [CSS öğreticileri](http://www.w3schools.com/css/default.asp) adresindeki [W3Schools.com](http://www.w3schools.com/). I de bu öğreticinin eşlik eden kod indirmenizi ve CSS ayarlarında oynamak teşvik `Styles.css` farklı biçimlendirme kuralları etkisini görmek için.


### <a name="creating-a-master-page-using-an-existing-design-template"></a>Varolan bir tasarım şablonu kullanarak bir ana sayfa oluşturma

Yıllar içinde ı küçük-Orta boyutlu şirketler için ASP.NET web uygulamalarının sayısı temel aldık. İstemcilerim bazıları kullanmak istedikleri var olan bir site düzenini vardı; diğer bir yetkin grafik Tasarımcı işe. Birkaç bana Web sitesi Düzen tasarlamak için alma. Şekil 6 ile anlayabilirsiniz gibi bir Web sitesinin Düzen tasarlamak için programcı görev, vergiler, doctor mu sırada open-heart surgery gerçekleştirmek, muhasebe bölümü sahip olarak genellikle olarak akıllıca olur.

Neyse ki, boş HTML tasarım şablonları sunar innumerous Web siteleri vardır - Google altı milyondan fazla sonuç için arama terimi "ücretsiz Web sitesi şablonları" döndürdü. Sık kullanılan my olanları biri [OpenDesigns.org](http://opendesigns.org/). CSS dosyaları ve görüntüleri Web sitesi projenize ekleyin ve şablonun HTML ana sayfanıza tümleştirme gibi bir Web sitesi şablonu bulduktan sonra.

> [!NOTE]
> Microsoft ayrıca, bir dizi sunar [ASP.NET tasarım Başlat Seti şablonları serbest](https://msdn.microsoft.com/en-us/asp.net/aa336613.aspx) Visual Studio'da yeni bir Web sitesi iletişim kutusuna tümleştirin.


## <a name="step-2-creating-associated-content-pages"></a>2. adım: İlişkili içerik sayfaları oluşturma

Oluşturulan ana sayfa ile biz ana sayfaya bağlı ASP.NET sayfaları oluşturmaya başlamak hazırsınız. Bu tür sayfaları denir *içerik sayfalarının*.

Şimdi yeni bir ASP.NET sayfası projeye ekleyin ve onu bağladıktan `Site.master` ana sayfa. Çözüm Gezgini'nde proje adına sağ tıklayın ve Yeni Öğe Ekle seçeneğini belirleyin. Web Form şablonunu seçin, bir ad girin `About.aspx`ve ardından "Select ana sayfa" Şekil 7'de gösterildiği gibi onay. Bunun yapılması görüntüler Seç bir ana sayfa iletişim kutusu (bkz. Şekil 8) kullanmak için ana sayfa seçebileceğiniz gelen kutusu.

> [!NOTE]
> Web sitesi projesini modeli yerine Web uygulama projesi modelini kullanarak ASP.NET Web sitenizi oluşturduysanız, Şekil 7'de gösterilen Yeni Öğe Ekle iletişim kutusunda "ana sayfa seç" onay kutusunu görmez. Bir içerik oluşturmak için Web uygulaması projesi modelini kullanırken, sayfa Web Form şablonunu yerine Web içeriği Form şablonu seçmelisiniz. Web içerik Form şablonunu seçerek ve Ekle düğmesine sonra aynı Şekil 8'de gösterilen iletişim kutusu görünür bir ana sayfa seçin.


[![Yeni bir içerik sayfa ekleyin](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**Şekil 07**: yeni bir içerik sayfası ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))


[![Site.master ana sayfa seçin](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**Şekil 08**: seçin `Site.master` ana sayfa ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))


Aşağıdaki bildirim temelli biçimlendirme gösterildiği gibi yeni bir içerik sayfasını içeren bir `@Page` noktaları için ana sayfa ve içerik denetimi her ana sayfanın ContentPlaceHolder denetimleri için yedeklemeniz yönergesi.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> 1. Adım'daki "Basit Site düzenini oluşturma" bölümünde t yeniden adlandırılmış `ContentPlaceHolder1` için `MainContent`. Bu ContentPlaceHolder denetimin değil adlandırırsanız `ID` aynı şekilde, içerik sayfanızın bildirim temelli biçimlendirme biraz yukarıda gösterilen biçimlendirmeden farklılık gösterir. Yani, ikinci içerik denetimin `ContentPlaceHolderID` yansıtılacaktır `ID` buna karşılık gelen ContentPlaceHolder ana sayfanızın denetim.


Bir içerik sayfasını işlerken ASP.NET altyapısı sayfanın Sigortası içerik kendi ana sayfanın ContentPlaceHolder denetimleri ile denetimleri. ASP.NET altyapısı içerik sayfasının ana sayfa belirler `@Page` yönergesi'nın `MasterPageFile` özniteliği. Yukarıdaki biçimlendirme gösterildiği gibi bu içerik sayfasını bağlı `~/Site.master`.

İki ContentPlaceHolder denetimi - ana sayfası olduğu için `head` ve `MainContent` -Visual Web Developer oluşturulan iki içerik denetimleri. Her içerik denetimi aracılığıyla belirli bir ContentPlaceHolder başvuran kendi `ContentPlaceHolderID` özelliği.

Ana sayfalar bekliyoruz önceki site genelinde şablon teknikleri üzerinden kendi tasarım zamanı desteğiyle olduğu. Şekil 9 gösterir `About.aspx` Visual Web Developer'ın Tasarım görünümü görüntülendiğinde içerik sayfası. Ana sayfa içeriği görünür olsa da, gri renkte görüntülenir ve değiştirilemez unutmayın. Ana sayfanın ContentPlaceHolders için için karşılık gelen içerik denetimleri, ancak düzenlenebilir. Ve gibi diğer ASP.NET sayfası ile içerik sayfasının arabirimi kaynak ya da Tasarım görünümleri aracılığıyla Web denetimler ekleyerek oluşturabilirsiniz.


[![Her iki sayfaya özgü ve ana sayfa içeriğini içerik sayfasının Tasarım görünümü görüntüler](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**Şekil 09**: içerik sayfasının Tasarım görünümü görüntüler hem sayfa özgü ve ana sayfa içeriğini ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>İçerik sayfasına işaretleme ve Web denetimleri ekleme

Bazı içerik oluşturmak için bir dakikanızı ayırın `About.aspx` sayfası. Şekil 10'da "Yazar hakkında" Başlık ve metin paragraflarını birkaç girilen bakın, ancak Web denetimleri çok eklemekten çekinmeyin gibi. Bu arabirim oluşturduktan sonra ziyaret `About.aspx` bir tarayıcı aracılığıyla sayfası.


[![Bir tarayıcı aracılığıyla About.aspx sayfasını ziyaret edin](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**Şekil 10**: ziyaret `About.aspx` sayfa aracılığıyla bir tarayıcı ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))


İstenen içerik sayfasını ve onun ilişkili ana sayfa Sigortalı ve web sunucusunda tamamen bir bütün olarak işlenen anlamak önemlidir. Son kullanıcının tarayıcı sonra ortaya çıkan, Sigortalı HTML olarak gönderilir. Bunu doğrulamak için Görünüm menüsüne giderek ve kaynağı seçme tarayıcı tarafından alınan HTML görüntüleyin. Çerçeve yok veya tek bir pencerede iki farklı web sayfalarını görüntüleme herhangi diğer özel teknikleri olduğuna dikkat edin.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Mevcut bir ASP.NET sayfası bir ana sayfa bağlama

Bu adımda gördüğümüz gibi ASP.NET web uygulaması için yeni bir içerik sayfası ekleme "ana sayfa seç" onay kutusu denetimi ve ana sayfa çekme olarak kadar kolaydır. Ne yazık ki, mevcut bir ASP.NET sayfasının bir ana sayfaya dönüştürme gibi kolay değildir.

Mevcut bir ASP.NET sayfası bir ana sayfa bağlamak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Ekleme `MasterPageFile` özniteliği ASP.NET sayfa için `@Page` uygun ana sayfaya işaret eden yönergesi.
2. Her ana sayfasında ContentPlaceHolders için içerik denetimleri ekleyin.
3. Seçmeli olarak kesin ve ASP.NET sayfanın var olan içeriğinin uygun içerik denetimlere yapıştırın. I "seçmeli olarak" Buraya ASP.NET sayfa nedeni büyük olasılıkla içeren zaten ana sayfa tarafından gibi ifade biçimlendirme say `DOCTYPE`, `<html>` öğesi ve Web formu.

Ekran görüntüleri ile birlikte bu işlemi adım adım yönergeler için kullanıma [Scott Guthrie](https://weblogs.asp.net/scottgu/)'s [kullanarak ana sayfalar ve Site gezintisi](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx) Öğreticisi. "Güncelleştirme `Default.aspx` ve `DataSample.aspx` ana sayfa kullanmak için" bölümünde ayrıntılı adımları.

Mevcut ASP.NET sayfaları içerik sayfalarına dönüştürmek için daha yeni içerik sayfaları oluşturmak çok daha kolay olduğundan, ı oluşturduğunuz her yeni bir ASP.NET Web sitesi ana sayfa siteye eklemenizi öneririz. Tüm yeni ASP.NET sayfaları bu ana sayfa bağlayın. İlk ana sayfa çok basit veya düz ise endişelenmeyin. ana sayfaya daha sonra güncelleştirebilirsiniz.

> [!NOTE]
> Yeni bir ASP.NET uygulaması oluştururken, Visual Web Developer ekler bir `Default.aspx` bir ana sayfaya bağlı değilse sayfası. Mevcut ASP.NET sayfası içerik sayfasına dönüştürme uygulama istiyorsanız, bir tane ile bunu `Default.aspx`. Alternatif olarak, silebilirsiniz `Default.aspx` ve, ancak bu kez "Select ana sayfa" onay kutusu denetimi yeniden ekleyin.


## <a name="step-3-updating-the-master-pages-markup"></a>3. adım: ana sayfanın biçimlendirme güncelleştirme

Ana sayfalar başlıca yararları tek bir ana sayfa sitesinde çok sayıda sayfaları için genel düzeni tanımlamak için kullanılabilir biridir. Bu nedenle, sitenin görünüm güncelleştirme tek bir dosya - ana sayfaya güncelleştirilmesini gerektirir.

Bu davranış göstermek için şimdi ana sayfamızı üst kısmındaki sol sütunda geçerli tarih içerecek şekilde güncelleştirin. Adlı bir etiket eklemek `DateDisplay` için `leftContent` `<div>`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

Ardından, oluşturun bir `Page_Load` olay işleyicisi için ana sayfa ve aşağıdaki kodu ekleyin:

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

Yukarıdaki kod etiketin ayarlar `Text` özelliğine geçerli tarih ve saat, haftanın günü, ayı ve günü iki basamaklı adı biçimlendirilmiş (bkz. Şekil 11). Bu değişiklikle içerik sayfalarına birini yeniden ziyaret. Şekil 11 gösterildiği gibi sonuçta elde edilen biçimlendirme hemen ana sayfaya değişiklik içerecek şekilde güncelleştirilmiştir.


[![Yansıtılan zaman görüntüleme değişikliklerdir ana sayfa için bir içerik sayfası](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**Şekil 11**: değişikliklerdir ana sayfa için yansıtılan ne zaman görüntüleme bir içerik sayfası ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))


> [!NOTE]
> Bu örnekte gösterildiği gibi sunucu tarafı Web denetimleri, kodu ve olay işleyicileri ana sayfalar içerebilir.


## <a name="summary"></a>Özet

Ana sayfalar kolayca güncelleştirilebilir tutarlı bir site genelinde Düzen tasarlamak ASP.NET geliştiricilerinin etkinleştirin. Visual Web Developer zengin tasarım zamanı desteği sunar gibi ana sayfalar ve bunların ilişkili içerik sayfaları oluşturma standart ASP.NET sayfaları oluşturma olarak kadar basittir.

Bu öğreticide oluşturduğumuz ana sayfa örnek iki ContentPlaceHolder denetimleri, head ve MainContent vardı. Biz yalnızca MainContent ContentPlaceHolder denetimi için biçimlendirme içerik sayfamızı ancak belirtilen. Sonraki öğreticide biz birden çok içerik kullanarak ara içerik sayfasındaki denetimleri. Biz de varsayılan tanımlamak bkz. varsayılan kullanma arasında geçiş yapmak için işaretleme master sayfa ve içerik sayfasındaki özel biçimlendirme sağlayarak tanımlanan nasıl biçimlendirme içerik için ana sayfa içinde de denetler.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET tasarımcıları için: serbest tasarım şablonları ve Kılavuzu ASP.NET Web siteleri Web standartlarını kullanarak oluşturma](https://msdn.microsoft.com/en-us/asp.net/aa336602.aspx)
- [ASP.NET ana sayfalar genel bakış](https://msdn.microsoft.com/en-us/library/wtxbf3hh.aspx)
- [Geçişli stil sayfaları (CSS) öğreticileri](http://www.w3schools.com/css/default.asp)
- [Sayfanın başlığı dinamik olarak ayarlama](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [ASP.NET ana sayfalar](http://www.odetocode.com/articles/419.aspx)
- [Hızlı Başlangıç öğreticileri ana sayfa](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar birden çok ASP/ASP.NET books ve 4GuysFromRolla.com kurucusu, 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Tan adresindeki ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog aracılığıyla [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Önceki](nested-master-pages-cs.md)
[sonraki](multiple-contentplaceholders-and-default-content-vb.md)
