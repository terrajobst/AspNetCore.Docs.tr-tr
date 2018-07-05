---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: Hangi dosyaları olarak dağıtılması gerektiğini belirleme (C#) | Microsoft Docs
author: rick-anderson
description: Dosyalarını geliştirme ortamından üretim ortamına dağıtılması için gerekenler bölümü olup ASP.NET uygulaması bize oluşturulduğuna bağlı...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: 750e2e19fdaaf2b11304b2227e7c582668d1a567
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363083"
---
<a name="determining-what-files-need-to-be-deployed-c"></a>Hangi dosyaları olarak dağıtılması gerektiğini belirleme (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> Dosyalarını geliştirme ortamından üretim ortamına dağıtılması gerekir bölümü olup ASP.NET uygulaması Web sitesi modeli veya Web uygulama modelini kullanarak oluşturulduğuna bağlı. Bu iki proje modelleri ve proje modeli dağıtım nasıl etkilediği hakkında daha fazla bilgi edinin.


## <a name="introduction"></a>Giriş

Bir ASP.NET web uygulaması dağıtma, ASP.NET ile ilgili dosyaları geliştirme ortamından üretim ortamına kopyalamayı kapsar. ASP.NET ile ilgili dosyaları içeren ASP.NET web sayfası biçimlendirme ve kodu ve istemci ve sunucu tarafı dosyalarını destekler. İstemci tarafı desteği, bu dosyaları web sayfalarınıza tarafından başvurulan ve doğrudan tarayıcıya - görüntüler, CSS dosyaları ve JavaScript dosyaları, örneğin gönderilen dosyalarıdır. Sunucu tarafı destek dosyalarını sunucu tarafında bir isteği işlemek için kullanılan içerir. Bu yapılandırma dosyaları, web Hizmetleri, sınıf dosyaları, yazılan veri kümeleri ve LINQ için diğerlerinin yanı sıra SQL dosyaları içerir.

Genel olarak, tüm istemci tarafı destek dosyalarını geliştirme ortamından üretim ortamına kopyalanacağını, ancak hangi sunucu tarafı destek dosyalarını kopyalanmasını olup, açıkça sunucu tarafı kodu bir derlemeye (bir derlemiyorsanızüzerindebağlıdır`.dll` dosyası) veya otomatik olarak oluşturulan bu derlemeleri yaşıyorsanız. Bu öğreticide, dosyaları açıkça otomatik olarak gerçekleşir. Bu derleme adım sahip karşı bir bütünleştirilmiş kod derleme yaparken dağıtılması için gerekenler vurgular.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Açık derlemesini otomatik derleme ile karşılaştırması

ASP.NET web sayfaları, bildirim temelli işaretleme ve kaynak koda ayrılır. HTML, bildirim temelli biçimlendirme bölümü içeren Web denetimleri ve veri bağlama söz dizimi; kod bölümü, Visual Basic veya C# kod halinde yazılmış olay işleyicilerini içerir. İşaretleme ve kod bölümlerini genellikle farklı dosyalarına ayrılır: `WebPage.aspx` sırasında bildirim temelli biçimlendirmesini içeren `WebPage.aspx.cs` kod barındırır.

Geçerli tarih ve saat Sayfa yüklediğinde, metin özelliği ayarlanmış bir etiket denetimi içeren Clock.aspx adlı bir ASP.NET sayfasına göz önünde bulundurun. Bildirim temelli biçimlendirme bölümü (içinde `Clock.aspx`) - etiket Web denetimi için biçimlendirme içerecektir`<asp:Label runat="server" id="TimeLabel" />` - kod bölümü sırasında (içinde `Clock.aspx.cs`) olması gereken bir `Page_Load` olay işleyicisi aşağıdaki kod ile:

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

Bu sayfa, sayfanın kod bölümü için bir isteğe hizmet vermek ASP.NET altyapısının sırayla ( `WebPage.aspx.cs` dosya) ilk olarak derlenmesi gerekir. Bu derleme, açıkça ya da otomatik olarak gerçekleşebilir.

Derleme açıkça olur sonra tüm uygulamanın kaynak kodu, bir veya daha fazla derlemeye derlenen (`.dll` dosyaları) içinde uygulamanın bulunan `Bin` dizin. Ortaya çıkan otomatik olarak oluşturulan daha sonra derleme olduğundan, varsayılan olarak, derleme otomatik olarak gerçekleşir, bulundukları `Temporary ASP.NET` konumunda bulunan dosyalar klasörü `%WINDOWS%\Microsoft.NET\Framework\`  *&lt;sürüm&gt;*, Bu konum aracılığıyla yapılandırılabilir olsa [ `<compilation>` öğesi](https://msdn.microsoft.com/library/s10awwz0.aspx) içinde `Web.config`. Açık derleme ile ASP.NET uygulama kodu bir derlemeye derlemek için bazı işlemler yapması gerekir ve bu adımı dağıtımdan önce gerçekleşir. Kaynağa erişildiğinde otomatik derleme ile derleme işlemi web sunucusunda gerçekleşir.

Bağımsız olarak hangi derleme modeli kullandığınız, tüm ASP.NET sayfaları biçimlendirme bölümü ( `WebPage.aspx` dosyaları) üretim ortamına kopyalanması gerekir. Derlemeleri kopyalamak ihtiyacınız ile açık derlemesini `Bin` klasör, ancak gerekmez ASP.NET sayfaları kod bölümlerini kopyalayın ( `WebPage.aspx.cs` dosyaları). Otomatik derleme ile kod mevcut olduğundan ve sayfanın sitesini ziyaret ettiğinizde otomatik olarak derlenebilir kod bölümü dosyaları kopyalamanız gerekir. Her bir ASP.NET web sayfası biçimlendirme bölümünü içeren bir `@Page` yönerge özelliklere sahip olan sayfanın ilişkili kod zaten açıkça derlenmiş durumdadır ya da otomatik olarak derlenmesine ihtiyacı olup olmadığını gösterir. Sonuç olarak, üretim ortamına ya da derleme modeli ile sorunsuz bir şekilde çalışabilir ve açık veya otomatik derleme kullanıldığını belirtmek için hiçbir özel yapılandırma ayarları uygulamak gerekmez.

Tablo 1, otomatik derleme ve açık derlemesini kullanırken dağıtmak için farklı dosyalar özetler. Derleme bakılmaksızın modeli kullandığınız not derlemeler her zaman dağıtmalısınız `Bin` bu klasör varsa klasörü. `Bin` Klasörünü içeren web uygulaması için belirli açık derlemesini modeli kullanılırken derlenmiş kaynak kodunu içeren derlemeler. `Bin` Dizin de içeren derlemeleri diğer projelerden ve kullanarak açık kaynak veya üçüncü taraf derlemeler ve bunlar üretim sunucusunda olmanız gerekir. Bu nedenle, bir genel kural karşısında, kopyalama `Bin` dağıtırken üretim klasöre. (Otomatik derleme modeli kullandığınız ve herhangi bir dış derlemeler kullanmıyorsunuz demektir sahip olmaz bir `Bin` Tamam dizin -!)

| **Model derleme** | **Biçimlendirme bölümü dosyasını dağıtma?** | **Kaynak kodu dosyası dağıtma?** | **Derlemeleri dağıtma `Bin` dizin?** |
| --- | --- | --- | --- |
| Açık derleme | Evet | Hayır | Evet |
| Otomatik derleme | Evet | Evet | Evet (varsa) |

**Tablo 1:** dağıttığınız hangi dosyaların derleme modelinde kullanılan bağlıdır.

## <a name="taking-a-trip-down-memory-lane"></a>Bellek yolundan bir seyahat alma

Hangi derleme yaklaşım kullanılır, kısmen ASP.NET uygulamasını Visual Studio'da nasıl yönetilir bağlıdır. Bu yana. Dört farklı sürümlerini Visual Studio - Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 ve Visual Studio 2008 olmuştur 2000 yılında NET'in başlangıcı. Visual Studio .NET 2002 ve 2003 kullanarak ASP.NET uygulamalarının yönetilen *Web uygulaması proje modeli*. Web uygulaması projesi modelinin önemli özellikleri şunlardır:

- Dosyalar düzenini proje çoklu proje dosyasında tanımlanır. Proje dosyasında tanımlı değil tüm dosyaları bölümü web uygulamasının Visual Studio tarafından kabul edilmez.
- Açık derlemesini kullanır. Proje derleme derler projedeki kod dosyaları yerleştirilen tek bir derleme içine `Bin` klasör.

Microsoft Visual Studio 2005 yayımlandığında Web uygulaması projesi modeli desteği bırakılan ve Web sitesi proje modeli ile değiştirildi. Web sitesi proje modeli kendisini aşağıdaki yollarla Web uygulaması projesi modelden Ayrıştırılan:

- Proje dosyaları kullanıma harfe dönüştüren bir tek proje dosyası bozmanın yerine dosya sistemi yerine kullanılır. Kısacası, web uygulaması klasörü (veya klasörleri) içindeki tüm dosyaları projenin bir parçası olarak kabul edilir.
- Visual Studio'da proje oluştururken bir derlemede oluşturmaz `Bin` dizin. Bunun yerine, bir Web sitesi projesi oluşturma, derleme zamanı hataları bildirir.
- Otomatik derleme desteği. Kodu önceden derlenmiş (açık derlemesini) olsa da üretim ortamına, biçimlendirme ve kaynak kodu kopyalayarak Web sitesi projeleri genellikle dağıtılır.

Microsoft, Visual Studio 2005 Service Pack 1 yayımlandığında Web uygulaması proje modeli geri. Ancak, yalnızca Web sitesi projesi modelini desteklemek Visual Web Developer devam eder. Güzel bir haberimiz var, bu sınırlama Visual Web Developer 2008 Service Pack 1 ile bırakıldı ' dir. Bugün, Visual Studio (ve Visual Web Developer) veya Web uygulaması proje modeli, hem de Web sitesi proje modeli kullanarak ASP.NET uygulamaları oluşturabilirsiniz. Her iki modelleri, kendi Artıları ve eksileri vardır. Başvurmak [Web Uygulama projeleri giriş: karşılaştırma Web sitesi projeleri ve Web Uygulama projeleri](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) iki model ve hangi proje modeli durumunuza en iyi şekilde çalışır karar vermenize yardımcı olacak bir karşılaştırma için.

## <a name="exploring-the-sample-web-application"></a>Örnek Web uygulamasına keşfetme

Bu öğretici için indirme Kitap incelemeleri adlı bir ASP.NET uygulaması içerir. Web sitesi birisi oluşturabilir hobi Web sitesi taklit eder, rehberi paylaşmak için çevrimiçi toplulukla inceler. Bu ASP.NET web uygulaması, çok basittir ve aşağıdaki kaynaklardan oluşur:

- `Web.config`, uygulamanın yapılandırma dosyası.
- Ana sayfa (`Site.master`).
- Yedi farklı ASP.NET sayfaları: 

    - ~`/Default.aspx`-site giriş sayfası.
    - ~`/About.aspx` -"bir hakkında Site" sayfası.
    - ~`/Fiction/Default.aspx` -incelendi kurgu kitapların listelendiği bir sayfa. 

        - ~`/Fiction/Blaze.aspx` -Richard Bachman Romanım incelenmesi *Blaze*.
    - ~/`Tech/Default.aspx` -incelendi teknoloji kitapların listelendiği bir sayfa. 

        - ~/`Tech/CYOW.aspx`-incelenmesi *oluşturma kendi Web siteniz*.
        - ~/`Tech/TYASP35.aspx` -incelenmesi *öğretin kendiniz ASP.NET 3.5 24 saat içindeki*.
- Üç farklı CSS stilleri klasördeki dosyaları.
- Dört bulunan - logo ASP.NET ve üç gözden geçirilmiş kitap kapsar görüntülerini tarafından desteklenen - tüm görüntü dosyaları `Images` klasör.
- A `Web.sitemap` site haritası tanımlar ve menülerde görüntülemek için kullanılan dosya `Default.aspx` kök dizininde sayfaları ve `Fiction` ve `Tech` klasörleri.
- Adlı bir sınıf dosyası `BasePage.cs` temel tanımlayan `Page` sınıfı. Bu sınıf işlevselliğini genişletir `Page` otomatik olarak ayarlayarak sınıfı `Title` site haritası sayfanın konumunu temel özellik. Buna koysalar herhangi bir ASP.NET arka plan kod sınıfı genişleten `BasePage` (yerine `System.Web.UI.Page`) başlığını site haritası içindeki konumuna bağlı olarak bir değere ayarlamanız gerekir. Örneğin, görüntülerken ~ /`Tech/CYOW.aspx` sayfası, başlığı "Giriş: teknoloji: oluşturma kendi Web siteniz için" ayarlanır.

Şekil 1 bir tarayıcıdan görüntülendiğinde Kitap incelemeleri Web sitesinin ekran görüntüsü gösterilmektedir. Burada, sayfayı görürsünüz ~ /`Tech/TYASP35.aspx`, kitap gözden geçirmeleri *öğretin kendiniz ASP.NET 3.5 24 saat içindeki*. Üst sayfa ve menü sol sütunda yayılan içerik haritası, tanımlanan site haritası yapısı dayanır `Web.sitemap`. Sağ üst köşedeki görüntü görüntüleri bulunan kitap kapak biridir `Images` klasör. Web sitesinin görünüm, geçişli stil sayfası kuralları stilleri klasöründeki CSS dosyaları tarafından ıpam'da sayfa düzeni ana sayfasında tanımlanan sırada il aracılığıyla tanımlanır `Site.master`.


[![Başlıkları kaynaklardan değerlendirmeleri gözden geçirmeleri kitap Web sitesi sunar](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**Şekil 1:** başlıkları kaynaklardan değerlendirmeleri gözden geçirmeleri kitap Web sitesi sunar ([tam boyutlu görüntüyü görmek için tıklatın](determining-what-files-need-to-be-deployed-cs/_static/image3.png))


Bu uygulama bir veritabanı kullanmaz; her gözden geçirme, uygulamada ayrı bir web sayfası olarak uygulanır. Bu öğreticide (ve sonraki birkaç öğreticileri) bir veritabanı yok. bir web uygulaması dağıtma adımlarını. Ancak, bir sonraki öğreticide biz incelemeleri, okuyucu açıklamalar ve bir veritabanı içinde diğer bilgileri depolamak için bu uygulamayı geliştirecek ve hangi adımları doğru şekilde bir veri odaklı web uygulamasını dağıtmak için gerçekleştirilmesi gereken keşfedin.

> [!NOTE]
> Bu öğreticiler, ASP.NET uygulamaları bir web ana bilgisayar sağlayıcısıyla barındırma üzerinde odaklanın ve ASP gibi ek konular keşfedin değil. NET site haritası sistemi veya bir temel kullanan `Page` sınıfı. Bu teknolojiler hakkında daha fazla bilgi ve öğretici boyunca kapsamdaki diğer konular hakkında daha fazla arka plan, her öğreticinin sonunda daha fazla bilgi bölümüne bakın.


Bu öğreticinin indirme web uygulamasının iki kopya varsa, her farklı bir Visual Studio Proje türü olarak uygulanan: BookReviewsWAP, bir Web uygulaması projesi ve BookReviewsWSP, bir Web sitesi projesi. Her iki proje Visual Web Developer 2008 SP1 ile oluşturulmuş ve ASP.NET 3.5 SP1'i kullanın. Çalışmak için bu projeleri masaüstünüze içeriği açma sırasında tarafından başlatın. Web uygulama projesi (BookReviewsWAP) açın, BookReviewsWAP klasöre gidin ve çözüm dosyasına çift tıklayarak `BookReviewsWAP.sln`. Web sitesi projesi (BookReviewsWSP) açmak için Visual Studio'yu başlatın ve ardından Dosya menüsünden Web sitesini Aç seçeneğini, Gözat `BookReviewsWSP` masaüstünüzün klasörüne ve Tamam'a tıklayın.


Bu dosyalar öğretici göz iki geriye kalan bölümlerinde uygulama dağıtılırken üretim ortamına kopyalamanız gerekir. Sonraki iki öğreticiler - *[bilgisayarınızı Site kullanarak FTP dağıtımı](deploying-your-site-using-an-ftp-client-cs.md)* ve *[dağıtma bilgisayarınızı Site kullanarak Visual Studio](deploying-your-site-using-visual-studio-cs.md)* -için farklı yollar Göster Bu dosyalar, bir web ana bilgisayar sağlayıcısına kopyalayın.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Dağıtmak için Web uygulama projesi dosyaları belirleme

Web uygulaması proje modeli açık derlemesini kullanır - projenin kaynak kodu, uygulamayı her seferinde tek bir derlemeye derlenir. Bu derleme ASP.NET sayfaları arka plan kod dosyaları içerir (~ /`Default.aspx.cs`, ~ /`About.aspx.cs`, vb.), hem de `BasePage.cs` sınıfı. Elde edilen derlemeyi BookReviewsWAP.dll olarak adlandırılır ve uygulamanın bulunan `Bin` dizin.

Şekil 2 Kitap incelemeleri Web uygulaması projesi dosyaları gösterir.


[![Çözüm Gezgini'nde Web uygulaması projesi oluşturan dosyaları listeler.](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**Şekil 2**: Çözüm Gezgini'nde Web uygulaması projesi oluşturan dosyaları listeler


Bir ASP.NET uygulamasını açıkça en son kaynak kodu bir birleştirme dosyasına derlemek için bir uygulama oluşturarak Web uygulama projesi model başlangıç kullanarak geliştirdi. Ardından, aşağıdaki dosyaları, üretim ortamına kopyalayın:

- Her ASP.NET için bildirim temelli biçimlendirme içeren dosyaları sayfasında, aşağıdaki gibi ~ /`Default.aspx`, ~ /`About.aspx`ve benzeri. Ayrıca, kopya herhangi bir ana sayfalar ve kullanıcı denetimleri için bildirim temelli biçimlendirme.
- Derlemeler (`.dll` dosyaları) içinde `Bin` klasör. Program veritabanı dosyalarını gerekmez (`.pdb`) veya XML dosyaları Bul `Bin` dizin.

Üretim ortamına ASP.NET sayfaları kaynak kod dosyaları kopyalarsınız gerekmez ya da kopyalamak zorunda `BasePage.cs` sınıf dosyası.

> [!NOTE]
> Şekil 2 gösterildiği gibi `BasePage` sınıfı projesinde, adlı klasöre yerleştirilen bir sınıf dosyası olarak gerçekleştirilen `HelperClasses`. Proje derlendiğinde, kodda `BasePage.cs` dosya ASP.NET sayfalarının arka plan kod sınıflarının yanı sıra tek bütünleştirilmiş kod içine derlenmiş `BookReviewsWAP.dll.` ASP.NET adlı özel bir klasör olan `App_Code` Web için sınıf dosyaları tutmak için tasarlanmıştır Site projeleri. Kodda `App_Code` klasörü otomatik olarak derlenir ve bu nedenle Web Uygulama projeleri ile kullanılmamalıdır. Bunun yerine, uygulamanızın sınıf dosyaları adlı normal bir klasörde koymalısınız `HelperClasses`, veya `Classes`, veya benzer bir şey. Alternatif olarak, ayrı bir sınıf kitaplığı projesinde sınıf dosyaları yerleştirebilirsiniz.


ASP.NET ile ilgili biçimlendirme dosyaları ve derlemede kopyalama yanı sıra `Bin` klasörü, ayrıca diğer sunucu tarafı destek dosyalarını yanı sıra - görüntüleri ve CSS dosyaları - istemci tarafı destek dosyalarını kopyalamak için ihtiyacınız `Web.config` ve `Web.sitemap`. Bu istemci ve sunucu tarafı açık veya otomatik derleme kullanmak isteyip istemediğinize bakılmaksızın üretim ortamına kopyalanacak dosyaları gerek destekler.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Web sitesi projesi dosyaları dağıtmak için dosyaları belirleme

Web sitesi proje modeli, otomatik derleme, bir Web uygulaması projesi modeli kullanılırken kullanılamıyor özelliği destekler. Açık derleme ile projenizin kaynak kodunu bir araya derlemek ve derlemeye üretim ortamına kopyalayın. Öte yandan, otomatik derleme ile yalnızca kaynak kodu üretim ortamına kopyalayın ve onu isteğe bağlı bir çalışma zamanı tarafından gerektiği şekilde derlenir.

Web Uygulama projeleri ve Web sitesi projeleri Visual Studio'da derleme menü seçeneği mevcuttur. Web Uygulama projeleri oluşturmayı derler projenin kaynak kodu bulunan tek bir derleme içine `Bin` dizin; bir Web sitesi projesi için derleme zamanı hataları kontrol eder, ancak herhangi bir derleme oluşturmaz oluşturma. Yapmanız gereken Web sitesi proje modeli tüm kullanılarak geliştirilmiş bir ASP.NET uygulaması dağıtma üretim ortamına uygun dosyaları kopyalama olduğu halde ilk derleme için hiçbir derleme zamanı hata olmadığından emin olmak için proje öneriyoruz.

Şekil 3 Kitap incelemeleri Web sitesi projesi dosyaları gösterir.


 [![Çözüm Gezgini'nde Web sitesi projesi oluşturan dosyaları listeler.](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**Şekil 3**: Çözüm Gezgini'nde Web sitesi projesi oluşturan dosyaları listeler


Bir Web sitesi projesi dağıtma, tüm ASP.NET ile ilgili dosyaları içeren ASP.NET sayfaları, ana sayfalar ve kullanıcı denetimleri için işaretleme sayfaları, kod dosyaları ile birlikte üretim ortamına - kopyalanmasını içerir. Ayrıca BasePage.cs gibi herhangi bir sınıf dosyası kopyalamanız gerekir. Unutmayın `BasePage.cs` dosyası `App_Code` Web sitesi projelerinde sınıf dosyaları için kullanılan özel bir ASP.NET klasörü klasörü. Özel klasör üretimde, de, sınıf dosyalarında olarak oluşturulmalıdır `App_Code` geliştirme ortamında klasörüne kopyalanmasını, için `App_Code` üretim klasöründedir.

ASP.NET işaretleme ve kaynak kod dosyalarını kopyalama yanı sıra diğer sunucu tarafı destek dosyalarını yanı sıra - görüntüleri ve CSS dosyaları - istemci tarafı destek dosyalarını kopyalamak etmeniz `Web.config` ve `Web.sitemap`.

> [!NOTE]
> Web sitesi projeleri de açık derlemesini kullanabilirsiniz. Bir sonraki öğretici açıkça bir Web sitesi projesini derlemek nasıl inceleyeceksiniz.


## <a name="summary"></a>Özet

Bir ASP.NET uygulaması dağıtma, gerekli dosyaları geliştirme ortamından üretim ortamına kopyalamayı kapsar. Eşitlenmesi gereken dosyalar kesin kümesi, ASP.NET uygulama kodu açıkça ya da otomatik olarak derlenmiş üzerinde bağlıdır. Web uygulaması proje modeli veya Web sitesi proje modeli kullanarak bir ASP.NET uygulamasını yönetmek için Visual Studio yapılandırılmış tarafından kullanılan derleme stratejisi etkilenir.

Web uygulaması proje modeli açık derlemesini kullanır ve projenin kod içinde tek bir derleme içine derler `Bin` klasör. Uygulama, ASP.NET sayfalarının biçimlendirme bölümü ve içeriğini dağıtım yaparken `Bin` klasör gerekir gönderilen üretim ortamına; - kod dosyaları ve arka plan kod sınıflar, örneğin - uygulamanın kaynak kodunda gerek yoktur üretim ortamına kopyalanacak.

Öğreticiler gelecekte göreceğiz gibi bir Web sitesi projesi derleyebilir daha mümkündür ancak Web sitesi proje modeli varsayılan olarak, otomatik derleme kullanır. Otomatik derleme kullanan bir ASP.NET uygulama dağıtımı, gerektirir, biçimlendirme bölümü *ve* üretim ortamına kaynak kodu kopyalanır. İlk kez istendiğinde kod üretim ortamına otomatik olarak derlenir.

Biz dosyaları geliştirme ve üretim ortamları arasında eşitlenmesi gereken inceleyen artık, bir web ana bilgisayar sağlayıcısı Kitap incelemeleri uygulamayı dağıtmak hazırız.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET derleme genel bakış](https://msdn.microsoft.com/library/ms178466.aspx)
- [ASP.NET kullanıcı denetimleri](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [ASP inceleniyor. NET Site gezintisi](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Web Uygulama projeleri giriş](https://msdn.microsoft.com/library/aa730880.aspx)
- [Ana sayfa öğreticiler](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Sayfalar arasında kod paylaşma](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [ASP.NET sayfalarının arka plan kod sınıfları için özel bir temel sınıf kullanma](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Visual Studio 2005'ın Web sitesi proje sistemi: nedir ve neden biz bunu?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [İzlenecek yol: Visual Studio'da bir Web uygulaması projesi için Web sitesi projesine dönüştürme](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Önceki](asp-net-hosting-options-cs.md)
> [İleri](deploying-your-site-using-an-ftp-client-cs.md)
