---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: "Başlık, Meta etiketler ve diğer HTML üstbilgileri ana sayfa (VB) belirtme | Microsoft Docs"
author: rick-anderson
description: "Getirilebilir tanımlamak için farklı teknikleri bakan &lt;head&gt; içerik sayfasından ana sayfa öğelerinde."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 1bbc2efc67d2d828dd0a5c1fcfe95145e8ffb2cb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>Başlık, Meta etiketler ve diğer HTML üstbilgileri ana sayfa (VB) belirtme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip) veya [PDF indirin](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> Getirilebilir tanımlamak için farklı teknikleri bakan &lt;head&gt; içerik sayfasından ana sayfa öğelerinde.


## <a name="introduction"></a>Giriş

Visual Studio 2008'de oluşturulan yeni ana sayfa varsa, varsayılan olarak, iki ContentPlaceHolder denetimi: adlı bir `head`ve bulunan `<head>` öğesi; ve adlı bir `ContentPlaceHolder1`, Web Form içinde yerleştirilmiş. Amacı `ContentPlaceHolder1` bir sayfa tarafından temelinde özelleştirilebilir Web formunda bir bölge tanımlamaktır. `head` ContentPlaceHolder sağlayan özel içeriğe ekleme sayfaları `<head>` bölümü. (Doğal olarak, bu iki ContentPlaceHolders için değiştirilmiş veya kaldırılabilir ve ek ContentPlaceHolder ana sayfaya eklenebilir. Ana sayfamızı `Site.master`, şu anda dört ContentPlaceHolder denetimleri vardır.)

HTML `<head>` öğesi, belge parçası olmayan web sayfası belge hakkında bilgi için depo görür. Dosyaları arama motorları veya iç gezginleri ve RSS akışları, JavaScript ve CSS gibi dış kaynaklara bağlantılar tarafından kullanılan meta bilgileri, bu web sayfasının başlığı gibi bilgileri içerir. Bu bilgilerin bazıları, Web sitesindeki tüm sayfalara ilgili olabilir. Örneğin, genel olarak aynı CSS kuralları ve her ASP.NET sayfası için JavaScript dosyaları almak isteyebilirsiniz. Ancak, bazı bölümleri vardır `<head>` sayfaya özgü öğesi. Sayfa başlığı birincil bir örnektir.

Bu öğreticide genel ve özel sayfa nasıl tanımlanacağı inceleyeceğiz `<head>` bölüm biçimlendirme ana sayfa ve içerik sayfaları.

## <a name="examining-the-master-pagesheadsection"></a>Ana sayfanın inceleniyor`<head>`bölümü

Visual Studio 2008 tarafından oluşturulan varsayılan ana sayfa dosyası aşağıdaki biçimlendirmede içeren kendi `<head>` bölümü:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

Dikkat `<head>` öğesi içeren bir `runat="server"` özniteliği bir sunucu denetimi (yerine statik HTML) olduğunu gösterir. Tüm ASP.NET sayfaları türetin [ `Page` sınıfı](https://msdn.microsoft.com/en-us/library/system.web.ui.page.aspx), bulunan `System.Web.UI` ad alanı. Bu sınıf içeren bir [ `Header` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.page.header.aspx) sayfanın erişim sağlayan `<head>` bölge. Kullanarak `Header` biz bir ASP.NET sayfasının başlığı ayarlayın veya ek biçimlendirme işlenmiş ekleme özelliği `<head>` bölümü. Ardından, bir içerik sayfasının özelleştirmek için mümkündür `<head>` sayfanın içinde biraz kod yazarak öğesi `Page_Load` olay işleyicisi. Programlı olarak sayfanın başlığı adım 1'de nasıl ayarlanacağı inceleyeceğiz.

Gösterilen biçimlendirme `<head>` yukarıdaki öğesi de içerir adlı ContentPlaceHolder denetiminin `head`. Özel içerik için içerik sayfalarına ekleyebilirsiniz gibi bu ContentPlaceHolder denetimi gerekli değildir `<head>` öğesi programlı olarak. Bununla birlikte, burada bir içerik sayfasını gereken static işaretleme eklemek için durumlarda yararlı `<head>` öğesi static işaretleme olarak eklenebilir bildirimli olarak karşılık gelen içerik denetimi için yerine programlı olarak.

Ek olarak `<title>` öğesi ve `head` ContentPlaceHolder, ana sayfaya ait `<head>` öğesi herhangi içermelidir `<head>`-tüm sayfalar için ortak düzeyi biçimlendirme. Bizim Web sitesinin tüm sayfaları tanımlanan CSS kurallarıyla `Styles.css` dosya. Sonuç olarak, biz güncelleştirilmiş `<head>` öğesinde [ *ana sayfalar Site genelinde düzen oluşturma* ](creating-a-site-wide-layout-using-master-pages-vb.md) karşılık gelen içerecek şekilde öğretici `<link>` öğesi. Bizim `Site.master` ana sayfa geçerli `<head>` biçimlendirme aşağıda gösterilmektedir.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>1. adım: bir içerik sayfasının başlık ayarlama

Web sayfasının başlığı aracılığıyla belirtilen `<title>` öğesi. Her sayfanın başlığı uygun bir değere ayarlamak önemlidir. Bir sayfasını ziyaret başlığını tarayıcının başlık çubuğunda görüntülenir. Ayrıca, bir sayfaya yer işareti ekleme, tarayıcılar sayfanın başlığı yer işareti için önerilen adı olarak kullanın. Ayrıca, birçok arama motorları arama sonuçlarını görüntülerken, sayfanın başlığı göster.

> [!NOTE]
> Varsayılan olarak, Visual Studio ayarlar `<title>` "Adsız Sayfa" ana sayfaya öğe. Benzer şekilde, yeni ASP.NET sayfaları sahip kendi `<title>` "Adsız sayfasına" çok ayarlayın. Sayfanın başlığı uygun bir değere ayarlamak unuttunuz kolay olabileceği için birçok sayfa var. "Adsız Sayfa" başlıklı Internet'te Bu ada sahip web sayfaları için Google arama kabaca 2,460,000 sonuçlar döndürür. Hatta Microsoft "Adsız Sayfa" başlıklı web sayfaları yayımlama açıktır. Bu yazma aynı anda bir Google araması Microsoft.com etki alanındaki 236 gibi web sayfaları bildirdi.


Bir ASP.NET sayfasının başlığı aşağıdaki yollardan birini belirtebilirsiniz:

- Doğrudan içindeki değer yerleştirerek `<title>` öğesi
- Kullanarak `Title` özniteliğini `<%@ Page %>` yönergesi
- Program aracılığıyla sayfanın ayarlama `Title` özelliğine benzer bir kod kullanarak `Page.Title="title"` veya `Page.Header.Title="title"`.

İçerik sayfaları sahip bir `<title>` şekliyle öğesi ana sayfasında tanımlanır. Bu nedenle, bir içerik sayfasının başlığı ayarlamak için kullanabilir `<%@ Page %>` yönergesi'nın `Title` özniteliği veya programlı olarak ayarlayın.

### <a name="setting-the-pages-title-declaratively"></a>Sayfanın başlığı bildirimli olarak ayarlama

Bir içerik sayfasının başlığı bildirimli olarak aracılığıyla ayarlanabilir `Title` özniteliği [ `<%@ Page %>` yönergesi](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx). Bu özellik, doğrudan değiştirerek ayarlanabilir `<%@ Page %>` yönergesi veya Özellikler penceresini aracılığıyla. Her iki yaklaşımın bakalım.

Kaynağı görünümünden bulun `<%@ Page %>` bildirim temelli biçimlendirme sayfanın üst kısmındaki yönergesi. `<%@ Page %>` İçin yönerge `Default.aspx` izler:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

`<%@ Page %>` Yönergesi ayrıştırma ve derleme sayfası ASP.NET altyapısı tarafından kullanılan sayfa özel öznitelikleri belirtir. Bu, ana sayfa dosyası, kendi kod dosyası ve diğer bilgileri arasında başlığını konumunu içerir.

Visual Studio ayarlar yeni bir içerik sayfasını oluştururken, varsayılan olarak, `Title` özniteliği "Adsız sayfasına". Değişiklik `Default.aspx`'s `Title` "Adsız sayfasından" "Ana sayfa öğreticileri için" öznitelik ve ardından sayfanın tarayıcısından görüntüleyin. Şekil 1 yeni sayfa başlığını yansıtır tarayıcının başlık çubuğunda gösterilir.


![Tarayıcının başlık çubuğunda şimdi gösterir &quot;ana sayfa öğreticileri&quot; yerine &quot;adsız sayfa&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**Şekil 01**: tarayıcının başlık çubuğunda "Adsız Sayfa" yerine "ana sayfa öğreticileri" şimdi gösterir


Sayfanın başlığı Özellikler penceresinden de ayarlanabilir. Özellikler penceresinden belge içeren yük sayfa düzeyi özellikleri için aşağı açılan listeden seçin `Title` özelliği. Şekil 2 gösterir sonra Özellikler penceresinde `Title` "Ana sayfa öğreticileri için" ayarlayın.


![Özellikler penceresini başlıktan çok yapılandırabilirsiniz](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**Şekil 02**: Özellikler penceresini başlıktan çok yapılandırabilirsiniz


### <a name="setting-the-pages-title-programmatically"></a>Sayfanın başlığı programlı olarak ayarlama

Ana sayfanın `<head runat="server">` biçimlendirme çevrilir bir [ `HtmlHead` sınıfı](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmlhead.aspx) sayfası ASP.NET altyapısı tarafından işlendiğinde örneği. `HtmlHead` Sınıfına sahip bir [ `Title` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) değeri yansıtılır işlenen içinde `<title>` öğesi. Bu özellik bir ASP.NET sayfasının arka plan kodu sınıftan erişilebilen `Page.Header.Title`; bu aynı özellik de erişilebilir aracılığıyla `Page.Title`.

Sayfanın başlığı programlı olarak ayarlama alıştırma, gitmek için `About.aspx` sayfanın arka plandaki kod sınıfı ve sayfa için bir olay işleyicisi oluşturun `Load` olay. Ardından, sayfanın başlığı kümesine "ana sayfa öğreticileri:: hakkında:: *tarih*", burada *tarih* geçerli tarihtir. Bu kod ekledikten sonra `Page_Load` olay işleyicisi aşağıdakine benzer görünmelidir:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

Şekil 3 gösterir tarayıcının başlık çubuğunda ziyaret eden `About.aspx` sayfası.


![Sayfanın başlığı programlı olarak ayarlama ve geçerli tarih içerir](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**Şekil 03**: sayfanın başlığı programlı olarak ayarlama ve geçerli tarih içerir


## <a name="step-2-automatically-assigning-a-page-title"></a>2. adım: Otomatik olarak bir sayfa başlığı atama

Adım 1'de gördüğümüz gibi bir sayfanın başlığı bildirimli olarak veya program aracılığıyla ayarlanabilir. Ancak, açıkça daha açıklayıcı bir başlık değiştirmek unutursanız, sayfanızı "Adsız Sayfa" varsayılan başlık gerekir. Biz açıkça değerini belirtmeyin gerektiğinde, ideal olarak, sayfanın başlığı otomatik olarak bize için ayarlanır. Örneğin, çalışma zamanında "Adsız Sayfa" sayfanın başlığı ise, biz ASP.NET sayfanın dosya adı ile aynı olacak şekilde otomatik olarak güncelleştirilir ve başlığının yüklü olduğu isteyebilirsiniz. İyi haber biraz ön iş otomatik olarak atanan başlık olması mümkündür sahip olmasıdır.

Tüm ASP.NET web sayfaları türetin `Page` System.Web.UI ad alanında sınıfı. `Page` Sınıfı bir ASP.NET sayfası tarafından gerekli en düşük işlevselliği tanımlar ve gibi temel özellikleri gösterir `IsPostBack`, `IsValid`, `Request`, ve `Response`, diğer birçok arasında. Görmemeleri, her bir web uygulaması sayfasında ek özellikler veya işlevsellikler gerektirir. Bu sağlama yaygın bir yolu, özel taban sayfası sınıfı oluşturmaktır. Türetilen bir sınıfı, oluşturduğunuz özel taban sayfası sınıftır `Page` sınıfı ve ek işlevler içerir. Taban sınıfı oluşturulduktan sonra bu sınıftan türetilen, ASP.NET sayfaları olabilir (yerine `Page` sınıfı), böylece, ASP.NET sayfaları için genişletilmiş işlevselliği sunar.

Bu adımda başlığı aksi açıkça ayarlanmamışsa, ASP.NET sayfa filename sayfanın başlığı otomatik olarak ayarlar bir taban sayfası oluşturun. 3. adım site haritasını kullanan sayfanın başlığı ayarı adresindeki arar.

> [!NOTE]
> Oluşturma ve özel taban sayfası sınıfları kullanma hakkında kapsamlı bir inceleme Bu öğretici seri kapsamında değildir. Daha fazla bilgi için okuma [özel taban sınıfı için bilgisayarınızı ASP.NET sayfalarının arka plan kodu sınıfları kullanarak](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Taban sayfası sınıfı oluşturma

Bizim ilk genişleten bir sınıf bir taban sayfası sınıfı oluşturmak üzere bir görevdir `Page` sınıfı. Başlangıç ekleyerek bir `App_Code` projenizi Çözüm Gezgini'nde proje adına sağ tıklayarak, ASP.NET klasörü Ekle seçerek ve ardından seçerek klasörüne `App_Code`. Ardından, sağ tıklayın `App_Code` klasörü ve adlı yeni bir sınıf ekleyin `BasePage.vb`. Şekil 4 gösterir sonra Çözüm Gezgini `App_Code` klasör ve `BasePage.vb` sınıfı eklendi.


![App_Code klasörünü ve BasePage adlı bir sınıf ekleyin](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**Şekil 04**: ekleme bir `App_Code` klasörü ve adlı bir sınıf`BasePage`


> [!NOTE]
> Visual Studio Proje yönetimi iki modlarını destekler: Web sitesi projeleri ve Web Uygulama projeleri. `App_Code` Klasör Web sitesi projesini modeliyle kullanılmak üzere tasarlanmıştır. Web uygulama projesi model kullanıyorsanız, yerleştirin `BasePage.vb` sınıf dışında bir klasörde `App_Code`, gibi `Classes`. Bu konu hakkında daha fazla bilgi için bkz [bir Web uygulaması projesine bir Web sitesi projesini geçirme](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx).


ASP.NET sayfaları arka plan kodu sınıflar için temel sınıf olarak özel taban sayfası işlevlerini yaptığından genişletmek gereken `Page` sınıfı.


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

Bir ASP.NET sayfasının istenen her bir dizi HTML'e oluşturulmakta olan istenen sayfaya sonuçlanan aşamaları boyunca devam eder. Geçersiz kılarak biz bir aşamada dokunabilirsiniz `Page` sınıfının `OnEvent` yöntemi. Bizim temeli sayfasında şimdi onu açıkça tarafından belirtilmemiş başlığı otomatik olarak ayarlamanız `LoadComplete` aşama (, tahmin olarak oluştuğu sonra `Load` aşama).

Bunu gerçekleştirmek için geçersiz kılma `OnLoadComplete` yöntemi ve aşağıdaki kodu girin:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

`OnLoadComplete` Yöntemi başlatır, belirleyerek `Title` özelliği henüz açıkça ayarlanmamış. Varsa `Title` özelliği `Nothing`, boş bir dize veya "Adsız Sayfa" değerine sahip istenen ASP.NET sayfası dosya adına atanır. İstenen ASP.NET sayfası - fiziksel yolu `C:\MySites\Tutorial03\Login.aspx`, örneğin - aracılığıyla erişilebilir durumda `Request.PhysicalPath` özelliği. `Path.GetFileNameWithoutExtension` Yöntemi yalnızca dosya adı kısmını çekmek için kullanılır ve bu dosya daha sonra atandığı `Page.Title` özelliği.

> [!NOTE]
> Başlık biçimi artırmak için bu mantığı geliştirmek için davet. Örneğin sayfanın dosya adı, `Company-Products.aspx`, yukarıdaki kod "Şirket-ürünleri" başlığı oluşturur, ancak ideal olarak tire "Şirket ürünleri" olduğu gibi bir alanı ile değiştirilmesi. Ayrıca, büyük bir değişiklik olduğunda bir alanı eklemeyi düşünün. Diğer bir deyişle, filename dönüştüren kod eklemeyi düşünün `OurBusinessHours.aspx` için bir başlık, "Mız iş saatleri".


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Taban sayfası sınıfı içerik sayfalarına sahip

Şimdi özel temel sayfasından çıkarmaya sitemizi ASP.NET sayfaları güncelleştirmek ihtiyacımız (`BasePage`) yerine `Page` sınıfı. Her arka plandaki kod sınıfı bu Git gerçekleştirmek ve sınıf bildiriminden değiştirmek için:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

Hedef:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

Bunu yaptıktan sonra bir tarayıcı aracılığıyla sitesini ziyaret edin. Başlığı açık olarak ayarlandığında, gibi bir sayfasını ziyaret edin, `Default.aspx` veya `About.aspx`, açıkça belirtilen başlık kullanılır. Ancak, varsayılan ("adsız sayfası"), başlık değiştirilmedi bir sayfasını ziyaret edin, taban sayfası sınıfı sayfanın dosya adı başlığı ayarlar.

Şekil 5 gösterir `MultipleContentPlaceHolders.aspx` sayfasında bir tarayıcıdan görüntülendiğinde. Başlık tam olarak sayfanın dosya adı (daha az uzantılı) olduğunu unutmayın "MultipleContentPlaceHolders".


[![Başlık belirtilmemiş açıkça ise, sayfanın dosya adı otomatik olarak kullanılır](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**Şekil 05**: bir başlık açıkça belirtilmezse, sayfanın dosya adı otomatik olarak kullanılan ise ([tam boyutlu görüntüyü görüntülemek için tıklatın](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>3. adım: sayfa başlığı Site Haritası alma

ASP.NET sayfası geliştiricilerinin hiyerarşik site haritası (örneğin, bir XML dosyası veya veritabanı tablosu) dış kaynak (örneğin, SiteMapPath site haritası hakkında bilgi görüntülemek için Web denetimlerle birlikte tanımlamak sağlam site haritası framework sunar Menü ve TreeView denetimleri).

Site eşlemesi yapısı, bir ASP.NET sayfasının arka plan kodu sınıfından de bir program aracılığıyla erişilebilir. Bu şekilde size otomatik olarak bir sayfanın başlığı karşılık gelen düğümü başlıkla site eşlemesinde ayarlayabilirsiniz. Şimdi geliştirmek `BasePage` bu işlevselliği sunduğunu için 2. adımda oluşturulan sınıf. Ancak önce site için site haritası oluşturmak ihtiyacımız.

> [!NOTE]
> Bu öğretici, okuyucu zaten ASP ile alışkın olduğu varsayılır. NET'in site haritası özellikleri. Site haritası kullanma hakkında daha fazla bilgi için my çok parçalı makale serisi başvurun [ASP inceleniyor. NET'in Site gezintisi](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Site Haritası oluşturma

Site eşlemesi sistem üzerinde oluşturulmuş [sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), bellek ve kalıcı depoya arasında site haritası bilgi serileştiren mantığı site haritası API ayrıştırır. .NET Framework ile birlikte [ `XmlSiteMapProvider` sınıfı](https://msdn.microsoft.com/en-us/library/system.web.xmlsitemapprovider.aspx), varsayılan site haritası sağlayıcısı olduğu. Adından da anlaşılacağı gibi `XmlSiteMapProvider` kendi site haritası depo olarak bir XML dosyası kullanır. Bizim site haritası tanımlamak için bu sağlayıcıyı kullanalım.

Başlangıç adlı Web sitesinin kök klasöründe bir site haritası dosyası oluşturarak `Web.sitemap`. Bunu başarmak için Çözüm Gezgini'nde Web sitesi adına sağ tıklayın, yeni öğe Ekle'ı seçin ve Site Haritası şablonu seçin. Dosya adlandırılır olun `Web.sitemap` ve Ekle'yi tıklatın.


[![Web sitenizin kök klasörüne birtakım adlı bir dosya ekleme](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**Şekil 06**: adlandırılmış dosya ekleme `Web.sitemap` Web sitenizin kök klasörüne ([tam boyutlu görüntüyü görüntülemek için tıklatın](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))


Aşağıdaki XML eklemek `Web.sitemap` dosyası:


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

Bu XML Şekil 7'de gösterilen hiyerarşik site haritası yapısını tanımlar.


![Site Haritasını şu anda oluşur, üç Site Haritası düğüm olması](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**Şekil 07**: Site Haritası olduğundan şu anda oluşur, üç Site Haritası düğümü


Biz yeni örnekleri ekledikçe sonraki öğreticilerde site haritası yapısı güncelleştireceğiz.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Gezinti Web denetimleri eklemek için ana sayfa güncelleştiriliyor

Biz tanımlı bir site haritası sahip olduğunuza göre şimdi Gezinti Web denetimleri eklemek için ana sayfa güncelleştirin. Özellikle, bir ListView denetimi site eşlemesinde tanımlanmış her düğüm için bir liste öğesi ile sırasız bir listesini oluşturur dersleri bölümünde sol sütuna ekleyelim.

> [!NOTE]
> ListView denetimi ASP.NET sürüm 3.5 yeni bir özelliktir. ASP.NET önceki bir sürümünü kullanıyorsanız, bunun yerine yineleyici denetimi kullanın. ListView denetimi hakkında daha fazla bilgi için bkz: [kullanarak ASP.NET 3.5'ın ListView ve DataPager denetimleri](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Varolan sırasız liste biçimlendirme dersleri bölümünden kaldırarak başlayın. Ardından, Araç Kutusu'ndan ListView denetimine sürükleyin ve dersleri bırakma başlığı. ListView araç, bir görünüm denetimleri yanında veri bölümünde bulunur: GridView, DetailsView ve FormView. ListView's ayarlamak `ID` özelliğine `LessonsList`.

ListView adlı yeni bir SiteMapDataSource denetimi bağlamak için veri kaynağı Yapılandırma Sihirbazı ' seçin `LessonsDataSource`. SiteMapDataSource denetimini hiyerarşik bir yapı site haritası sisteminden döndürür.


[![SiteMapDataSource denetimini LessonsList ListView denetimine bağlama](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**Şekil 08**: SiteMapDataSource denetimini LessonsList ListView denetimine bağlama ([tam boyutlu görüntüyü görüntülemek için tıklatın](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))


SiteMapDataSource denetimini oluşturduktan sonra biz SiteMapDataSource denetim tarafından döndürülen her düğüm için bir liste öğesi ile sırasız bir listesini oluşturur böylece ListView'ın şablonları tanımlamanız gerekir. Bu, aşağıdaki şablonu biçimlendirme kullanılarak gerçekleştirilebilir:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

`LayoutTemplate` Sırasız bir listesini için biçimlendirme oluşturur (`<ul>...</ul>`) sırada `ItemTemplate` liste öğesi olarak SiteMapDataSource tarafından döndürülen her bir öğeyi işler (`<li>`) belirli Ders bağlantısını içerir.

ListView'ın şablonları yapılandırdıktan sonra Web sitesini ziyaret edin. Şekil 9 gösterildiği gibi tek bir madde işaretli öğe, giriş dersleri bölüm içerir. Hakkında ve birden çok ContentPlaceHolder denetimleri dersleri kullanarak nerede? SiteMapDataSource hiyerarşik bir veri kümesi döndürmek için tasarlanmıştır ancak ListView denetimi yalnızca tek bir hiyerarşi düzeyi görüntüleyebilirsiniz. Sonuç olarak, yalnızca ilk düzeyi SiteMapDataSource tarafından döndürülen site haritası düğümlerinin görüntülenir.


[![Bir tek liste öğesi dersleri bölümü içerir](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**Şekil 09**: tek bir liste öğesi dersleri bölüm içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))


Birden çok düzeyi görüntülenecek biz içinde birden çok ListViews içe `ItemTemplate`. Bu teknik incelenmesi [ *ana sayfalar ve Site gezintisi* öğretici](../../data-access/introduction/master-pages-and-site-navigation-vb.md) , my [veri öğretici serisi çalışma](../../data-access/index.md). Ancak, Bu öğretici seri için bizim site haritası yalnızca iki düzey içerir: Giriş (en üst düzey); ve bir alt giriş olarak her Ders. İç içe ListView oluşturmak yerine, biz yerine başlangıç düğümü ayarlayarak değil döndürülecek SiteMapDataSource söyleyebilirsiniz kendi [ `ShowStartingNode` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) için `False`. SiteMapDataSource ikinci katman site haritası düğümlerinin döndürerek başlayacağını net etkisidir.

Bu değişikliği ListView madde işareti öğeleri hakkında için görüntüler ve kullanarak birden çok ContentPlaceHolder denetimleri dersler, ancak bir madde işareti öğesi için giriş atlar. Bu sorunu gidermek için şu açıkça bir madde işareti öğesi için giriş ekleyebilirsiniz `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

Başlangıç düğümü atlamak için SiteMapDataSource yapılandırma ve açıkça bir giriş madde işareti öğesi ekleme dersleri bölümü artık hedeflenen çıktısını görüntüler.


[![Dersleri bölüm madde öğesi, giriş ve her alt düğüm içerir.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**Şekil 10**: giriş ve her alt düğüm için madde öğesi dersleri bölüm içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Site Haritasını kullanan başlık ayarlama

Yerinde site haritası ile güncelleştiriyoruz bizim `BasePage` site eşlemesinde belirtilen başlık kullanılacak sınıfı. 2. adımda yaptığımız gibi yalnızca sayfanın başlığı sayfasında geliştirici tarafından açıkça ayarlanmamış ise site haritası düğümün başlık kullanılacak istiyoruz. İstenen sayfa açıkça ayarlanmış yoksa sayfa başlığı ve 2. adımda yaptığımız gibi biz istenen sayfanın dosya adı (daha az uzantılı) kullanmaya geri döner sonra site haritası bulunamadı. Şekil 11 bu karar işlemi gösterilmektedir.


![Bir açıkça sayfa konu başlığını ayarla olmaması durumunda, ilgili Site Haritası düğümün başlık kullanılır](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**Şekil 11**: bir açıkça sayfa konu başlığını ayarla olmaması durumunda, ilgili Site Haritası düğümün başlık kullanılır


Güncelleştirme `BasePage` sınıfının `OnLoadComplete` yöntemine aşağıdaki kodu ekleyin:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

Önceki gibi `OnLoadComplete` yöntemi başlatır sayfanın başlığı açıkça ayarlanmış olup olmadığını belirleme. Varsa `Page.Title` olan `Nothing`, boş bir dize veya bir değere kodu otomatik olarak atar sonra "Adsız Sayfa" değeri atanır `Page.Title`.

Kullanılacak başlık belirlemek için kod başvurarak başlatır [ `SiteMap` sınıfı](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx)'s [ `CurrentNode` özelliği](https://msdn.microsoft.com/en-us/library/system.web.sitemap.currentnode.aspx). `CurrentNode`döndürür [ `SiteMapNode` ](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.aspx) şu anda istenen sayfaya karşılık gelen site haritası örneği. Şu anda istenen sayfa varsayılarak site haritası içinde bulundu `SiteMapNode`'s `Title` özelliği, sayfanın başlığı atanır. Şu anda istenen sayfa site eşlemesinde değilse `CurrentNode` döndürür `Nothing` ve (2. adımda yapıldığı gibi) istenen sayfanın dosya adı'nın başlığı olarak kullanılır.

Şekil 12 gösterir `MultipleContentPlaceHolders.aspx` sayfasında bir tarayıcıdan görüntülendiğinde. Bu sayfanın başlığı açıkça ayarlanmadığından, karşılık gelen site haritası düğümün başlık yerine kullanılır.


![MultipleContentPlaceHolders.aspx sayfanın başlığı Site Haritası çekilir](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**Şekil 12**: MultipleContentPlaceHolders.aspx sayfanın başlığı Site Haritası çekilen


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>4. adım: Diğer sayfaya özgü biçimlendirme ekleme`<head>`bölümü

Adım 1, 2 ve 3 Aranan özelleştirme adresindeki `<title>` sayfa sayfa başına öğe. Ek olarak `<title>`, `<head>` bölüm içerebilir `<meta>` öğeleri ve `<link>` öğeleri. Bu öğreticide daha önce belirtildiği gibi `Site.master`'s `<head>` bölüm içeren bir `<link>` öğesine `Styles.css`. Çünkü bu `<link>` öğenin ana sayfa içinde tanımlanan, yer aldığı `<head>` tüm içerik sayfalarının bölümü. Ancak nasıl biz Git ekleme hakkında `<meta>` ve `<link>` öğeleri sayfa tarafından temelinde?

Sayfaya özgü içeriğe ekleme en kolay yolu `<head>` bölümdür ana sayfasında ContentPlaceHolder denetimi oluşturarak. Bu tür bir ContentPlaceHolder zaten sahibiz (adlı `head`). Bu nedenle, özel eklemek için `<head>` biçimlendirme, karşılık gelen oluşturmak içerik sayfasında denetimindeki ve biçimlendirme var. yerleştirin.

Ekleme özel göstermeye `<head>` biçimlendirme bir sayfaya şimdi dahil bir `<meta>` içerik sayfalarının geçerli bizim kümesine description öğesi. `<meta>` Description öğesi web sayfası hakkında kısa bir açıklama sağlar; arama sonuçlarını görüntülerken bu bilgileri herhangi bir formda çoğu arama motoru birleştirmek.

A `<meta>` description öğesi aşağıdaki biçime sahiptir:


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

Bir içerik bu biçimlendirme eklemek için yukarıdaki metin ana sayfaya ait eşleyen içerik denetimi eklemek `head` ContentPlaceHolder. Örneğin, tanımlamak için bir `<meta>` description öğesi için `Default.aspx`, aşağıdaki biçimlendirmeyi ekleyin:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

Çünkü `head` ContentPlaceHolder HTML sayfasının gövdesinde değil, içerik denetimi eklenen biçimlendirme Tasarım görünümünde görüntülenmez. Görmek için `<meta>` Açıklama öğesi ziyaret `Default.aspx` bir tarayıcı aracılığıyla. Sayfa yüklendikten sonra kaynağını görüntülemek ve unutmayın `<head>` bölüm, içerik denetimi belirtilen biçimlendirme içerir.

Eklemek için bir dakikanızı ayırın `<meta>` açıklama öğelerine `About.aspx`, `MultipleContentPlaceHolders.aspx`, ve `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Program aracılığıyla biçimlendirme ekleme`<head>`bölge

`head` ContentPlaceHolder bildirimli olarak özel biçimlendirme ana sayfaya ait eklemek verir `<head>` bölge. Özel biçimlendirme de bir program aracılığıyla eklenebilir. Sözcüğünün `Page` sınıfının `Header` özelliği döndürür `HtmlHead` ana sayfada tanımlı örneği ( `<head runat="server">`).

Program aracılığıyla içeriğe ekleme çalışabilme `<head>` bölge, içerik eklemek için dinamik olduğunda yararlıdır. Belki de sayfasını ziyaret kullanıcıya temel aldığı; belki de, bir veritabanından alınır. Bir nedenle bağımsız olarak, içeriği ekleyebilirsiniz `HtmlHead` denetimler ekleyerek kendi `Controls` koleksiyonu sözlüğüdür:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

Yukarıdaki kod ekler `<meta>` anahtar sözcükleri öğesine `<head>` bölgenin, sayfa açıklayan anahtar sözcükler virgülle ayrılmış bir listesini sağlar. Eklemeyi unutmayın bir `<meta>` oluşturduğunuz etiketi bir [ `HtmlMeta` ](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmlmeta.aspx) örneği, Ayarla kendi `Name` ve `Content` özellikleri ve ardından ekleyin `Header`'s `Controls` koleksiyonu. Benzer şekilde, programlı olarak eklemek için bir `<link>` öğesini oluşturmak bir [ `HtmlLink` ](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmllink.aspx) nesne özelliklerini ayarlayın ve ardından ekleyin `Header`'s `Controls` koleksiyonu.

> [!NOTE]
> Rastgele biçimlendirmeleri eklemek için oluşturma bir [ `LiteralControl` ](https://msdn.microsoft.com/en-us/library/system.web.ui.literalcontrol.aspx) örneği, Ayarla kendi `Text` özelliği ve ardından ekleyin `Header`'s `Controls` koleksiyonu.


## <a name="summary"></a>Özet

Bu öğreticide biz çeşitli şekillerde eklemek için arama `<head>` sayfa tarafından temelinde bölge biçimlendirme. Bir ana sayfa içermelidir bir `HtmlHead` örneği (`<head runat="server">`) bir ContentPlaceHolder ile. `HtmlHead` Örneğinin izin verdiği programlı olarak erişmek için içerik sayfalarına `<head>` bölgesini ve kullanıcının sayfası programlı ve bildirimli olarak ayarlamak için başlık; eklenecek özel biçimlendirme ContentPlaceHolder denetimi sağlar `<head>` İçerik denetimi bildirimli olarak bölümünden.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Sayfanın başlığı ASP.NET dinamik olarak ayarlama](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [ASP inceleniyor. NET'in Site gezintisi](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [HTML Meta etiketleri kullanma](http://searchenginewatch.com/showPage.html?page=2167931)
- [ASP.NET ana sayfalar](http://www.odetocode.com/articles/419.aspx)
- [ASP.NET 3.5'ın kullanarak ListView ve DataPager denetimleri](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Özel bir taban sınıf, ASP.NET sayfaları için arka plan kodu sınıfları kullanma](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar birden çok ASP/ASP.NET books ve 4GuysFromRolla.com kurucusu, 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Tan adresindeki ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog aracılığıyla [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Zack Can ve Suchi Banerjee yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Önceki](multiple-contentplaceholders-and-default-content-vb.md)
[sonraki](urls-in-master-pages-vb.md)
